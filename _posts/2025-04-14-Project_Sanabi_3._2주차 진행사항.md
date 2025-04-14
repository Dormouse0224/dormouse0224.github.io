---
layout: post
title: Project Sanabi 3. 2주차 진행사항
date: 2025-04-14 09:00:00 +0900
category: Project_Sanabi
---

## 3. 2주차 진행사항

0. [작업 목록](#0-작업-목록)
1. [Editor UI](#1-editor-ui)
2. [FSM Editor](#2-fsm-editor)
3. [Tile, Collider Editor](#3-tile-collider-editor)
4. [UI 오브젝트](#4-ui-오브젝트)


---

<br><br>

1주차에서 미진했던 에디터의 보충 제작으로 인해 시간이 소요되어 아직 플레이어 / 몬스터 제작을 시작하지 못함.

다만, 스테이지 관련 에디터는 구현이 끝나 각 레벨은 제작만 하면 됨. (텍스트 렌더링 오류는 해결 필요)

3주차는 일단 플레이 가능한 데모 스테이지를 제작하는 것을 목표로 한다.



---

<br><br>

>### 1. Editor UI

 - Material, Flipbook 을 ImGui를 통해 생성 가능


---

<br><br>

>### 2. FSM Editor

![alt text](\public\img\FSM_Editor.png)

 - Engine 라이브러리를 사용하는 외부 라이브러리에서 State 클래스를 상속받아 상태를 스크립트처럼 구현하고 추가할 수 있도록 함.

 - CodeGen 코드 생성기를 이용해서 자동으로 등록용 클래스가 만들어지고, 클라이언트 실행 시 엔진에 등록되어 사용할 수 있음.

 - 상태 변경 조건 Trigger 함수 또한 CodeGen 에서 함수 구조를 분석 후 구조가 일치하는 함수를 포인터로 넘겨 엔진에 등록시킴.

 - 엔진에서는 ImGui를 통해 등록받은 State 와 상태 변경 조건 Trigger 함수를 조합해서 사용할 수 있음.


---

<br><br>

>### 3. Tile, Collider Editor

![alt text](\public\img\Tile_Editor.png)

 - Tile 크기와 해당 타일맵에서 사용하는 아틀라스 이미지를 고르고, 에디터를 통해 타일 스프라이트를 선택해서 브러시처럼 타일을 배치할 수 있음.

![alt text](\public\img\Collider_Editor.png)

 - 충돌체의 속성(시뮬레이션/쿼리/트리거) 설정 및 충돌 대상 레이어 선택, 크기 와 오프셋을 입력받아 강체에 추가할 수 있음


---


<br><br>

>### 4. UI 오브젝트

![alt text](\public\img\UI_Object.png)

 - FSM과 비슷한 구조로, 외부 라이브러리에서 스크립트로 구현된 이벤트 함수를 등록받아 클릭/키다운 시 해당 이벤트 함수를 호출해 준다.

 - 단, ImGui를 통해 선택하는 것이 아닌 스크립트 쪽에서 초기에 등록시켜야 한다.

 - 스크립트에서는 UI의 상태를 받아와 추가적인 동작을 정의할 수 있다. (클릭 시 텍스쳐 변경 등)

 - 폰트와 문자열을 입력받아 텍스트를 렌더링할 수 있다.

---

<br><br>

>### 0. 작업 목록

> **Editor**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|Material|Material을 새로 생성하는 기능|✅ Completed|
|Flipbook|Flipbook 생성 및 오브젝트 추가 기능|✅ Completed|
|FSM|State 클래스와 Trigger 함수로 구현|✅ Completed|
|State|FSM 컴포넌트와 연계|✅ Completed|
|UI|클릭 및 키다운 상호작용 이벤트 구현|✅ Completed|
|UI|UI에 연결할 이벤트 함수를 스트립트로부터 설정하는 기능|✅ Completed|
|Font & UI|폰트 파일로 입력받은 텍스트를 UI 위치에 출력하는 기능|✅ Completed|
|TileRender|타일 편집기|✅ Completed|
|PhysX|충돌체를 추가/편집/제거하는 기능|✅ Completed|

<br>

> **Level**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|Level|시작 레벨 만들기|⏳ In Progress|
|Level|스테이지 만들기|⏳ In Progress|

<br>

> **Player/Monster**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|Player|기본 이동 구현 (이동, 점프, 벽 점프, 등반)|⏳ In Progress|
|Player|로프 액션 구현|❌ Incomplete|
|Monster|Player 와 공격 상호작용|❌ Incomplete|

<br>

> **기타 작업**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|AssetMgr|FindAsset 을 Load 로 대체|✅ Completed|
|AssetMgr|Content 자동 로딩 및 동기화|✅ Completed|



<br>

> **현재 확인된 문제**

|오류 대상|오류 내용|해결 현황|
|---|---|---|
|Particle|Play/Stop 전환 중 파티클이 비정상적으로 생성되는 현상 있음|❌ Incomplete|
|Particle|m_MaxParticle 값이 같을 때 이전 파티클 데이터가 남아있음|❌ Incomplete|
|Transform|쿼터니언 회전 문제 - y축 회전 시 90도에서 매끄럽게 전횐되지 않음|❌ Incomplete|
|ImGui|다른 PC 에서 실행 시 UI 렌더링에 오차 발생|✅ Completed|
|PhysX|레벨 Play 도중 Level 변경 시 삭제된 이전 레벨의 오브젝트 충돌 ContactEnd 호출됨|✅ Completed|
|Font|UI 컴포넌트 텍스트 렌더링 시 위치에 오류가 있음|❌ Incomplete|


---