---
layout: post
title: Project Sanabi 2. 1주차 진행사항
date: 2025-04-07 09:00:00 +0900
category: Project_Sanabi
---

## 2. 1주차 진행사항

1. Editor UI
2. PhysX 충돌 이벤트 구조
3. FSM 동작 구조
0. 작업 목록


---

<br><br>

>### 1. Editor UI

 - 모든 에셋의 파일 저장/불러오기 구현 완료.

 - 레벨 생성, 오브젝트 생성 및 배치, 컴포넌트 추가 기능 구현 완료.

 - 기존 Collider2D 에서 담당하던 충돌 및 이벤트 처리를 전부 PhysX 기능으로 대체함.

 - 새로운 에셋이나 타일 오브젝트를 생성하는 기능이 미흡함 - 2주차에 시작 및 스테이지 레벨을 제작하면서 보충 예정.

 - FSM 의 구조 설계 완료 - 실제 컴포넌트로 구현 예정.


---

<br><br>

>### 2. PhysX 충돌 이벤트 구조


![alt text](\public\img\PhysXActor_CollisionEvent.png)

- 충돌 이벤트 함수는 PhysX Actor 컴포넌트에서 함수 포인터로 입력받아 저장하고, 실제 구현은 스크립트에 한다.

- GameObject 가 PhysX Actor 컴포넌트와 스크립트를 모두 보유하면, 스크립트의 Tick 호출 시 함수를 PhysX Actor 컴포넌트에 최초 1회만 등록한다.

---


<br><br>

>### 3. FSM 동작 구조


![alt text](\public\img\FSM.png)

- State 는 오브젝트의 특정 상태를 정의하는 추상 클래스이며, int, float, Vec4 등 여러 타입의 변수를 vector 컨테이너로 관리한다.

- 매 프레임마다 할 일을 정의하는 Tick 함수가 인터페이스로 있으며, 필요시 Begin/End 함수를 override 해 상태 전환 시 할 일을 정의할 수 있다.

- FSM Component 는 State 전환 조건인 Trigger 함수와 대상 State 객체 2개를 하나의 구조체로, vector 컨테이너로 관리하며, FinalTick 시점에 모든 Trigger 를 순차적으로 검사한다.

- 스크립트에서 구현된 Trigger 함수를 스크립트의 Tick 에서 등록받아 저장한다.

---

<br><br>

>### 0. 작업 목록

> **Object**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|Object|오브젝트를 레벨에 추가하는 기능|✅ Completed|
|Object|오브젝트와 프리펩 간 전환|✅ Completed|
|Object|Object 데이터 및 컴포넌트 저장 및 불러오기|✅ Completed|

<br>

> **Asset**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|모든 Asset|파일 저장 및 불러오기|✅ Completed|
|Mesh|프리뷰 이미지에서 드래그로 프리뷰 카메라를 조작하는 기능|✅ Completed|
|Graphic Shader|셰이더의 각종 옵션(토폴로지, 렌더 도메인, 각 스테이트)을 변경하는 기능|✅ Completed|
|Compute Shader|컴퓨트 셰이더의 속성(텍스쳐 또는 상수 데이터) 조정 기능|❌ Incomplete|
|Prefab|기존 오브젝트를 프리펩으로 만드는 기능|✅ Completed|
|Prefab|프리펩을 레벨에 오브젝트로 배치하는 기능|✅ Completed|
|Sprite|새로운 스프라이트를 생성하거나 기존 스프라이트를 편집하는 기능|❌ Incomplete|
|Flipbook|플립북에 씬을 추가하거나 제거하는 기능|✅ Completed|
|Level|레벨을 새로 생성하고 오브젝트를 추가하는 기능|✅ Completed|
|State|FSM 컴포넌트와 연계|❌ Incomplete|

<br>

> **Component**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|모든 Component|컴포넌트 저장 및 불러오기|✅ Completed|
|모든 Component|컴포넌트를 오브젝트에 추가하는 기능|✅ Completed|
|Collider2D|컴포넌트 제거 - 충돌 관련 기능 PhysX로 통합|✅ Completed|
|PhysX|액터의 각 물리적 속성을 편집하는 기능|✅ Completed|
|PhysX|충돌 이벤트 콜백|✅ Completed|
|FlipbookRender|플립북의 속성을 편집하는 기능|✅ Completed|
|FSM|각 상태와 상태 전환 조건을 편집하는 기능|❌ Incomplete|
|FSM|State 스크립트를 FSM 에 등록하는 기능|❌ Incomplete|
|Button|버튼에 연결할 이벤트 함수를 스트립트로부터 설정하는 기능|❌ Incomplete|

<br>

> **기타 작업**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|CodeGen|코드 생성기 추가 - 스크립트 라이브러리의 스크립트 등록 함수 작성 자동화|✅ Completed|
|Level|레벨 저장/불러오기/복사 및 상태 전환 구현|✅ Completed|
|Clone|복사 생성자 구현|✅ Completed|
|TileRender|타일 렌더링 구조 변경|✅ Completed|
|Transform|회전 방식을 오일러 회전에서 쿼터니언 회전으로 변경|✅ Completed|
|AssetMgr|에셋 타입을 지정하여 파일로부터 직접 로드 해 주는 함수 추가|✅ Completed|
|AssetMgr|FindAsset 을 Load 로 대체|❌ Incomplete|


<br>

> **현재 확인된 문제**

|오류 대상|오류 내용|해결 현황|
|---|---|---|
|Particle|Play/Stop 전환 중 파티클이 비정상적으로 생성되는 현상 있음|❌ Incomplete|
|Particle|m_MaxParticle 값이 같을 때 이전 파티클 데이터가 남아있음|❌ Incomplete|
|Transform|쿼터니언 회전 문제 - y축 회전 시 90도에서 매끄럽게 전횐되지 않음|❌ Incomplete|
|ImGui|다른 PC 에서 실행 시 UI 렌더링에 오차 발생|❌ Incomplete|
|PhysX|레벨 Play 도중 Level 변경 시 삭제된 이전 레벨의 오브젝트 충돌 ContactEnd 호출됨|❌ Incomplete|


---