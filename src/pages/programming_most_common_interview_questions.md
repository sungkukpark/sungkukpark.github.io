---
title: "[프로그래밍] 초초초 기초 질문 20선"
date: "2021-06-30"
---

원문: [초초초 기초 질문 20선](https://www.facebook.com/dgtgrade/posts/3654195844639255)

## 문제

1. 1바이트는 몇 비트인가요?
*) 참고로 "옛날에는..." 이렇게 대답 시작하면 무조건 통과다. 그런 수준의 체크를 하고자 하는 것이 아니다.

2. 1픽셀은 몇바이트인가요?
*) 마찬가지로 채널이 몇개냐에 따라서... 이렇게 대답하면 통과다. 이하 모든 질문 마찬가지

3. 2^10은 얼마인가요?

4. Stack과 Queue의 차이가 뭔가요?

5. Binary Tree의 시간 복잡도가 어떻게 되나요?

6. DNS의 역할이 무엇인가요?

7. HTTPS와 HTTP의 차이는 뭔가요?

8. 스마트폰 카메라 해상도가 (대강) 어떻게 되나요?

9. 왜 사진에는 JPG를 쓸까요?

10. 칼라값 `ffffff`는 무슨 색인가요?

11. `<a href>`가 무슨 뜻인가요?

12. call by reference가 무슨 말인가요?

13. Event Listener가 무슨 말인가요?

14. OOP에서 상속이 무슨 말인가요?

15. non-blocking call이 뭔가요?

16. 버전관리에서 commit이 뭔가요?

17. try/catch는 무슨 뜻인가요?

18. 디버깅 할때 breakpoint가 뭔가요?

19. 패스워드는 서버에 어떻게 보관되나요?

20. SSD가 HDD보다 빠른 이유가 뭔가요?

---

## 문제 및 해답

### 1바이트는 몇 비트인가요?

*) 참고로 "옛날에는..." 이렇게 대답 시작하면 무조건 통과다. 그런 수준의 체크를 하고자 하는 것이 아니다.

- 답은 **8 비트**이다. 1 바이트의 사이즈는 역사적으로 하드웨어 의존적이었으며 사이즈를 강제하는 확정된 기준이 존재하지 않았다. 1 바이트의 사이즈는 1 비트부터 48 비트까지 다양했다. 그러나 현재는 국제표준화기구 문서(ISO/IEC 2382-1:1993)에 명시된 대로 1 바이트의 사이즈는 **8 비트(bit)**가 사실상 표준(de facto standard)으로 받아들여지고 있다. 따라서 1 바이트의 사이즈는 사실상 **8 비트**라고 할 수 있다.

### 1픽셀은 몇바이트인가요?
*) 마찬가지로 채널이 몇개냐에 따라서... 이렇게 대답하면 통과다. 이하 모든 질문 마찬가지

- 답은 **채널의 숫자에 따라 다르다**. BW (흑백) 채널의 경우 1 픽셀의 메모리 공간 크기는 1 바이트(1 비트)이다. 컬러 채널의 경우는 RGB라는 컬러 배색을 활용하는데 R, G, B 각각이 1 바이트로서 총 3 바이트 (24 비트)의 메모리 공간을 차지한다. 따라서 **1 픽셀이 차지하는 메모리 공간은 컬러를 표현하는 채널의 숫자에 따라 다르며 흑백 채널의 경우는 1 바이트, RGB 채널의 경우는 3 바이트에 해당한다**.

### 2^10은 얼마인가요?

- 답은 **1024**이다. `^`는 거듭제곱 연산을 의미하기 때문에 2^10은 2를 10번 거듭제곱했을 때의 값, 즉, 2의 10승값이다. 2^10은 2^5 * 2^5로 표현할 수 있으므로 이는 32 * 32로 표현할 수 있다. 이를 계산한 값은 1024이다. 따라서 2^10의 값은 **1024**이다.

### Stack과 Queue의 차이가 뭔가요?

### Binary Tree의 시간 복잡도가 어떻게 되나요?

### DNS의 역할이 무엇인가요?

### HTTPS와 HTTP의 차이는 뭔가요?

### 스마트폰 카메라 해상도가 (대강) 어떻게 되나요?

### 왜 사진에는 JPG를 쓸까요?

### 칼라값 `ffffff`는 무슨 색인가요?

- 답은 **하얀색**이다. 우선, `ffffff`가 헥스 코드(Hex Color) 표기, 즉, 색상을 나타내는 16진수 코드 표기라고 할 때, 해당 표기를 세 자리로 나누면 RGB 컬러값은 각각 `ff`, `ff`, `ff`가 된다. 16진수에 해당하는 `ff`를 10진수로 변환하면 `f`는 10진수로 15이기 때문에 `ff`의 변환값은 다음과 같다: 16^1 * 15 + 16^0 * 0 = (16 * 15) + 1 * 15 = 240 + 15 = 255. 따라서 RGB의 각 자리는 255가 된다. RGB의 10진수 표현은 가장 어두운 색인 0에서 가장 밝은 색인 255까지 범위를 가지며 RGB (255, 255, 255) 값에 해당하는 색은 RGB 색상 범위 내에서 가장 밝은 색인 하얀색이다. 따라서 헥스 코드 컬러값 `ffffff`는 **하얀색**을 표현한 값이다.

### `<a href>`가 무슨 뜻인가요?

우선 `<a href>`가 HTML 태그라고 전제할 때, `a`는 anchor 요소(element)라고 부르며 하이퍼링크(hyperlink)를 정의하는 태그에 해당한다. 하이퍼링크란 하나의 페이지에서 다른 페이지를 연결해주는 것을 의미한다. `href`는 이 링크의 목적지를 지정할 때 쓰이는 어트리뷰트(attribute)로서 다음 예제와 같이 사용할 수 있다. `<a href>` 태그는 여는 태그로서 닫는 태그인 `</a>`로 태그를 완결시켜야 한다는 점에 주목하자.

```html
<a href="http://twitter.com/">여기</a>를 누르시면 트위터 페이지로 연결됩니다
```

따라서, `<a href>`는 **HTML 태그의 일종으로서 anchor 요소에 href 어트리뷰트를 더해 하이퍼링크 기능을 제공하는 태그**이다.

### call by reference가 무슨 말인가요?

### Event Listener가 무슨 말인가요?

### OOP에서 상속이 무슨 말인가요?

### non-blocking call이 뭔가요?

### 버전관리에서 commit이 뭔가요?

### try/catch는 무슨 뜻인가요?

### 디버깅 할때 breakpoint가 뭔가요?

### 패스워드는 서버에 어떻게 보관되나요?

### SSD가 HDD보다 빠른 이유가 뭔가요?
