---
layout: post
title: DirectX 3D Practice 3. 2차 진행사항
date: 2025-07-22 12:00:00 +0900
category: DirectX_3D_Practice
---

## 3. 2차 진행사항

1. [Shadow](#1-shadow)
    - [Directional Light : Cascade Shadow Mapping](#--1-directional-light--cascade-shadow-mapping)
    - [Point Light : Cube Shadow Mapping](#--2-point-light--cube-shadow-mapping)
    - [Spot Light : Default Shadow Mapping](#--3-spot-light--default-shadow-mapping)

---

<br><br>

>### 1. Shadow

<br>

3D 공간상에서의 Directional, Point, Spot 세 종류 광원 구현.

<br>

#### - 1. Directional Light : Cascade Shadow Mapping

방향성 광원의 그림자는 Cascade Shadow Mapping 을 사용하여 구현했다.

[Cascade Shadow Mapping 관련 참고 문서](https://ogldev.org/www/tutorial49/tutorial49.html)

방향성 광원의 특성 상, 카메라의 시야에 잡히는 모든 물체에 대해 그림자가 생겨야 한다. 다만, 이를 위해서 그림자 매핑에 사용되는 텍스쳐의 해상도가 아무리 커져도 한 오브젝트가 매핑 텍스쳐에서 차지하는 픽셀 영역이 한정되기 때문에, 가까운 물체에 대해서 매우 낮은 퀄리티의 그림자가 출력되게 된다.

이러한 문제점을 해결하기 위해, 카메라의 시야 프러스텀을 거리에 따라 3분할하여 각각의 프러스텀 영역에 따라 매핑 텍스쳐에 해당 프러스텀 영역만을 매핑하여 계단식으로 그림자 매핑을 구현한 것이 Cascade Shadow Mapping (CSM) 이다.

가까운 영역일수록 매핑되는 영역이 좁으므로, 동일 해상도에서 오브젝트가 차지하는 픽셀 영역이 넓어지게 된다.

![alt text](\public\img\cascade_frustum.png)

<br>

아래 이미지는 CSM을 적용하지 않았을 때 그림자이다. 방향성 광원의 그림자 맵은 8192\*8192 해상도임에도 불구하고, 카메라의 뷰 거리가 1~10000 이기에 각 오브젝트가 차지하는 픽셀이 매우 적어 그림자가 블록 형태로 나타나게 된다.

![alt text](\public\img\DirectionalShadow_No_CSM.png)

<br>

이번에는 CSM을 적용했을 때의 그림자이다. 각 Cascade 마다 할당된 매핑 텍스쳐는 모두 8192\*8192 해상도로 동일하지만, 가까운 영역에서 오브젝트가 차지하는 픽셀 영역이 매우 넓어져 높은 퀄리티의 그림자를 출력할 수 있다.

디버그 이미지에서 붉은색 영역이 Near Cascade, 초록 영역이 Middle Cascade 이다.

![alt text](\public\img\DirectionalShadow_CSM.png)

![alt text](\public\img\DirectionalShadow_CSM_debug.png)


<br>

#### - 2. Point Light : Cube Shadow Mapping

점광원의 경우, 방향성 광원과 다르게 광원 위치를 중심으로 사방으로 뻗어나가며 그림자를 생성한다.

따라서, 그림자 매핑 시 직교 투영을 사용한 방향성 광원과 달리, 원근 투영을 사용해 매핑을 진행해야 한다.

또한, 광원을 중심으로 상/하/전/후/좌/우 총 6방향으로 매핑을 진행해야 하므로, 큐브맵 텍스쳐를 매핑 텍스쳐로 설정해 그림자 매핑 및 샘플링을 진행한다.

점광원은 한정적인 영역에만 빛을 투사하므로, 매핑 시 최적화를 위해 해상도를 1/8 스케일인 1024\*1024 해상도 6장을 사용했다. (만약 프레임 문제가 생기면, 더 낮춰도 된다)

![alt text](\public\img\PointShadow.png)

<br>

#### - 3. Spot Light : Default Shadow Mapping

스포트라이트의 그림자는 점광원의 그림자를 부분적으로 채용하면 된다.

기본적으로 점광원처럼 원근 투영을 사용해 매핑하고, FOV 값을 스포트라이트의 각도로 설정하면 된다.

점광원처럼 여러 방향을 매핑할 필요가 없으므로, 매핑 텍스쳐는 한장만 사용하며, 매핑 시 과도한 왜곡을 방지하기 위해 스포트라이트의 각도를 180도 미만으로 제한한다.

![alt text](\public\img\SpotShadow.png)



<br><br>

최종적으로, 여러 광원에 대해 그림자를 순차적으로 적용하여 광원 렌더링 타겟에 합치는 것으로 그림자를 구현한다.

또한, 3D 애니메이션 모델링에 대해 스키닝도 포함하여 그림자를 매핑하므로, 아래 이미지와 같이 특정 동작에서 모델에 그림자가 비추는 효과를 구현했다.

![alt text](\public\img\ModelShadow.png)


---