---
layout: post
title: Project Sanabi 5. 4주차 진행사항
date: 2025-04-30 09:00:00 +0900
category: Project_Sanabi
---

## 5. 4주차 진행사항

1. [Particle](#1-particle)
2. [MRT, Differed Shading](#2-mrt-differed-shading)
3. [이후 계획](#3-이후-계획)


---

<br><br>

 - *Nexon 메이플스토리 집중채용 준비로 인해 기존 계획 무산.*
 - *실제 계획과 달리 2일만 프로젝트를 진행함.*


비, 몬스터 사망 파티클 구현 완료.

MRT, Differed Shading (노멀맵, 블룸 처리) 구현중

<br>

---

<br><br>

>### 1. Particle

![alt text](\public\img\SNB_Particle.png)

비 파티클, 몬스터 사망 시 파티클 효과 구현.

몬스터 파티클의 경우, PhysX를 통해 충돌체를 생성해 출돌 및 물리 시뮬레이션 효과를 받음.

<br>

---

<br><br>

>### 2. MRT, Differed Shading

![alt text](\public\img\MRT_Color.png)

![alt text](\public\img\MRT_Normal.png)

![alt text](\public\img\MRT_Pos.png)

멀티 렌더 타겟(MRT)을 이용해 색상, 노멀, 좌표 텍스쳐를 생성.

이후 광원 처리 셰이더에서 하나의 렌더 타겟으로 병합(Differed Shading).

광원 처리가 끝난 Lighting Texture 에서 임계값 이상의 픽셀을 추출한 블룸 텍스쳐를 생성, 블러 처리 후 적용.

추가적인 post process effect 적용 후 최종 렌더 타겟에 렌더링 후 디스플레이 송출.


<br>

---

<br><br>

>### 3. 이후 계획


- 컴퓨트 셰이더 응용 **(구현중)**
    1. 블룸 효과
    2. 노멀맵 반사 - MRT와 Differed Shading 으로 광원 처리 패스를 이용.

- 포스트 프로세스 이펙트
    1. 몬스터 사망 시 폭팔하는 효과
    2. 플레이어 피격 시 화면 노이즈

- 보스 구현은 스크립트 작성 노가다로 결과물 대비 시간만 많이 잡아먹을 것 같아 단순화하여 구현 예정 (제일 후순위).

---

<br>

> **현재 확인된 문제**

|오류 대상|오류 내용|해결 현황|
|---|---|---|
|Particle|Play/Stop 전환 중 파티클이 비정상적으로 생성되는 현상 있음|❌ Incomplete|
|Particle|m_MaxParticle 값이 같을 때 이전 파티클 데이터가 남아있음|❌ Incomplete|
|Transform|쿼터니언 회전 문제 - y축 회전 시 90도에서 매끄럽게 전횐되지 않음|❌ Incomplete|
|Font|UI 컴포넌트 텍스트 렌더링 시 위치에 오류가 있음|❌ Incomplete|


---