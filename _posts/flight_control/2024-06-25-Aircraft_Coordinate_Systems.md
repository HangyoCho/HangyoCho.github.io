---
title: '[Flight Control 2] - Aircraft Coordinate Systems and Transformations'
author: HangyoCho
date: 2024-06-25 15:39:00 +0900
categories: [3. Aerospace Engineering, 3.1 Flight Control]
tags: [Flight Control, UAV, Coordinate Transformation]
pin: false
math: true
mermaid: true
comments: true
---

## Aircraft Coordincate Systems
## 관성좌표계 (Inertial)
우주에 고정되어 있는 좌표계로 뉴턴의 제 2법칙을 적용할 수 있는 유일한 좌표계이다.  

- 지구 중심에 원점을 두고 있으며 화전하지 않는 좌표계 (발사체, 위성)
- 태양 중심에 원점을 두고 회전하지 않고 있는 좌표계 (화성 탐사)  
  
### 지구중심 관성좌표계 (Earth Centered Inertial: ECI)
항공기와 같이 지구에서 운항하는 비행체에 대해서는 일반적으로 지구 중심에 원점을 두고 지구와 병진 운동을 하고 있으나, 회전하고 있지 않은 좌표계를 관성좌표계로 가정할 수 있다.  

## 지구중심 지구 고정좌표계 (Earth Centered Earth Fixed: ECEF)
지구중심 관성좌표계와 같이 지구 중심에 원점을 두고 있으며, 지구와 함께 자전운동을 하고 있는 좌표계이므로 지구에 고정되어 있는 좌표계이다.  

## 지면좌표계 (North East Down: NED)
<div style="text-align: center;">
  <img src="https://www.researchgate.net/publication/215523100/figure/fig9/AS:670436761280522@1536855988090/Illustration-of-the-ECI-ECEF-and-NED-reference-frames.png" alt="ECI and NED Frame" width=480 height=480/>
  <p>ECI and NED Frame</p>
</div>

지면좌표계의 원점은 지표면에 있으며, 지구와 함께 자전운동을 한다.  
이 좌표계에서 $X_E$축은 북쪽을, $Y_E$축은 동쪽을, 그리고 $Z_E$축은 지구 중심 방향을 향한다.     

일반적으로 항공기 운동 특성을 해석하기 위해 사용하는 지구 모델은 완전 구 형태로 가정한다.  

지구 중심 지구 고정좌표계와 마찬가지로 지구에 고정되어 있는 좌표계이며, 관성좌표계로 가정할 수 있다.  

## 기체고정 좌표계 (Body-Fixed)
항공기 기체에 고정된 좌표계로 $X_B$축은 동체에 기수축, $Y_B$축은 오른쪽 날개 방향, 그리고 $Z_B$축은 오른손 법칙에 의해 동체 아랫방향을 향하는 좌표계이다.  

항공기는 병진 운동과 함께 기체 축에 대해서 회전운동을 한다.

기체고정 좌표계에 대해서 관성 모멘트가 일정하므로 모멘트 방정식을 기술하는데 편리하며, 지면좌표계에서 오일러각(Euler angle)을 통해 변환된다.  


| 좌표축 | 방향       | 회전운동          | 작용 모멘트 |
|--------|------------|-------------------|-------------|
| $X_B$  | 전진방향  | 롤 (Roll, $\phi$)  | 롤링 모멘트 |
| $Y_B$  | 오른쪽 날개 | 피치 (Pitch, $\theta$) | 피칭 모멘트 |
| $Z_B$  | 수직 하방  | 요 (Yaw, $\psi$)  | 요잉 모멘트 |


<div style="text-align: center;">
  <img src="https://www.researchgate.net/publication/322746836/figure/fig2/AS:587508022804481@1517084236128/Airplane-Reference-Frames-Stevens-and-Lewis-1992.png" alt="Body-Fixed Coordinate System" width=640 height=480/>
  <p>Body-Fixed Coordinate System</p>
</div>
<!-- https://www.researchgate.net/publication/328578766/figure/fig21/AS:687421054255106@1540905359112/Figure-A6-Transformation-from-the-Aerodynamic-to-Body-Fixed-Coordinate-System.png -->

기체고정 좌표계에 대한 항공기 속도벡터의 방향을 이용해서 공기역학적인 힘과 모멘트를 표현하는데 매우 중요한 두 가지 각인 받음각 (Angle of Attack, $\alpha$)과 옆미끄럼각 (Sideslip Angle, $\beta$)을 정의할 수 있다.  

항공기의 무게중심의 병진속도를 $V_P$라 하고, 속도의 좌표축에 대한 성분을 $(U, V, W)$라 하자.  

각 좌표축 속도는 아래와 같이 정의된다.  

$$
\begin{aligned}
U &= V_P \cos \beta \cos \alpha \\
V &= V_P \sin \beta \\
W &= V_P \cos \beta \sin \alpha
\end{aligned}
$$

$$
V_P = \sqrt{U^2 + V^2 + W^2}
$$

결과적으로 받음각($\alpha$)과 옆미끄럼각($\beta$)은 아래와 같다.

$$
\begin{aligned}
\alpha &= \tan^{-1}(W / U) \\
\beta &= \sin^{-1}(V / V_P)
\end{aligned}
$$


## 안정성 좌표계 (Stability)
기체좌표계에서 $Y_B$축을 중심으로 받음각만큼 회전을 수행하여 얻게되는 좌표계이다.  

정상상태(Steady State)로 비행하고 있는 항공기가 바람과 같은 외란에 의해 운동에 교란이 발생하였을 때, 항공기의 운동을 해석하는데 많이 사용된다.  

안정성 좌표계는 정상상태 비행조건에 따라 다르게 정의되지만, 하나의 비행조건에 따라 정의된 후에는 정의된 좌표계를 계속 사용하기 때문에 동체에 고정되어 있는 동체 좌표계의 일종이다.  

항공기가 대칭비행을 하고 있을 경우, 항공기에 작용하는 항력은 안정성 좌표계의 $X_S$축에 반대 방향으로 작용하며, 양력은 안정성 좌표계의 $Z_S$축의 반대 방향으로 작용한다.  

## 바람 좌표계 (Wind)
항공기의 상대 바람 방향을 나타내는 좌표계로서, 공력 자료들의 기준이 되므로 힘 방정식들을 기술하는데 편리한 좌표계이다.  

안정성 좌표계에서 $Z_S$축을 중심으로 옆미끄럼각 만큼 회전시킨 좌표계이다.  

항공기에 작용하는 항력과 양력은 대칭비행을 하고 있는 상태인 경우나 대칭비행이 아닌 비행상태인 경우에서나 바람 좌표계의 $X_W$과 $Z_W$축의 항상 반대 방향으로 작용한다.  
