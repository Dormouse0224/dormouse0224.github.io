---
layout: post
title: Project Sanabi 1. 프로젝트 개요
date: 2025-03-29 09:00:00 +0900
category: Project_Sanabi
---

## 1. 프로젝트 개요

1. [DirectX 산나비 모작 프로젝트](#1-directx-산나비-모작-프로젝트)
2. [프로젝트 로드맵](#2-프로젝트-로드맵)
  - [프로젝트 사전 준비](#--프로젝트-사전-준비---0330)
  - [1주차](#--1주차-0331--0406---editor--ui)
  - [2주차](#--2주차-0407--0413---script--interaction)
  - [3주차](#--3주차-0414--0420---shader-effect)
  - [4주차](#--4주차-0421--0427---wrap-up)

---

<br><br>

>### 1. DirectX 산나비 모작 프로젝트

<br>

|||
|---|---|
|프로젝트 이름|Sanabi DirectX 11 모작 프로젝트|
|프로젝트 기간|2025.03.31 ~ 2025.04.28|
|개발환경 및 언어|C++ 17|
||DirectX 11|
||Visual Studio 2022|
||자체 제작 엔진|
|프로젝트 참여자|김재영|

<br>

대한민국의 인디 게임 개발사 원더포션에서 개발하고, 네오위즈에서 유통하는 로프 액션 플랫폼 인디 게임 "산나비"를 DirectX 및 기타 라이브러리를 이용하여 모작하는 프로젝트 입니다.

[산나비 스팀 페이지](https://store.steampowered.com/app/1562700/_/)


<br>

각 주차 프로젝트 세부 진행도는 ✅ Completed / ⏳ In Progress / ❌ Incomplete 로 기록됩니다.

---

<br><br>

>### 2. 프로젝트 로드맵

<br>

#### - 프로젝트 사전 준비 ( ~ 03.30)

> DirectX 기반 자체 엔진 리팩토링

> 에셋, 레벨 객체를 별도의 스크립트 라이브러리로 분리


---

<br>

#### - 1주차 (03.31 ~ 04.06) - Editor & UI

**1주차 목표**

- 에디터의 구현을 중심으로 에셋과 컴포넌트 설계 및 구현.

- 레벨 및 맵을 Editor UI를 통해 작성이 가능한 단계까지를 최종 목표로 한다.


<br>

**1주차 작업 목록**

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
|Mesh|프리뷰 이미지에서 드래그로 프리뷰 카메라를 조작하는 기능|❌ Incomplete|
|Mesh|프리뷰 토폴로지를 변경시키는 기능|❌ Incomplete|
|Sound|소리를 미리 들어보는 기능|❌ Incomplete|
|Graphic Shader|셰이더의 각종 옵션(토폴로지, 렌더 도메인, 각 스테이트)을 변경하는 기능|❌ Incomplete|
|Compute Shader|컴퓨트 셰이더의 속성(텍스쳐 또는 상수 데이터) 조정 기능|❌ Incomplete|
|Prefab|기존 오브젝트를 프리펩으로 만드는 기능|❌ Incomplete|
|Prefab|프리펩을 레벨에 오브젝트로 배치하는 기능|❌ Incomplete|
|Sprite|새로운 스프라이트를 생성하거나 기존 스프라이트를 편집하는 기능|❌ Incomplete|
|Flipbook|플립북에 씬을 추가하거나 제거하는 기능|❌ Incomplete|
|Flipbook|플립북의 각 씬 속성을 편집하는 기능|❌ Incomplete|
|Level|레벨을 새로 생성하고 오브젝트를 추가하는 기능|❌ Incomplete|
|State|FSM 컴포넌트와 연계|❌ Incomplete|

<br>

> **Component**

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|모든 Component|컴포넌트 저장 및 불러오기|✅ Completed|
|모든 Component|컴포넌트를 오브젝트에 추가하는 기능|❌ Incomplete|
|Collider2D|컴포넌트 제거 - 충돌 관련 기능 PhysX로 통합|❌ Incomplete|
|PhysX|액터의 각 물리적 속성을 편집하는 기능|✅ Completed|
|PhysX|충돌 이벤트 콜백|✅ Completed|
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


<br>

> **현재 확인된 문제**

|오류 대상|오류 내용|해결 현황|
|---|---|---|
|Particle|Play/Stop 전환 중 파티클이 비정상적으로 생성되는 현상 있음|❌ Incomplete|

<br>

---

<br>

#### - 2주차 (04.07 ~ 04.13) - Script & Interaction

**2주차 목표**

- 오브젝트와 컴포넌트가 사용할 스크립트의 작성 및 오브젝트간 상호작용 구현

- 실제 스테이지의 전환 등 게임의 기능적인 부분의 플레이가 가능한 단계를 최종 목표로 한다.


<br>


---

<br>

#### - 3주차 (04.14 ~ 04.20) - Shader Effect

**3주차 목표**

- 인게임 내 그래픽 요소(후처리, 파티클, 반사광 처리 등)의 셰이더 작성

- 게임의 그래픽스적 요소까지 구현하여 원작과의 차이를 줄이는 것을 목표로 한다.

- 여유가 있으면 다이얼로그 이벤트 구현도 고려해 볼 것.


<br>

---

<br>

#### - 4주차 (04.21 ~ 04.27) - Wrap-Up

**4주차 목표**

- 이전 주차 작업 중 미흡한 부분 마무리



<br>

---

<br>

#### - 마무리 및 프로젝트 발표 (04.28 ~ )


<br>

---