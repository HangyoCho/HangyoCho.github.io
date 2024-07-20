---
title: 'Offboard Control을 위한 Pixhawk-Jetson 통신'
author: HangyoCho
date: 2024-07-17 19:56:00 +0900
categories: [4. Drone, 4.1 PX4]
tags: [UAV, PX4, Jetson, Pixhawk]
pin: false
math: true
mermaid: true
comments: true
---

## Pixhawk-Jetson
Pixhawk를 사용하면서 Jetson과 연결한 후 Mavros를 통해 제어를 진행한다.  
이때 Jetson과 Pixhawk 사이의 통신 방법에 대해 적어보려고 한다.

CUAV X7에 PX4를 업로드하여 진행 하였으며, Jetson의 경우 Orin NX를 사용하였다.

## Pixhawk-Jetson 연결 방식
Pixhawk와 Jetson 사이의 연결 방식은 크게 두 가지로 나뉜다.  
Jetson에 위치한 GPIO 핀을 이용한 UART 통신 방법과 USB to TTL 컨버터를 사용하는 방식이다.  

두 방식 모두 사용하는 Pixhawk의 Pinout을 확인한 후 Gnd와 TX, RX에 대한 와이어링을 진행해주면 된다.  
또, Jetson과 Pixhawk 사이의 TX, RX를 교차하여 연결 해주는 것도 잊지 말자.

<div style="text-align: center;">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcqN2Eg%2FbtrATdDdbq7%2FTczEayQd9pPR6raBSKTm11%2Fimg.jpg" alt="gpio"/>
  <p>using GPIO pin</p>
</div>

<div style="text-align: center;">
  <img src="https://ardupilot.org/dev/_images/ODroid_Pixhawk_Wiring.jpg" alt="gpio"/>
  <p>using USB to TTL Converter</p>
</div>

연결 방식은 두가지 방식 중 GPIO 핀을 이용한 방식을 채택하였으며 이유는 다음과 같다.

- USB to TTL 컨버터의 크기가 너무 큼
- 안정적인 통신이 가능하며 부피도 거의 차지 안함
- 드론 비행 중 발생할 수 있는 USB의 접촉 불량 가능성

## PX4 Parameter
PX4 파라미터 설정은 간단하다.  
Jetson과 연결하고자 하는 Pixhawk의 포트를 정한 후 그 포트에 대한 설정을 진행해주면 된다. 

나의 경우에는 CUAV X7의 UART 포트를 활용하고자 하여, GPS2로 설정을 진행하였다.  

### Default UART Order (CUAV X7)
- SERIAL0 = console = USB (MAVLink2)
- SERIAL1 = Telemetry1 (MAVlink2 default)= USART2 DMA-enabled
- SERIAL2 = Telemetry2 (MAVlink2 default)= USART6 DMA-enabled
- SERIAL3 = GPS1 = USART1
- SERIAL4 = GPS2 = UART4
- SERIAL5 = USER = UART8 (not available except on custom carrier boards) DMA-enabled
- SERIAL6 = USER = UART7
- SERIAL7 = USB2 (Default protocol is MAVLink2) 

##

변경한 파라미터는 아래와 같다.


```
MAV_1_CONFIG = GPS 2(202)
SER_GPS2_BAUD = 921600 8N1 (921600)
```  

## Port Permission & Mavros
기본적으로 Pixhawk와 연결된 포트를 사용하기 위하여 권한을 바꿔줄 필요가 있다.  
기본적으로 GPIO핀으로 통신을 진행한다면 ttyTHS0로 잡힐 것이다.  
USB to TTL 방식도 똑같이 해당되는 포트에 대한 권한을 풀어주면 된다.  

여기서 a+rw는 모든 사용자에게 읽기 및 쓰기 권한을 부여하는 것이다.  

```Bash
sudo chmod a+rw /dev/ttyTHS0
```
##
이후 Mavros안에 위치한 px4.launch 파일에서 **'fcu_url'** 에 내가 설정한 포트 이름과 Baudrate를 입력 해주면 통신이 가능해진다.  


```XML
<launch>
	<arg name="fcu_url" default="/dev/ttyTHS0:921600" />
	<arg name="gcs_url" default="" />
	<arg name="tgt_system" default="1" />
	<arg name="tgt_component" default="1" />
	<arg name="log_output" default="screen" />
	<arg name="fcu_protocol" default="v2.0" />
	<arg name="respawn_mavros" default="false" />
	<arg name="namespace" default="mavros"/>

	<include file="$(find-pkg-share mavros)/launch/node.launch">
		<arg name="pluginlists_yaml" value="$(find-pkg-share mavros)/launch/px4_pluginlists.yaml" />
		<arg name="config_yaml" value="$(find-pkg-share mavros)/launch/px4_config.yaml" />
		<arg name="fcu_url" value="$(var fcu_url)" />
		<arg name="gcs_url" value="$(var gcs_url)" />
		<arg name="tgt_system" value="$(var tgt_system)" />
		<arg name="tgt_component" value="$(var tgt_component)" />
		<arg name="log_output" value="$(var log_output)" />
		<arg name="fcu_protocol" value="$(var fcu_protocol)" />
		<arg name="respawn_mavros" value="$(var respawn_mavros)" />
		<arg name="namespace" value="$(var namespace)"/>
	</include>
</launch>
```