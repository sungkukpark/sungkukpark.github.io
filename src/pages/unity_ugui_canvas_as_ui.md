---
title: "[유니티] UI로서의 캔버스(Canvas)"
date: "2020-05-20"
---

캔버스(Canvas)는 모든 UI 요소가 배치되는 영역(area)이다. 캔버스는 Canvas 컴포넌트를 가진 게임오브젝트로서 **모든 UI 요소는 반드시 캔버스의 자식이어야 한다**.

GameObject > UI > Image 메뉴를 통해 Image와 같은 UI 요소를 생성할 때, 씬(scene) 내에 캔버스가 존재하지 않으면 자동으로 새로운 캔버스가 생성된다. 그리고 해당 UI 요소는 새로 생성된 캔버스의 자식으로 생성된다.

캔버스는 씬 뷰(Scene View)에서 직사각형으로 표시된다. 이는 게임 뷰(Game View)에서 매번 눈으로 확인하지 않고도 UI 요소를 찾을 수 있도록 지원하기 위해 만들어진 것이다.

캔버스는 EventSystem 오브젝트를 통해 유니티의 메시징 시스템을 지원한다.

> 역주: EventSystem 컴포넌트를 가진 오브젝트가 존재하지 않으면 캔버스 내의 UI 요소는 입력을 받을 수 없다.

## 캔버스 내 요소가 그려지는 순서(Draw order)

**캔버스 내의 UI 요소는 하이어아키(Hierarchy)에 배치된 순서대로 그려진다**. 첫 번째 자식이 먼저 그려지고, 두 번째 자식이 그 다음으로 그려진다. 만약 UI 요소 두 개가 겹치면 하이어아키에 배치된 순서를 기준으로 보다 뒤쪽에 있는 요소가 그 앞쪽에 있는 요소 위에 그려진다.

어떤 요소가 먼저 올지 변경해야 할 때는 드래그-앤-드롭으로 하이어아키 내 요소를 재정렬한다. 해당 순서는 Transform 컴포넌트 내 다음 메소드를 통해 제어할 수 있다: `SetAsFirstSibling`, `SetAsLastSibling`, `SetSiblingIndex`.

## 렌더 모드(Render Modes)

각 캔버스는 해당 캔버스를 스크린 공간(screen space)에 그릴지 월드 공간(world space)에 그릴지를 지정하는 렌더 모드를 가지고 있다.

### 스크린 공간 - 오버레이(Screen Space - Overlay)

스크린 공간 - 오버레이 렌더 모드는 화면 내의 UI 요소를 **스크린 위에 그린다**. 스크린의 크기나 해상도가 변경되면 캔버스는 이에 맞춰 그 자신의 사이즈를 변경한다.

![UI in screen space overlay canvas](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/images/GUI_Canvas_Screenspace_Overlay.png)

### 스크린 공간 - 카메라(Screen Space - Camera)

이 렌더 모드는 스크린 공간 - 오버레이 렌더 모드와 유사하지만 이 렌더 모드에서 캔버스는 특정 카메라에서 주어진 거리만큼 떨어진 곳에 배치되게 된다. UI 요소는 이 카메라에 렌더되고 이는 카메라의 설정값에 의해 캔버스가 UI가 영향을 받는다는 것을 의미한다. 즉, 카메라가 원근(Perspective) 투영 모드일 경우에는 UI 요소들도 원근감을 갖고 그려질 것이고, 이 투시에 따른 왜곡은 해당 카메라의 화각(FOV, Field of View)에 의해 제어될 것이다. 카메라의 크기가 변경되거나 해상도가 변경되거나 카메라의 절두체(frustum)가 변경되면 캔버스 역시 자동으로 그 사이즈를 변경할 것이다.

![UI in screen space camera canvas](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/images/GUI_Canvas_Screenspace_Camera.png)

### 월드 공간(World Space)

이 렌더 모드에서는 캔버스가 씬 내의 다른 오브젝트인 것처럼 동작하게 된다. 캔버스의 사이즈는 RectTransform에 의해 수동으로 지정될 것이며 UI 요소는 씬 내 3D 배치에 기반해 다른 오브젝트의 앞이나 뒤에 그려질 것이다. 이는 월드의 일부가 되어야 하는 UI 요소를 표현하는데 유용하다. 이처럼 월드의 일부가 되는 UI 요소를 "다이어제틱 인터페이스(diegetic interface)"라고도 부른다.

![UI in world space canvas](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/images/GUI_Canvas_Worldspace.png)

---

원문: [Canvas | Unity UI | 1.0.0](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/UICanvas.html)
