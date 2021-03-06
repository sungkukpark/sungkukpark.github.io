---
title: "[게임디자인] 라이엇이 불확실한 시스템에 대처하는 전략"
date: "2021-07-05"
---

[STRATEGIES FOR WORKING IN UNCERTAIN SYSTEMS]: https://technology.riotgames.com/news/strategies-working-uncertain-systems

원문: [STRATEGIES FOR WORKING IN UNCERTAIN SYSTEMS]
원문 작성일자: 2021년 4월 27일

다음 상황에 당신이 놓였다고 상상해보라.

당신은 컴퓨터 앞에 앉아 업무를 시작하기 위해 준비하고 있다.
You sit down at your computer, ready to begin the work day. A steaming cup of coffee rests beside your keyboard and your next task is on the monitor in front of you. You start to write a program that must hit a third party URL over the internet every time you run it. The hours pass and you feel content with the progress you’ve made so far.  

Suddenly things start breaking. You debug for an hour and you’ve collected some data: a dozen crashes, a handful of different HTTP error codes, several uncaught exceptions, and at least one 60 second timeout. Your requests only get through half the time and you’re getting responses back that don’t match the little documentation you could find. As you twirl the lock of hair you just yanked out of your head, you wonder: What happened?

My name is Brian "Bizaym" Teschke. I'm a software engineer on a central technology team at Riot Games called Developer Connections and we’ve seen all kinds of system instability issues. My team is responsible for solving common problems across game teams so they can just focus on making awesome games. We often have to write programs that interface with several APIs and libraries - both internal and external - each with their own quirks and inconsistencies. This has allowed us to see patterns of both issues and resolutions, which I’m looking forward to outlining for you in this post. 

Ultimately, I hope to answer this question:

How can I hope to write a system more reliable than the services that power it?

In this article, I'll explain some tips and tricks my team has used to overcome or avoid cases like the one above, and how we figure out why they happen in the first place. I'll be taking the perspective of working with a third-party's REST API for most of my examples, but these techniques can also be applied to a library package that interfaces with an online service. For each category of issue, I’ll provide a short example, a breakdown of common causes I’ve run into, and the solution for my example that applies these learnings.

SYSTEM INSTABILITY


EXAMPLE: SLOW BUILD TRANSFERS
I once worked with a service we used for processing builds of our game binaries, the first step of which was to transfer the build (often large files of +3GB) - we would trigger workflows several times a day. For the most part, my files would transfer fine, but sometimes it would run 10 times slower, and I would frequently get errors, usually near the end of my workday. Luckily, it was always fast on Fridays so I could get off on time for the weekend. But why? 

PATTERN BREAKDOWN
It’s very hard to keep a service running perfectly forever, and much easier to have one that only mostly works. You might see a timeout, an HTTP error code, or maybe a friendly “Try again later” message. There are many causes of outages - some of them completely out of the developer’s control, such as a hardware failure or bandwidth saturation. Sometimes the changes are intentional and planned, like an update during a maintenance window or the rollout of a new version.

While still mid-development, it’s a good idea to save at least one good response from a dependent service and use those in case of an outage. Continue working on your program and use the static response to ensure that the code works until the real service is back up. Pro tip: put this in a folder called Unit Tests and run it constantly. Your peers and future self will thank you.  

This works when you’re still developing your program, but what if this happens in production?

Typically the solution most often proposed for intermittent service errors is to simply retry the request. Maybe it’ll work next time, right? This works for short term failures... but what if it's a longer outage? Hopefully you implemented an exponential backoff so you aren’t just hammering your third party while they try to come back online! Maybe there’s an underlying reason why this outage happens. Do some digging and see if you can find a pattern.

SOLUTION
In the story above, I took my unreliable upload scripts and went looking for reasons why it was slow at specific times of the day. It turns out that the external service I was using had a lot of customers from Asia that would use their systems early in the morning which, with the time zone difference, is around late afternoon on the US Pacific Coast. My Friday would be their Saturday, so there wouldn’t be any peak traffic then! I scheduled all my upload tasks to run outside those peak times and error rates went way down.

CHANGING FUNCTIONALITY


EXAMPLE: DATETIME STRUGGLES
My team at Riot was working on a service that allowed game teams to run test scenarios on remote hardware. The third-party company hosting the remote hardware was also in the process of developing improvements to their solution to meet Riot's unique needs. There wasn't a lot of rigor around communicating API updates, so we would often test a feature in the morning that would hit breaking changes in the afternoon. I once experienced an odd crash when parsing what should have been a standard datetime string but instead got this interesting (but not necessarily incorrect) response:

{
  ...
  "starttime": "2021-02-10T00:28:58+00:00"
  "endtime": "Processing..."
  ...
}
 
I supported many different date formats but that was not a response I was expecting.

PATTERN BREAKDOWN
We’ve covered the case where a service goes down, but what about when a service changes unexpectedly? It could be that the endpoints are moved around on you, the request structure changes, or even the data types are now different. This happens most often while using a system still under active development, like a pre-release version. Take a look at the packages in your dependency tree that have a version less than 1.0. You might be surprised by how many there are!

Sudden changes can happen often with APIs that already have a frontend offering (such as a website or app) controlling them. This is because the owner can release changes to both the frontend and backend to ensure breaking changes aren’t seen by the typical user.  Except maybe you’re not a typical user and you are hitting the backend directly and those breaking changes now affect you. So what can you do?

The first step is to set up some tests that will show what’s breaking. Run them automatically during your off hours and you might have a list of To Dos when you start work in the morning... instead of finding out while you’re in the zone, where they can derail your train of thought. These kinds of tests are especially important if you run a production service and have customers of your own you need to support. Finding the problems before they do and informing them with a warning message or banner can go a long way to improving the relationship and confidence your customers have in you and your systems.

Change is inevitable, and if it’s frequently breaking your code, you might want to think about setting up some mechanisms to reduce its impact. The adapter pattern is a great fit. Write a single file that handles all the communication between this problematic third-party API and the rest of your system.

This has several benefits:

You only have to fix bugs in one file if there are changes.

You can capture any failures here so that they don’t bring down your entire system.

It keeps your system pristine, consistent, and isolated from the changes out of your control.

Keep a clean separation by inspecting all information coming in from the external API; don’t make blind assumptions and pass around nested objects or you risk introducing bugs all over your code. Use the objects that make the most sense in your language and environment (i.e. a built-in timestamp class) and make the necessary translations to or from the third-party data (i.e. from a date formatted string) in your adapter if necessary.

You can get even more power by applying the strategy pattern on top of your adapters.  Ask yourself if there are other APIs out there that suit your needs. Write an adapter in front of those APIs too and switch between them when one isn’t working. You give your system the flexibility to stay operational while you prioritize the breaking updates with your other critical work. This opens the door for adding more potential benefits to your application, such as cross-referencing your data, managing requests around rate-limits, or replacing a paid service with multiple free offerings.

Sometimes a breaking change is a feature you weren’t expecting. For example, if you suddenly get an array when you expect a single object, it could be an enhancement that allows the caller to query multiple records at once and the single object response is just the simplest case. Ensure your code can handle requests that could return one or more objects.

SOLUTION
Once in a while you just run into an edge case that the original author simply hadn't thought about. In my example above, I discovered from the developer that there was a window between when the task completes and when their backend updates their database. Which is where my status call could sneak in and get a stale value (ever heard of something being eventually consistent?). This field was left as an unexpected default value when it was returned to me. I made my adapter functions more fault-tolerant. It now catches cases like this where data from an API is missing or malformed, but a default value (like null) will now be handled just fine in the rest of my program.

CONFLICTING DOCUMENTATION


EXAMPLE: OUTDATED DOCS
What about when it's not the service changing that causes you frustration but that the user guide you're following seems to conflict with reality? 

I once had a colleague approach me about a library I owned that could parse a project file into a data structure that was easier to work with in a program. This colleague was particularly irritated at me for how the documentation seemed to make no sense and contradicted itself. Parts of it referenced features that didn't seem to exist, arguments were incorrect, and even some examples were using a different programming language.

PATTERN BREAKDOWN
Documentation is often one of the last steps before releasing a service. During development, you don’t want to go back and update the docs every time you have to make changes. Unfortunately, this also means it’s often an area where corners are cut to meet deadlines, and it’s one of the last places to get updated as newer versions are rolled out. It could have errors in its details, completely undocumented features, or have been poorly translated from another language. 

On the opposite end of the spectrum, you could be looking at enormous amounts of text covering every change on every microversion of the platform. Your searches could yield millions of results over several years across a wide community and you'll find it impossible to identify a solution to your problem that works in your specific set of circumstances.

Try to reach out to the author if possible. Submit a support ticket, open an issue on Github, join the company’s public Slack channel, or send a good old-fashioned email. Besides simply answering your questions, they could give you some insight into their project roadmap. You might be the only user of a feature they thought about dropping and now you can influence its priority. They could have newer, unpublished documentation or a beta version of the service they can give you access to use. In some cases you might even be able to help contribute to the project for the benefit of everyone.

And if that doesn’t work, and you’re stuck with the original documentation? See if you can do some exploratory investigation. Try a GET request on as many relevant nouns you can think of.  Maybe you’ll get lucky and find a crucial model’s data structure. Can't figure out what fields a search query for/transactions expects? Try a GET on /item, /sku, /store, /employee> and see what fields are available on them.

PUT /person not working? Try a POST instead if other models are using that method.  Developers tend to be consistent, so follow the patterns in functionality. Other endpoints use plural nouns?  Try /persons or /people. There are a lot of words that mean the same thing that are not necessarily standard programming concepts, and this can be particularly confusing for an author whose first language is not English.  Instead of person, it could be user, customer, client, or even shopper.

Great developers often build solutions that cover a wider range of problems than asked for. The program’s requirements only ask for a single record, but we know it’s better to process things in bulk when we can, so maybe you’re passing in a single user where the service expects an array instead. Put yourself in the shoes of the author and imagine how you would write this feature. Search the internet for popular implementations for, as an example, a login endpoint. There’s a good chance the author did something similar and this can shed some light on how you need to pass your credentials.

SOLUTION
It so happened that my project was based off an abandoned open source project that itself was also based off an older open source project from another language. I had also merged in some new features from other public forks of the old project, and in doing so, completely scrambled the documentation folder with bits from all contributors. I took some time to clean up the different parts that were merged together, updated the parts where I added new features, and translated the old parts into the new language.

CONCLUSION
Let’s face it. None of us are perfect developers. As good as our intentions are to build the best solutions we can, we sometimes fall short of our own expectations. You may be thinking of some systems you yourself have written that you wish you had more time to go back and improve. It’s the reality of the world we live and work in.

Remember that every challenge is an opportunity in disguise. I hope these tips from the kinds of problems I’ve seen over the years will help you crush those bugs before they even appear in your code, level up your craft, and boost your customer's experience. 

Look towards long-term solutions instead of quick fixes and you end up saving more time.  Robust solutions often grow their own influence, by being the shining example for future features or being that solid foundation for yet another young developer to rely on for their next great project.

Key strategies:

Write unit tests using static responses to keep you working when systems are down.

Rely on the adapter pattern to keep external changes isolated.

Don't be afraid to reach out if the documentation seems iffy.

The next time you run across an API or third-party plugin that frustrates you, remember that it was written by a programmer just like yourself. Forgive the quirks and inconsistencies of the author’s code and remember that you have techniques in your back pocket to help you get through this.

Thanks for reading! If you have any questions, feel free to post them below.

Posted by Brian Teschke