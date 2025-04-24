---
layout: post
title: Project Sanabi 4. 3주차 진행사항
date: 2025-04-21 09:00:00 +0900
category: Project_Sanabi
---

## 4. 3주차 진행사항

0. [작업 목록](#0-작업-목록)
1. [Player State](#1-player-state)
2. [Monster - Turret & Battle Gate](#2-monster---turret--battle-gate)
3. [Cursor](#3-cursor)
4. [4주차 계획](#4-4주차-계획)


---

<br><br>

플레이어 행동 상태 구현 완료. (대기, 달리기, 점프, 벽타기, 피격, 사망, 로프 발사, 로프 스윙, 처형, 홀딩, 대시 총 11종)

몬스터 1종 및 투사체 구현, 전투구역 차단문 구현.

커서 이미지 교체.

스테이지 1 오브젝트 배치중. (진행률 20%, 1일 소요 예상)

![alt text](\public\img\SNB_Week3_Progress.png)

<br><br>

**데모 플레이 영상**

<iframe width="560" height="315" src="https://www.youtube.com/embed/eeRZEjJL50c?si=Vp1LNgsnyr_KqBOI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

<br><br>

>### 1. Player State

#### - 기본 조작 (대기, 이동, 점프)

- A, D 로 횡이동 및 점프

- 마지막 진행 방향에 따라 좌우 반전을 적용함

<br>

#### - 벅타기 / 벽점프

- Player 가 측면에서 Tile 객체와 충돌중인 경우, Jump 상태에서 벽타기로 진입할 수 있음.

- 벽타기 중 점프 시 벽의 반대 방향으로 속도를 가지고 점프함.

<br>

#### - 로프 스윙

- PhysX 물리엔진의 **PxDistanceJoint** 클래스를 활용해 고정점을 기준으로 진자 운동 구현.

- **레이 캐스팅**을 통해 커서 방향의 일정 거리 내 충돌점을 찾고,  Tile 인 경우 로프로 Static Actor 객체로 고정점을 생성.

- 레이 캐스팅 결과의 거리값이 일정 이상인 경우 해당 방향의 x축 방향으로 힘을 줘서 초기 속도를 추가시킴.

<br>

#### - 처형, 홀딩, 대시

- 로프 스윙과 비슷하게 **레이 캐스팅**으로 탐색한 객체가 Monster 인 경우 처형 상태에 진입함.

- 몬스터까지 이동 후 좌클릭 키다운 유지 시 홀딩을 적용, 키다운을 해제해서 홀딩에서 벗어난 경우 몬스터를 Dead 상태로 변경.

- 쳐형 종료 시 커서 방향으로 초기 속도를 주어 대시 구현.

<br>

#### - 피격, 사망

- 피격 시 피격 상태 도중 무적 적용, 피격 순간에 바라보던 방향의 반대로 넉백됨.

- 사망 시 카메라 줌인과 함께 사망 모션이 출력되고, 모션 종료 시 잠시 뒤에 최근 Play 상태로 전환했던 레벨을 매니저에서 불러옴.

<br>

---

<br><br>

>### 2. Monster - Turret & Battle Gate

#### - 대기, 스폰

- 몬스터는 전투구역 차단문의 자식으로 존재하며, 플레이어가 차단문 통과시 차단문이 닫히며 자식의 몬스터 객체를 깨우는 식으로 동작함.

- 스폰 시 레이 캐스팅에만 충돌하는 쿼리 충돌체를 추가함.

<br>

#### - 조준, 사격

- 매 틱마다 플레이어의 방향을 가져와 해당 방향으로 Head 파츠를 회전시킴.

- 사격 진입 시 일정 간격으로 총알 투가체를 Head 방향으로 발사, 총알 투사체는 벽 또는 플레이어와 충돌 또는 일정 시간 후 삭제됨.

<br>

#### - 홀딩, 사망

- 홀딩 상태에서는 공격을 하지 않음.

- 사망 시 쿼리 충돌체를 제거하고 사망 모션을 출력.

<br>

---

<br><br>

>### 3. Cursor

- 커서를 삭제 후 키매니저에서 커서 위치에 객체를 생성해 UI 카메라를 이용해서 커서 위치에 렌더링하는 방식으로 구현.

---

<br><br>

>### 4. 4주차 계획


- 보스 구현은 스크립트 작성 노가다로 결과물 대비 시간만 많이 잡아먹을 것 같아 단순화하여 구현 예정.

- 포스트 프로세스 이펙트
    1. 몬스터 사망 시 폭팔하는 효과
    2. 플레이어 피격 시 화면 노이즈

- 파티클
    1. 몬스터 사망 시 사망 파츠 흩뿌려지는 효과
    2. 화면 전역에 비내리는 효과

- 컴퓨트 셰이더 응용
    1. 블룸 효과
    2. 노멀맵 반사 - 이건 픽셀 셰이더에서 함수를 만들어 처리할지, 컴퓨트 셰이더로 처리하면 이점이 있을지 고민중.


![alt text](\public\img\normalmap_example.png)

---


<br><br>

>### 0. 작업 목록

> **Editor**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|Outliner|오브젝트 상속 관계 편집 기능|✅ Completed|

<br>

> **Level**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|Level|시작 레벨 만들기|⏳ In Progress|
|Level|스테이지 만들기|⏳ In Progress|

<br>

> **Player, Monster, etc**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|Player|기본 이동 구현 (이동, 점프, 벽 점프, 등반)|✅ Completed|
|Player|로프 액션 구현|✅ Completed|
|Monster|Turret 몬스터 구현|✅ Completed|
|Monster|Player 와 공격 상호작용|✅ Completed|
|BattleGate|전투구역용 차단문 구현|✅ Completed|

<br>

> **기타 작업**

|작업 대상|작업 내용|작업 현황|
|---|---|---|



<br>

> **현재 확인된 문제**

|오류 대상|오류 내용|해결 현황|
|---|---|---|
|Particle|Play/Stop 전환 중 파티클이 비정상적으로 생성되는 현상 있음|❌ Incomplete|
|Particle|m_MaxParticle 값이 같을 때 이전 파티클 데이터가 남아있음|❌ Incomplete|
|Transform|쿼터니언 회전 문제 - y축 회전 시 90도에서 매끄럽게 전횐되지 않음|❌ Incomplete|
|Font|UI 컴포넌트 텍스트 렌더링 시 위치에 오류가 있음|❌ Incomplete|


---