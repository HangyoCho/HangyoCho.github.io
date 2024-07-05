---
title: '[Lecture 1] - Automatic Flight Control'
author: HangyoCho
date: 2024-06-24 13:30:00 +0900
categories: [3. Aerospace Engineering, 3.1 Flight Control]
tags: [FLight Control, UAV]
pin: false
math: true
mermaid: true
comments: true
---

## 1. 개요
### 1. 사용 교재
비행 동역학, 모델링 및 항공기의 제어와 관련하여 학습한 내용에 대하여 정리하며 포스트를 작성해보려고 한다. 
<div style="text-align: center;">
  <img src="https://www.kyungmoon.com/data/item/1652670429/thumb-k7460_67mE7ZaJ64Z7Jet7ZWZ67CP7KCc7Ja0_7ZGc7KeA_600x600.jpg" alt="비행동역학 및 제어 교재" width="300" height="300"/>
  <p>비행동역학 및 제어 교재</p>
</div> 

학습을 진행할 교재는 ["비행동역학 및 제어"](https://search.shopping.naver.com/book/catalog/35307703670?cat_id=50005678&frm=PBOKPRO&query=%EB%B9%84%ED%96%89%EB%8F%99%EC%97%AD%ED%95%99+%EB%B0%8F+%EC%A0%9C%EC%96%B4&NaPm=ct%3Dly8aqg5k%7Cci%3Deab4ca4427f46763c3f0c69cd48fb7ce5222de0b%7Ctr%3Dboknx%7Csn%3D95694%7Chk%3Dc1b3a59abb7bd7e62e1fba0ca3e763fb13848a44)이다. 


## 2. 비행동역학이란?

### 비행역학(Flight Mechanics)
비행역학은 공중에서 항공기의 주요 운동과 행동을 거시적으로 설명하는 학문이다. 항공기의 이륙, 상승, 선회 시점 등의 큰 거동을 포함한다. 상승률, 하강률, 활공각 등의 요소를 다루는 과목이다.

### 공기역학(Aerodynamics)
공기역학은 항공기 주위로 흐르는 공기의 세밀한 분석을 다루는 과목이다. 공기 입자의 분포, 와류 생성, 박리 현상 등 공기의 미시적인 작용과 현상을 해석하는 학문이다. 이 과목은 공기가 기체에 어떤 식으로 힘을 작용하는지를 연구한다.

### 비행동역학(Flight Dynamics)
비행동역학은 항공기의 움직임을 다각도에서 분석하는 학문이다. 이 분야는 공기역학, 추진기관, 항공기 구조역학, 동역학 등 다양한 학문분야의 내용을 기반으로 하며, 응용수학, 수치해석, 실험 등을 도구로 하여 항공기의 동특성을 해석하는 학문이다. 비행동역학은 공기역학을 통해 계산된 힘들이 항공기의 자세와 속도에 미치는 영향을 분석하고, 외부 난류와 조종사의 제어 입력 등의 외부 요인에 대한 항공기의 반응을 연구한다.


### 항공기의 동적 특성
- **안정성:** 항공기가 외부 교란 후 원래 상태로 돌아올 수 있는 능력
- **조종성:** 조종 입력에 따라 항공기가 얼마나 효과적으로 반응하는지를 나타내는 특성

## 3. 유도, 항법, 제어
### 유도 (Guidance)
유도는 항공기가 목표 지점까지의 경로를 따라 비행하도록 지시하는 시스템이다. 유도 시스템은 목표의 위치를 파악하고, 항공기가 그 위치로 이동하기 위해 경로를 따라가도록 하는 역할을 한다. 다양한 센서와 알고리즘을 사용하여 동적인 환경에서도 항공기가 정확한 경로를 유지할 수 있도록 한다.

### 항법 (Navigation)
항법은 항공기의 정확한 위치와 속도를 파악하는 과정이다. GPS 같은 위성 항법 시스템, 관성 항법 시스템, 또는 카메라나 LiDAR와 같은 센서들을 활용하여 지속적으로 위치와 속도 정보를 갱신한다. 이를 통해 항공기가 현재 어디에 있으며, 어느 방향으로 얼마나 빠르게 이동하고 있는지를 정확하게 알 수 있다.

### 제어 (Control)
제어는 항공기의 운동을 조정하여 원하는 비행 상태나 자세를 유지하게 하는 기술이다. 제어 시스템은 센서에서 수집한 데이터를 기반으로 계산된 제어 신호를 사용하여 항공기의 액추에이터(모터, 서보, 엔진 등)를 조작한다. 이를 통해 항공기의 안정적인 비행을 보장하고, 외부 환경 변화나 내부 파라미터의 변동에도 적응할 수 있도록 한다.
 
<div style="text-align: center;">
  <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgurbzqrkgg6LoYiiGEWFWr5WQarsN1d33Jwq15YQ2pcBvByR91Tlb9FYpS1qnnQHt4iXpv4UcTVkp2pqanVknmpN5w6WySf_Ay_6AUIfN3Ug1ni7Np_phkniNOu99F49KQ91sYljXu43w/s1600/gnc_block_diagram.jpg" alt="Block diagram" />
  <p>Block Diagram about Flight Control</p>
</div> 

이번 학습을 진행하며 고정익 항공기에 대한 수학적 모델링을 진행한 후 가이던스(Guidance) 및 오토파일럿(Auto-Pilot)을 Matlab의 Simulink로 구성하여 시뮬레이션을 진행하는 것을 최종 목표로 둘 것이다.
