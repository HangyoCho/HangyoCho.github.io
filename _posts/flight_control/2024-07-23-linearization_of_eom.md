---
title: '[Flight Control 7] - Linearization of Aircraft Equations of Motion'
author: HangyoCho
date: 2024-07-23 16:52:00 +0900
categories: [3. Aerospace Engineering, 3.1 Flight Control]
tags: [Flight Control, UAV, Equation of Motion, Linearization]
pin: false
math: true
mermaid: true
comments: true
---

## Aircraft Equations of Motion 
#### Equations of Linear Motion
  

$$
X = m(\dot{U} + QW - VR + g\sin{\Theta})
\tag{1} 
$$

$$
Y = m(\dot{V} + UR - PW - g\cos{\Theta}\sin{\Phi})
\tag{2} 
$$

$$
Z = m(\dot{W} + VP - UQ - g\cos{\Theta}\cos{\Phi})
\tag{3} 
$$

#### Equations of Angular Motion

$$
I_{xx} \dot{P} - I_{xz} \dot{R} = ( \dot{R} + PQ)(I_{zz} - I_{yy}) + QR(I_{zz} - I_{yy}) = L
\tag{4} 
$$

$$
I_{yy} \dot{Q} + I_{xz} \dot{P} = (P^2 - R^2) + PR(I_{xx} - I_{zz}) = M
\tag{5} 
$$

$$
I_{zz} \dot{R} - I_{xz} \dot{P} + PQ(I_{yy} - I_{xx}) + I_{xz} QR = N
\tag{6} 
$$

#### Kinematic Equations 

$$
P = \dot{\Phi} - \dot{\Psi} \sin{\Theta}
\tag{7} 
$$

$$
Q = \dot{\Theta} \cos{\Phi} + \dot{\Psi} \cos{\Theta} \sin{\Phi}
\tag{8} 
$$

$$
R = \dot{\Psi} \cos{\Theta} \cos{\Phi} - \dot{\Theta} \sin{\Phi}
\tag{9} 
$$

### Linearization via Taylor Series Expansion

특수한 비행조건에 대해서는 항공기의 운동방정식을 선형화된 식으로 표현할 수 있다.  
선형화된 운동방정식은 비선형 방식에 비해서 해석적인 해를 구할 수 있고, 다양한 제어기 설계 기법을 적용할 수 있다는 장점이 있다.  
비선형 운동방정식을 선형화하기 위해서 주어진 운동을 다른 형태로 표현한다.  

$$
평균 운동(평형, trim 상태) + 동적 운동(교란 운동)
$$


다음과 같이 하나의 비선형 시스템을 고려하자.

$$
\dot{x} = f(x)
\tag{10} 
$$

선형화를 위해 상태변수 벡터를 다음과 같이 가정한다.

$$
x = x_e + \Delta x
\tag{11} 
$$



이때 테일러(Taylor) 급수로 1차항까지 전개하면 다음과 같다.

$$
f(x) = f(x_e + \Delta x) \approx f(x_e) + \frac{\partial f}{\partial x} \bigg|_{x_e} \Delta x
\tag{14} 
$$

여기서 사용된 테일러 급수는 아래와 같이 표현된다.

$$
f(x) = f(a) + \frac{f'(a)}{1!}(x - a) + \frac{f''(a)}{2!}(x - a)^2 + \frac{f^{(3)}(a)}{3!}(x - a)^3 + \cdots
\tag{12}
$$

$$
f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!}(x - a)^n
\tag{13}
$$

한편, 평형상태 조건 $x_e$는 다음 조건을 만족한다.

$$
\dot{x}_e = f(x_e) = 0
\tag{15} 
$$

따라서 최종 선형화된 운동방정식은 다음 형태가 된다.

$$
\Delta\dot{x} = \frac{\partial f}{\partial x} \bigg|_{x_e} \Delta x
\tag{16} 
$$

## Linearization of Equations of Motion

선형화를 위해 임의상태의 운동이 평형(Equilibrium) 상태로부터 일부 교란이 발생한 것으로 이루어진다고 가정하므로, 항공기의 경우 먼저 평형상태의 조건을 다음과 같이 가정한다.  

여기서 가정한 평형상태의 조건은 상수이기에 미분을 거치면 사라지게된다.

$$
(U_0, V_0, W_0, P_0, Q_0, R_0) = constant
\tag{17} 
$$

$$
\dot{U}_0 = \dot{V}_0 = \dot{W}_0 = \dot{P}_0 = \dot{Q}_0 = \dot{R}_0 = 0
\tag{18} 
$$

또한, 주어진 평형상태를 유지하기 위해 외부에서 가해지는 힘을 $X_0, Y_0, Z_0, L_0, M_0, N_0$로 가정한다.

$$
X_0 = m(Q_0 W_0 - R_0 V_0 + g\sin{\Theta_0})
\tag{19} 
$$

$$
Y_0 = m(U_0 R_0 - P_0 W_0 - g\cos{\Theta_0} \sin{\Phi_0})
\tag{20} 
$$

$$
Z_0 = m(P_0 V_0 - Q_0 U_0 - g\cos{\Theta_0} \cos{\Phi_0})
\tag{21} 
$$

$$
L_0 = (I_{zz} - I_{yy})Q_0 R_0 - I_{xz} P_0 Q_0
\tag{22} 
$$

$$
M_0 = I_{xx}(P_0^2 - R_0^2) + (I_{xx} - I_{zz}) P_0 R_0
\tag{23} 
$$

$$
N_0 = I_{zz} R_0 P_0 + I_{xz} (Q_0^2 + R_0 Q_0)
\tag{24} 
$$

만일 평형상태로부터 동적 운동이 발생하였다고 가정하면 결과적으로 나타나는 운동 변수는 다음과 같이 표현된다.

$$
U = U_0 + u, \quad V = V_0 + v, \quad W = W_0 + w
\tag{25} 
$$

$$
P = P_0 + p, \quad Q = Q_0 + q, \quad R = R_0 + r
\tag{26} 
$$

$$
\Phi = \Phi_0 + \phi, \quad \Theta = \Theta_0 + \theta, \quad \Psi = \Psi_0 + \psi
\tag{27} 
$$

결과적으로 평형상태의 비행조건이 주어졌을 때 미소 교란에 대한 선형 운동방정식 형태는 아래와 같이 나타낼 수 있다.  
여기서 $vr$와 같은 것들은 small perturbation의 곱이기에 미소 요소로 판단하에 사라지게 된다.

$$
\Delta X = m[\dot{u} + W_0 q + Q_0 w - V_0 r - R_0 v + g\cos{\Theta_0}\theta]
\tag{28} 
$$

$$
\Delta Y = m[\dot{v} + U_0 r + R_0 u - W_0 p - P_0 w - (g\cos{\Theta_0}\cos{\Phi_0})\phi + (g\sin{\Theta_0}\sin{\Phi_0})\theta]
\tag{29} 
$$

$$
\Delta Z = m[\dot{w} + V_0 p + P_0 v - U_0 q - Q_0 u + (g\cos{\Theta_0}\sin{\Phi_0})\phi + (g\sin{\Theta_0}\cos{\Phi_0})\theta]
\tag{30} 
$$

$$
\Delta L = I_{xx}\dot{p} - I_{xz} \dot{r} + (I_{zz} - I_{yy})(Q_0r + R_0q) - I_{xz}(P_0q +  Q_0p)
\tag{31} 
$$

$$
\Delta M = I_{yy}\dot{q} + (I_{xx} - I_{zz})(P_0r + R_0p) - (2R_0 r - 2P_0 p)I_{xz}
\tag{32} 
$$

$$
\Delta N = I_{zz}\dot{r} - I_{xz} \dot{p} + (I_{yy} - I_{xx})(P_0q + Q_0p) + I_{xz}(Q_0r + R_0q)
\tag{33} 
$$

### Simplification of Aircraft Equations

동체 각속도와 자세각(오일러 각) 사이의 비선형 관계식을 선형화하면 마찬가지로 다음과 같은 선형화된 운동학 관계식을 얻을 수 있다.

$$
p = \dot{\phi} - \dot{\psi} \sin{\Theta_0} - \theta (\dot{\Psi_0} \cos{\Theta_0})

\tag{34} 
$$

$$
q = \dot{\theta} \cos{\Phi_0} - \theta (\dot{\Psi_0} \sin{\Phi_0} \sin{\Theta_0}) + \dot{\psi} \sin{\Phi_0} \cos{\Theta_0}

\tag{35} 
$$

$$
r = \dot{\psi} \cos{\Theta_0} \cos{\Phi_0} - \phi (\dot{\Psi_0} \cos{\Theta_0} \sin{\Phi_0} + \dot{\Theta_0} \cos{\Phi_0}) 
- \dot{\theta} \sin{\Phi_0} - \theta (\dot{\Psi_0} \sin{\Theta_0} \cos{\Phi_0})
\tag{36} 
$$

위 식들은 물리적 의미를 전달하기에 아직 복잡한 형태로서 몇몇 특수한 형태의 평형 비행상태를 적용하면 더 간략화 될 수 있다.

  - 직선비행 $\dot{\Psi}_0 = \dot{\Theta}_0 = 0$
  - 대칭비행 $\Psi_0 = V_0 = 0$
  - 수평비행 $\Phi_0 = 0$

 

먼저 수평 직선대칭비행 형태의 평형 비행상태를 가정하면 선형화된 운동방정식은 다음과 같다:

$$
\Delta X = m \left[ \dot{u} + W_0 q + Q_0 w - R_0 v + g \cos \Theta_0 \theta \right]

\tag{37} 
$$
 

$$
\Delta Y = m \left[ \dot{v} + U_0 r + R_0 u - W_0 p - P_0 w - g \cos \Theta_0 \phi \right]

\tag{38} 
$$
 
 
$$
\Delta Z = m \left[ \dot{w} + P_0 v - U_0 q - Q_0 u + g \sin \Theta_0 \theta \right] 

\tag{39} 
$$

이때 모멘트 평형 방정식은 영향을 받지 않는 점을 주목할 필요가 있다.

운동학 관계식도 평형 비행상태를 적용하여 다음과 같이 선형화 될 수 있다.

$$
p = \dot{\phi} - \dot{\psi} \sin \Theta_0
\tag{40} 
$$

$$
q = \dot{\theta}
\tag{41} 
$$

$$
r = \dot{\psi} \cos \Theta_0
\tag{42} 
$$


트림(모멘트 평형)상태의 비행조건을 고려하면, $P_0 = Q_0 = R_0 = 0$이 성립하므로 아래와 같은 선형화된 운동방정식을 얻을 수 있다.

$$
\Delta X = m[\dot{u} + W_0 q + g \cos \Theta_0 \theta]
\tag{43} 
$$

$$
\Delta Y = m[\dot{v} + U_0 r - W_0 p - g \cos \Theta_0 \phi]
\tag{44} 
$$

$$
\Delta Z = m[\dot{w} - U_0 q - g \sin \Theta_0 \theta]
\tag{45} 
$$

$$
\Delta L = I_{xx} \dot{p} - I_{xz} \dot{r}
\tag{46} 
$$

$$
\Delta M = I_{yy} \dot{q}
\tag{47} 
$$

$$
\Delta N = I_{zz} \dot{r} - I_{xz} \dot{p}
\tag{48} 
$$



### Decoupling of Longitudinal and Lateral Equations of Motion

일반적으로 항공기의 운동방정식을 종(Longitudinal) 운동과 횡(Lateral) 운동으로 분리하여 쓸 수 있다.

#### 종방향 선형화된 운동방정식

$$
\Delta X = m[\dot{u} + W_0 q + g \cos \Theta_0 \theta]
\tag{49} 
$$

$$
\Delta Z = m[\dot{w} - U_0 q - g \sin \Theta_0 \theta]
\tag{50} 
$$

$$
\Delta M = I_{yy} \dot{q}
\tag{51} 
$$

#### 횡방향 선형화된 운동방정식

$$
\Delta Y = m[\dot{v} + U_0 r - W_0 p - g \cos \Theta_0 \phi]
\tag{52} 
$$

$$
\Delta L = I_{xx} \dot{p} - I_{xz} \dot{r}
\tag{53} 
$$

$$
\Delta N = I_{zz} \dot{r} - I_{xz} \dot{p}
\tag{54} 
$$


