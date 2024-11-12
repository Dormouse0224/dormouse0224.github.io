---
layout: post
title: Project Dungeon&Fighter 4. 3주차 진행사항
date: 2024-11-12 12:00:00 +0900
category: Project_Dungeon&Fighter
---

## 4. 3주차 진행사항

1. [플레이어 스킬 - 스킬 오브젝트](#1-플레이어-스킬---스킬-오브젝트)
2. [플레이어 스킬 - 트래블러 스킬 구현](#2-플레이어-스킬---트래블러-스킬-구현)
3. [플레이어 및 보스의 피격과 사망](#3-플레이어-및-보스의-hud-피격과-사망)
4. [플레이어 아바타 시스템](#4-플레이어-아바타-시스템)


---

<br><br>

>### 1. 플레이어 스킬 - 스킬 오브젝트

- 플레이어 스킬 오브젝트는 플레이어가 스킬을 사용할 때 사용되는 여러 오브젝트를 의미한다.


- 구현한 스킬 오브젝트로는 아래의 3가지 종류를 구현했다:


  - 1. 중력의 영향을 받을 수 있는 투사체


  - 2. 충돌체를 통해 영역 내 시작과 종료 또는 지속시간 중 즉발 데미지나 도트 데미지를 주는 오브젝트


  - 3. 지면 위치를 기준으로 데미지를 주는 장판 오브젝트


---

<br><br>


![alt text](\public\img\arrow_0.png)

![alt text](\public\img\arrow_1.png)

![alt text](\public\img\arrow_2.png)

![alt text](\public\img\arrow_3.png)



**투사체**

  - 지정된 위치에서 지정된 방향(X, Y, Z축)으로 초기 속도에 따라 이동을 시작한다.

  - 함수를 통해 투사체가 받는 중력을 조절하거나, 중력을 받지 않도록 설정할 수 있다.

  - Delegate 를 통해 객체 생성 시 함수 포인터를 할당하여 투사체 사망 시 동작을 추가로 지정할 수 있다. (폭발, 장판 생성 등)

  - 투사체의 최대 관통 횟수를 설정할 수 있으며, 투사체는 마지막 충돌 또는 지면과의 충돌 시 사망한다.

  - 투사체 비행 중 모션 애니메이션은 투사체의 방향에 따라 투사체 중심점 기준으로 회전한다.


<br>



![alt text](\public\img\arrow_collider_0.png)

![alt text](\public\img\arrow_collider_1.png)



**충돌 오브젝트**

  - 충돌체를 이용하여 충돌 영역 내 데미지를 부여한다.

  - 데미지는 충돌 시작 데미지, 충돌 중 일정 주기의 도트 데미지, 객체 사망 시 마지막 데미지 총 3종류의 데미지를 설정할 수 있다.

  - 객체 생성 시 입력한 지속시간이 종료되면 스스로 삭제된다.


<br>



![alt text](\public\img\arrow_AoE_0.png)

![alt text](\public\img\arrow_AoE_1.png)



**장판(AoE) 오브젝트**

  - 지면상의 위치를 기준으로 원형 범위 내 데미지를 부여한다.

  - 객체 생성 시 입력한 지속시간이 종료되면 스스로 삭제된다.



<br><br>


>### 2. 플레이어 스킬 - 트래블러 스킬 구현


 - 지정된 키 입력 또는 커맨드 입력을 통해 구현된 스킬을 사용할 수 있다.

 - 스킬은 사용중 선딜레이, 스킬 쿨타임, 플레이어 스킬 모션, 스킬 데미지 오브젝트와 오브젝트 애니메이션 등이 복합적으로 구성되어 있다.


현재 3주차까지 구현된 내용은 다음과 같다:
  1. 기본 공격 (3단 콤보 공격)
  2. 스킬 - 기어드라이브 (지상/공중 사용 시 다른 스킬로 분기)
  3. 스킬 - 안개폭우 (다수의 투사체 사출 및 지면 공격 판정)
  4. 스킬 - 신무성장 : 사지타리우스 (애니메이션 컷씬 연출, 스테이지 전체 공격, 플레이어 무적)


<br>



![alt text](\public\img\skill_basic_0.png)

![alt text](\public\img\skill_basic_1.png)

![alt text](\public\img\skill_basic_2.png)



**기본 공격 (3단)**


  - Z 키로 최대 3단 공격이 가능한 트래블러의 기본 공격이다.

  - 제자리에서 연속으로 시전 시 연계 공격이 가능하며, 움직이거나 일정 시간이 지나면 초기화된다.

  - 첫번째와 두번째 공격은 중력의 영향을 받는 투사체를 발사하며, 세번째 공격은 중력의 영향을 받지 않고 직선으로 날아가는 투사체를 발사한다.

  - 모션 시간보다 선딜레이가 짧아 적절한 타이밍에 연계 시 모션 캔슬이 가능하다.

  - 최대 관총 수치는 모두 1로, 첫 충돌 시 사망한다.


<br>


![alt text](\public\img\skill_geardrive_0.png)

![alt text](\public\img\skill_geardrive_1.png)

![alt text](\public\img\skill_geardrive_2.png)

![alt text](\public\img\skill_geardrive_3.png)



**기어드라이브**


  - 트래블러의 첫번째 스킬로, 지상과 공중 모두 사용 가능하며, 공중 사용 시 지상과 다른 기능을 가진다.

  - 지상 사용 시, 투사체가 직선으로 날아가며 처음 충돌하면 사망하고 주변 범위에 충돌 오브젝트로 폭발 데미지를 준다.

  - 공중 사용 시, 플레이어가 살짝 점프하며 투사체를 아래 40도 방향으로 쏘고, 중간의 모든 충돌을 관통하고 지면까지 도달해 폭발 데미지를 준다.



<br>


![alt text](\public\img\skill_mistheavyrain_0.png)

![alt text](\public\img\skill_mistheavyrain_1.png)

![alt text](\public\img\skill_mistheavyrain_2.png)



**안개폭우**


  - 트래블러의 두번째 스킬로, 하늘로 12발의 투사체를 발사하여 잠시 후 지면에 일제히 낙하해 장판형 즉발 데미지를 준다.

  - 처음 하늘로 발사한 투사체가 일정 거리를 이동 후 위치를 계산해 Delegate 함수로 낙하하는 투사체를 생성, 이후 장판 데미지 오브젝트를 생성하는 방식으로 구현했다.

  - 낙하 투사체는 모든 오브젝트를 관통하여 무조건 지면에서 사망하며, 데미지는 없지만 함수 등을 통해 쉽게 추가할 수 있다.


<br>


![alt text](\public\img\skill_sagittarius_0.png)

![alt text](\public\img\skill_sagittarius_1.png)

![alt text](\public\img\skill_sagittarius_2.png)

![alt text](\public\img\skill_sagittarius_3.png)

![alt text](\public\img\skill_sagittarius_4.png)

![alt text](\public\img\skill_sagittarius_5.png)


**신무성장 : 사지타리우스**


  - 트래블러의 세번째 스킬이자 컷인 연출을 가지고 있는 궁극기이다.

  - 애니메이션과 모션 모두 단계적으로 진행되며, 플레이어는 시전 중 무적 상태를 유지하여 데미지를 입지 않는다.

  - 두번의 카메라 플래시 이펙트와 함께 2회에 걸쳐 스테이지에 존재하는 모든 적 오브젝트에게 데미지를 준다.





<br><br>


>### 3. 플레이어 및 보스의 HUD, 피격과 사망


![alt text](\public\img\hud_boss.png)

![alt text](\public\img\hud_player.png)


- 실시간으로 플레이어의 HP 와 MP 게이지, 보스 몬스터의 HP 게이지를 보여주는 HUD 를 추가했다.

- 플레이어의 경우 HP와 MP 재생 수치에 따라 HP 와 MP가 다시 차오른다.

- 오브젝트의 HP가 0 이하가 되면 FSM에 의해 사망 상태에 들어가게 되며, 플레이어와 보스 각각 상태에 따른 행동을 수행한다.

<br>



![alt text](\public\img\player_death_0.png)



**플레이어 사망**


  - 플레이어 캐릭터가 사망하면 화면이 어두워지며 카운트다운이 시작되고, 타운트다운 종료 시 세리아의 방으로 이동한다.

  - 카운트다운 종료 전 X 키를 입력하면 원래 위치에서 즉시 부활할 수 있다.



<br>


![alt text](\public\img\largo_death_0.png)



**보스 몬스터 사망**


  - 보스 몬스터가 사망하면 사망 애니메이션을 재생하고, 애니메이션 종료 시 객체 삭제 후 일정 시간 뒤 세리아의 방으로 이동한다.




<br><br>


>### 4. 플레이어 아바타 시스템


![alt text](\public\img\avatar_0.png)

![alt text](\public\img\avatar_1.png)




- 아바타의 코드를 통해 헤어, 모자, 상의, 하의, 신발, 무기 총 6개 파츠의 아바타를 개별적으로 변경할 수 있는 구조를 추가

- 각 파츠별로 아바타의 고유 번호만을 이용해 모션 애니메이션을 작성하고, 레이어를 구분하여 렌더링함

- 추후 추가될 인벤토리 / 장비창 시스템에서 사용될 예정


<br>



---