---
title: '[Flight Control 6] - Aircraft Kinematic Equation'
author: HangyoCho
date: 2024-07-17 15:35:00 +0900
categories: [3. Aerospace Engineering, 3.1 Flight Control]
tags: [Flight Control, UAV, Kinematics]
pin: false
math: true
mermaid: true
comments: true
---

## Transformation of Gravitational Force
먼저, Kinematic Equation을 도출하기 전에 간단하게 지구 중력에 의한 외력인 mg의 성분을 오일러각을 이용하여 기체 고정 좌표계에 대해 표현 할 수 있다.  
여기서 중력은 지면 고정 좌표계에서 Z축의 성분밖에 없으니 아래와 같이 표현한다.

$$
mg = mg \hat{k_1} = mg \Phi\Theta\Psi
\begin{pmatrix}
0 \\
0 \\
1
\end{pmatrix}
=
\begin{pmatrix}
-mg\sin\Theta \\
mg\sin\Phi \cos\Theta  \\
mg\cos\Phi \cos\Theta
\end{pmatrix}
\tag{1}
$$

따라서, 아래와 같이 선형 운동방정식을 표현할 수 있다.

### Equations of Linear Motion 

$$
X = m(\dot{U} + QW - VR + g\sin{\Theta})\tag{2}
$$

$$
Y = m(\dot{V} + UR - PW - g\cos{\Theta}\sin{\Phi})\tag{3}
$$

$$
Z = m(\dot{W} + VP - UQ - g\cos{\Theta}\cos{\Phi})\tag{4}
$$

## Aircraft Kinematic Equations
운동학 관계식(Kinematic Equation)은 자세각과 동체 각속도 간의 관계를 표현하며, 운동방정식과 함께 항공기의 운동을 설명한다.  
항공기의 각속도 벡터를 기체고정좌표계에서 표현하면 아래와 같다. 

$$
\omega = P\hat{i}+Q\hat{j}+R\hat{k} \tag{5}
$$

또한, 항공기의 각속도 벡터는 오일러각의 순간적인 시간변화율을 이용하여 아래와 같이 표현할 수 있다. 

$$
\omega = \omega _\Phi + \omega _ \Theta + \omega _ \Psi \tag{6}
$$

먼저, 오일러각의 시간변화율을 이용하여 표현된 각속도 백터를 기체고정좌표계로 표현하면 아래와 같다. 
오일러각은 $\dot{\Psi}$는 관성좌표계, $\dot{\Theta}$는 yaw가 변환된 상태의 좌표계, $\dot{\Phi}$는 yaw및 pitch가 변환된 상태의 좌표계에 위치한다.  

따라서, $\dot{\Psi}$의 경우 기체고정좌표계에서 표현하기 위하여 $\Phi$ 변환과 $\Theta$ 변환을, $\dot{\Theta}$는 $\Phi$ 변환을 거쳐야 한다.   

여기서, $\dot{\Phi}$는 이미 yaw 및 pitch 변환이 적용된 좌표계에서 측정되기 때문에, 기체고정좌표계에서의 각속도 방향과 일치한다. 이 경우에는 추가적인 변환이 필요 없다.  

$$
\omega _ \Phi = 
\begin{pmatrix}
\dot{\Phi} \\
0 \\
0
\end{pmatrix} 
\tag{7}
$$

$$
\omega _ \Theta = \Phi
\begin{pmatrix}
0 \\
\dot{\Theta} \\
0
\end{pmatrix} 
\tag{8}
$$

$$
\omega _ \Psi = \Phi\Theta
\begin{pmatrix}
0 \\
0 \\
\dot{\Psi}
\end{pmatrix} 
\tag{9}
$$

위 세 축의 회전변화율을 DCM(Direction Cosine Matrix)로 표현하게 되면 아래와 같다.  
아래 식 (10)은 오일러각의 시간변화율을 항공기 동체의 각속도 성분으로 변환시켜주는 관계식이다.

$$
\begin{pmatrix}
P \\
Q \\
R
\end{pmatrix} = 
\begin{pmatrix}
1 & 0 &-\sin\Theta \\
0 & \cos\Phi & \cos\Theta \sin\Phi \\
0 & -\sin\Phi & \cos\Theta \cos\Phi
\end{pmatrix}
\begin{pmatrix}
\dot{\Phi} \\
\dot{\Theta}  \\
\dot{\Psi}
\end{pmatrix} \tag{10}
$$

또한, 이 행렬은 DCM과 같은 자세변환행렬과는 달리 직교행렬이 아니기에, 역변환을 하기 위하여  아래와 같이 변환 행렬의 역행렬을 따로 도출하여 곱해줘야한다. 

$$
\begin{pmatrix}
\dot{\Phi} \\
\dot{\Theta}  \\
\dot{\Psi}
\end{pmatrix} = 
\begin{pmatrix}
1 & \sin\Phi \tan\Theta &\cos\Phi \tan\Theta \\
0 & \cos\Phi & -\sin\Phi \\
0 & \sin\Phi \sec\Theta & \cos\Phi \sec\Theta
\end{pmatrix}
\begin{pmatrix}
P \\
Q \\
R
\end{pmatrix}
\tag{11}
$$

### Kinematic Equations 
최종적으로 위 식을 성분별로 표현하면 아래와 같은 운동학 관계식이 도출된다. 

$$
P = \dot{\Phi} - \dot{\Psi} \sin{\Theta} \tag{12}
$$

$$
Q = \dot{\Theta} \cos{\Phi} + \dot{\Psi} \cos{\Theta} \sin{\Phi} \tag{13}
$$

$$
R = \dot{\Psi} \cos{\Theta} \cos{\Phi} - \dot{\Theta} \sin{\Phi} \tag{14}
$$
