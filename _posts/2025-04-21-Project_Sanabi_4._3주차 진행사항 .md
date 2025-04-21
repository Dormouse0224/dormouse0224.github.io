---
layout: post
title: Project Sanabi 4. 3주차 진행사항
date: 2025-04-21 09:00:00 +0900
category: Project_Sanabi
---

## 4. 3주차 진행사항

0. [작업 목록](#0-작업-목록)
1. Player State
2. Monster - Turret & Battle Gate
3. Cursor
4. 4주차 계획


---

<br><br>

플레이어 행동 상태 구현 완료. (대기, 달리기, 점프, 벽타기, 피격, 사망, 로프 발사, 로프 스윙, 처형, 홀딩, 대시 총 11종)

몬스터 1종 및 투사체 구현, 전투구역 차단문 구현.

커서 이미지 교체.

스테이지 1 오브젝트 배치중. (진행률 20%, 1일 소요 예상)

![alt text](\public\img\SNB_Week3_Progress.png)

https://youtu.be/eeRZEjJL50c

---

<br><br>

>### 1. Player State

#### - 기본 조작 (대기, 이동, 점프)


#### - 벅타기 / 벽점프



#### - 로프 스윙



#### - 처형, 홀딩, 대시


---

<br><br>

>### 2. Monster - Turret & Battle Gate


---

<br><br>

>### 3. Cursor



---

<br><br>

>### 4. 4주차 계획


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