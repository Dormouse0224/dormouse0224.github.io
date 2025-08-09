---
layout: post
title: DirectX 3D Practice 2. 1차 진행사항
date: 2025-07-14 12:00:00 +0900
category: DirectX_3D_Practice
---

## 2. 1차 진행사항

1. [3D Light](#1-3d-light)
2. [Cascade Frustum Culling](#2-cascade-frustum-culling)

---

<br><br>

>### 1. 3D Light

<br>

3D 공간상에서의 Directional, Point, Spot 세 종류 광원 구현.

<br>

1. Directional Light

방향성 광원. 렌더링 화면 내 모든 물체에 특정 방향으로 빛을 비춘다.

![alt text](\public\img\directional_light.png)

<br>

2. Point Light

점광원. 일정 반경 이내에 빛을 비춘다. 거리에 따른 감쇄 효과가 있다.

![alt text](\public\img\point_light.png)

<br>

3. Spot Light

스포트라이트. 일정 반경 이내에, 정해진 각도로 원뿔 형태의 빛을 비춘다.

점광원과 같이 거리에 따른 감쇄 효과가 있다.

![alt text](\public\img\spot_light.png)


---

<br><br>

>### 2. Cascade Frustum Culling

<br>

Cascade Shadow Map 구현을 위한 계단식 카메라 절두체와 절두체 컬링 구현.

[Cascade Shadow Mapping 관련 참고 문서](https://ogldev.org/www/tutorial49/tutorial49.html)

<br>

![alt text](\public\img\cascade_frustum.png)

절두체를 위 그림과 같이 세 단계로 분할하여, 오브젝트 위치에 따라 어떤 절두체에 포함되는지 검사한다.

만약 어떤 절두체에도 포함되지 않는다면, 렌더링 대상에서 제외되어 드로우콜이 발생하지 않도록 한다.



---