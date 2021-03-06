---
title: "[Unity] Rigidbody와 Rigidbody2D의 기본"
date: "2018-12-31"
---

[Rigidbody]: https://docs.unity3d.com/ScriptReference/Rigidbody.html
[Rigidbody2D]: https://docs.unity3d.com/ScriptReference/Rigidbody2D.html
[Unity Practice (Sungkuk Park)]: https://github.com/sungkukpark/unitypractice.git

Rigidbody와 Rigidbody2D의 기본적인 개념과 사용 시의 유의할 점을 소개한다.

## Rigidbody

> UnityEngine 내부 클래스, Component를 상속, UnityEngine.PhysicsModule 내부에서 구현

오브젝트의 위치(position)를 물리 시뮬레이션을 통해 제어한다.

어떤 오브젝트든 Rigidbody를 추가하는 순간 해당 오브젝트의 움직임(motion)이 Unity 물리 엔진의 제어 하에 높이게 된다. 이에 따라, 코드를 추가하지 않고도 Rigidbody 오브젝트는 중력에 의해 아래로 떨어지고 가까이 접근하는 물체와의 충돌에 반응한다. 단, Rigidbody 오브젝트와 충돌하는 물체의 경우 **Collider 컴포넌트가 반드시 부착되어 있어야 한다**.

여기서 Collider란 모든 충돌체(colliders)가 상속받는 기반 클래스(base class)이다. **만약 Collider가 추가된 오브젝트가 게임플레이 중에 움직여야(moved) 한다면, 반드시 Rigidbody 컴포넌트가 해당 오브젝트에 부착되어 있어야 한다**. 또한, 만약 사용자가 Rigidbody 오브젝트가 다른 오브젝트와 물리적인 상호작용을 하길 원하지 않는다면 해당 Rigidbody를 키네마틱(kinematic)으로 설정할 수도 있다.

스크립트 상에서, Rigidbody에 힘(force)을 가하거나 Rigidbody의 설정값을 변경하는 등의 작업은 Update 이벤트 함수 대신 FixedUpdate 이벤트 함수에 포함할 것을 권장한다. 일반적으로 Update가 갱신 작업(update task)에 주로 선호되지만, **물리 업데이트의 경우 기준이 되는 시간 간격(time step)은 프레임 업데이트(frame update)와 일치하지 않는다**. FixedUpdate는 각 물리 업데이트가 수행되기 직전에 호출되기 때문에, FixedUpdate 내부에 추가된 변경 내역은 즉시 적용된다.

일반적으로 Rigidbody를 처음 사용하는 사용자들의 불만은 게임의 물리가 "느리게(slow motion)" 실행된다는 것이다. 이는 사용자가 사용하는 모델의 스케일(scale) 때문이다. 기본 중력 설정에서 1 월드 유닛(unit)은 1 미터에 해당한다. 물리를 사용하는 게임이 아닌 이상, 사용자가 사용하는 모델의 스케일이 100 유닛이어도 아무 상관이 없다. 그러나 물리를 사용하는 게임의 경우, 이런 모델들은 초대형 오브젝트로 취급된다. 만약 원래 작아야 할 오브젝트의 크기값이 크게 설정되었으면 해당 오브젝트는 아주 느리게 추락할 것이다. 반면, 물리 엔진이 해당 오브젝트를 초대형 오브젝트로 인지하면 해당 오브젝트는 아주 빠르게 추락할 것이다. 이러한 사실을 염두에 두고 **현실(real life)에 근거해 스케일 값을 배정하도록 하라** (예컨대, 자동차는 약 4 유닛 정도 된다. 이는 4 미터에 해당한다).

## Rigidbody2D
> UnityEngine 내부 클래스, Component를 상속, UnityEngine.Physics2DModule 내부에서 구현

2D 스프라이트(sprites)를 위한 Rigidbody 물리 컴포넌트.

Rigidbody2D 클래스는 본질적으로 **Rigidbody가 3D에서 제공하는 기능과 정확히 같은 기능을 2D에서 제공**한다. Rigidbody2D를 스프라이트에 추가하면 해당 스프라이트는 물리 엔진의 제어 하에 높이게 된다. 이는 즉, 스프라이트가 중력에 의해 영향을 받으며 스크립트로부터 힘(force)을 통해 제어될 수 있다는 것을 의미한다. 적당한 충돌체(collider) 컴포넌트를 추가하면, 해당 스프라이트는 다른 스프라이트와 충돌하는 반응을 보일 것이다. 이러한 동작은 전적으로 Unity 물리 시스템이 담당한다. 때문에 **게임플레이를 위한 인상적이고 사실적인 물리 동작이 코드를 거의 추가하지 않고도 구현 가능**해진다. 이는 곧 물리 엔진을 사용하는 게임은 "**창발적인(emergent)**" 게임플레이를 제공할 수 있다는 뜻이기도 하다. **코드에서 직접적으로 표현되지는 않았지만 물리 엔진에 의해 미리 지정되지 않은 동작이나 상황이 언제든 발생할 수 있기 때문**이다.

---

원문: [Rigidbody], [Rigidbody2D]

최종 업데이트: 2018년 12월 31일
