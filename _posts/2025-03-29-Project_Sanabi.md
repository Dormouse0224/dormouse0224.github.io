---
layout: post
title: Project Sanabi 1. 프로젝트 개요
date: 2025-03-29 09:00:00 +0900
category: Project_Sanabi
---

## 1. 프로젝트 개요

1. [프로젝트 개요](#1-프로젝트-개요-1)
2. [프로젝트 로드맵](#2-프로젝트-로드맵)
  - [사전 준비 및 학습](#--사전-준비-및-학습-0907--1023)
  - [1주차](#--1주차-1024--1030)
  - [2주차](#--2주차-1031--1106)
  - [3주차](#--3주차-1107--1013)
  - [4주차](#--4주차-1114--1120)

---

<br><br>

>### 1. 프로젝트 개요

<br>

|||
|---|---|
|프로젝트 이름|Sanabi DirectX 11 모작 프로젝트|
|프로젝트 기간|2025.03.31 ~ 2025.04.28|
|개발환경 및 언어|C++ 17|
||WDirectX 11|
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

#### - 1주차 (03.31 ~ 04.06)

**1주차 최종 목표**

- 레벨 및 맵을 Editor UI를 통해 작성 가능.


<br>

**1주차 작업 목록**

> **Asset**

<details>
<summary>펼치기/접기</summary> 

|작업 대상|작업 내용|작업 현황|
|---|---|---|
|모든 Asset|파일 저장 및 불러오기|❌ Incomplete|
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

</details>

<br>

> **Component**

<details>
<summary>펼치기/접기</summary> 

|작업 대상||작업 목록|작업 현황|
|---|---|---|
|모든 Component|컴포넌트를 오브젝트에 추가하는 기능|❌ Incomplete|
|PhysX|액터의 각 물리적 속성을 편집하는 기능|❌ Incomplete|
|PhysX|충돌 이벤트 콜백|❌ Incomplete|
|FSM|각 상태와 상태 전환 조건을 편집하는 기능|❌ Incomplete|
|FSM|State 스크립트를 FSM 에 등록하는 기능|❌ Incomplete|
|Button|버튼에 연결할 이벤트 함수를 스트립트로부터 설정하는 기능|❌ Incomplete|

</details>

<br>

---

<br>

#### - 2주차 (04.07 ~ 04.13)


<br>


---

<br>

#### - 3주차 (04.14 ~ 04.20)


<br>

---

<br>

#### - 4주차 (04.21 ~ 04.27)



<br>

---

<br>

#### - 마무리 및 프로젝트 발표 (04.28 ~ )


<br>

---