---
title: "[Maya] Maya 모델링을 위한 레퍼런스 이미지 설정 방법 (번역)"
date: "2021-01-24"
---

모델링에 필요한 것은 좋은 레퍼런스다. 본문은 이미지 플레인(image plane)을 사용해 레퍼런스를 설정하는 방법을 기술한다.

[![Maya Quick Tip: Setting up background images for modeling reference](https://img.youtube.com/vi/DXJJKX1fBcs/0.jpg)](https://www.youtube.com/watch?v=DXJJKX1fBcs)

이미지를 Maya로 가져오는 방법은 여러 가지가 있겠지만, 레퍼런스용이라면 프리 이미지 플레인(free image plane)을 사용하는 방법이 단연 최고의 방법이다.

1. Create > Free Image Plane 메뉴를 통해 프리 이미지 플레인을 하나 생성한 측면(side) 레퍼런스 이미지를 불러온다. 여기서 이미지는 Attribute Editor > Image Plane Attributes > Image Name 메뉴를 통해 지정할 수 있다
1. 크기를 늘리거나 줄이고 원하는 위치에 맞게 이동시키고 회전시킨다. 여기서 `J` 단축키를 누르면서 이동시키거나 회전시키면 각도나 그리드에 맞게 스내핑(snapping) 된다. 크기를 늘리거나 줄일 때는 중앙 핸들(handle)을 사용해 레퍼런스 이미지가 왜곡되지 않도록 주의한다
1. 같은 과정을 전면(front) 레퍼런스 이미지에도 적용한다
1. 레퍼런스 이미지의 특징들(features)이 사이드와 전면 레퍼런스 두 이미지 플레인 사이에 제대로 정렬되어 되어있는지 확인한다. 이를 확인하기 위해 플레인(plane) 오브젝트를 사용할 수도 있고, 필요하다면 프리 이미지 플레인을 조정할 수도 있다
1. 내 경우에는 Maya에서 제공하는 이미 존재하는 메쉬를 배치하고 이를 변형하는 식으로 작업하는 것을 선호한다. 예컨대, 인간의 두상을 불러오고 싶으면 Content Browser Windows > General Editors > Content Browser로 이동해 해당 메쉬를 불러오면 된다
1. 레퍼런스 이미지를 메쉬에 맞게 정렬하고 크기를 늘리거나 줄인다. Show > Image Planes를 비활성화시키는 방식으로 최종 결과물만 따로 볼 수도 있다

레퍼런스 플레인을 설정한 다음에는 이를 선택한 뒤 Display 타입이 Reference로 설정된 새 레이어를 생성한다. 이런 식으로 해당 레이어에 속하는 레퍼런스 플레인은 선택할 수 없게 되고 쉽게 켜거나 끌 수 있게 된다

이로써 우리는 모델과 안전하게 분리된 모델 레퍼런스 이미지를 설정하는 법을 배웠다.

## 참고 자료

- [Maya Quick Tip: Setting up reference images for modeling | Tutorials | AREA by Autodesk](https://area.autodesk.com/tutorials/maya-quick-tip-setting-up-a-reference-image-for-modeling/)
