---
layout: post
title: Project Dungeon&Fighter 5. 4주차 진행사항
date: 2024-11-20 12:00:00 +0900
category: Project_Dungeon&Fighter
---

## 5. 4주차 진행사항

1. [로딩 및 프레임 보완](#1-로딩-및-프레임-보완)
2. [UI (인벤토리, 장비창, 아바타)](#2-ui-인벤토리-장비창-아바타)
3. [NPC 상호작용 (던전 선택, 맵이동)](#3-npc-상호작용-던전-선택-맵이동)
4. [1~3주차 동안 미진했던 부분을 보완하기 위한 추가 작업](#4-13주차-동안-미진했던-부분을-보완하기-위한-추가-작업)
5. [현재 직면한 문제점](#5-현재-직면한-문제점)

---

<br>

 - **감기로 인해 스킬 HUD 는 진행하지 못했음.**

<br><br>

>### 1. 로딩 및 프레임 보완


![alt text](\public\img\backgroundload.png)


- 스레드와 뮤텍스를 활용해 게임 실행 준비 시간을 release 기준 3분 -> 30초 정도로 줄이고, 평균 프레임을 20FPS -> 50FPS 정도로 늘렸다.


- 로딩 및 프레임 보완에는 다음과 같은 방법을 사용했다:


  - 1. 백그라운드 작업으로 게임 실행 중 리소스 파일 읽기와 메모리 로드를 동시에 수행.


  - 2. 멀티스레드로 동시에 여러개의 텍스쳐 로딩을 처리.


  - 3. GDI+의 DrawImage는 회전에만 사용, 나머지 반전 / 선형닷지 / 알파블렌딩은 직접 함수를 구현해 LockBits로 직접 픽셀 비트를 조작.



---


<br><br>


>### 2. UI (인벤토리, 장비창, 아바타)


 - UI 계층 구조를 이용해 인벤토리와 아바타 탭, 장비창, 아바타창, 인벤토리 슬롯을 구현하였다.

 - 아이템은 각각의 분류에 맞는 인벤토리에만 들어가며, 아바타는 교체 시 캐릭터 외형을 변경한다.



<br>



![alt text](\public\img\avatarsystem.png)



**아바타**


  - 아바타 인벤토리 및 아바타 슬롯에 아이템을 드래그-드롭으로 장착하거나 해제할 수 있다.

  - 장착 시 아바타는 자동으로 알맞는 위치에 장착된다.

  - 아이템을 다른 아이템으로 드래그 시 아이템끼리 서로 교체가 된다.

  - 아바타는 장착 또는 해제 즉시 캐릭터 외형을 변경시킨다.


<br>


![alt text](\public\img\equipinventory.png)



**장비, 소비**


  - 아바타와 비슷하지만, 장비는 캐릭터 외형을 변경시키지 않는다.

  - 아이템은 각각의 분류에 맞는 인벤토리에만 들어갈 수 있다.





<br><br>


>### 3. NPC 상호작용 (던전 선택, 맵이동)


- NPC 에게 가까이 가면 NPC는 행동을 취한다.

- NPC 에게 가까이 가서 X 키를 눌러 상호작용이 가능하다.

- 상호작용으로 특정 맵으로 이동시키거나, 생성된 던전 리스트를 골라 이동할 수 있는 UI를 띄운다.

<br>


![alt text](\public\img\selectdungeonui.png)



**던전 리스트 선택**


  - 게임에 등록된 던전 목록을 불러와서 선택할 수 있는 UI를 띄운다.

  - 선택 시 해당 던전의 Start 스테이지로 이동한다.



<br>


![alt text](\public\img\npcteleport.gif)


**특정 맵으로 이동**


  - 상호작용 시 지정된 맵으로 이동시킨다.




<br><br>


>### 4. 1~3주차 동안 미진했던 부분을 보완하기 위한 추가 작업


![alt text](\public\img\dungeonclear.gif)

![alt text](\public\img\monsterhurt.png)

![alt text](\public\img\portallock.png)




- 던전 클리어 시 클리어 이펙트를 추가하였다.

- 몬스터의 피격 경직을 추가로 구현하였다.

- 던전에 남은 몬스터가 있으면 포탈이 잠기는 등 포탈 기능을 추가하였다.



<br><br>


>### 5. 현재 직면한 문제점

1. **릴리즈 버전 빌드 과정 중 오류**

  - 디버그 버전에서는 문제가 없어 원인 분석이 힘듦
  - *해결됨: assert 내부에 함수를 사용하는 부분이 있어서 릴리즈때 최적화로 인해 삭제되는거였음*


2. **백그라운드 로딩 중 드물게 발생하는 메모리 액세스 오류**

  - 발생 조건은 확인했는데 원인을 알 수 없음


---