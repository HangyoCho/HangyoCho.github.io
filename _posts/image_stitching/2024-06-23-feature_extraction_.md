---
title: '[Lecture 2] - Feature Extraction'
author: HangyoCho
date: 2024-06-23 22:32:00 +0900
categories: [2. Computer Vision, 2.1 Image Stitching]
tags: [Computer Vision, Image Stitching, OpenCV, Feature Extraction]
pin: false
math: true
mermaid: true
comments: true
---

## 1. 특징점 추출 (Feature Extraction)
<div style="text-align: center;">
  <img src="https://miro.medium.com/v2/resize:fit:700/0*frzlaC71UDZkepF3.jpg" alt="Example of Feature Extraction"/>
  <p>Example of Feature Extraction</p>
</div>

이미지에서 특징을 추출하기 위해서는 이미지 내부의 독특한 패턴이나 구조를 찾아내는 것이 중요하다.  
고유한 특징을 나타내는 점의 집합을 특징점(feature point) 또는 키포인트(keypoint)이라고 한다. 특징점 주변의 부분 이미지을 잘라서 특징점에 대한 특징을 기술하는 방법을 기술자(descriptor) 또는 특징 벡터(feature vector)라고 한다. 두 개의 이미지이 같은지 다른지 판별하기 위해 매칭을 할 때 특징점을 활용한다.

<div style="text-align: center;">
  <img src="./image/feature extraction/opencv_extractor.jpg" alt="Feature Extractor on OpenCV"/>
  <p>Feature Extractor on OpenCV</p>
</div>

아래 알고리즘들은 보편적으로 사용되는 특징점 추출 알고리즘이며, OpenCV에서 여러 가지 특징점 검출 알고리즘이 구현되어 있다. Feature2D 클래스에 자식 클래스 형태로 구현되어 있으며, Feature2D 클래스에 선언된 `detect()`, `compute()`, `detectAndCompute()` 함수는 자식 클래스에서도 이용할 수 있다. SURF는 특허가 걸려있어 상업적으로 이용할 때는 특허료를 내야 하지만 SIFT는 2020년 3월 특허가 만료되어 OpenCV 4.4.0 버전부터 자유롭게 사용이 가능하다.  
따라서, 쉽게 사용 가능한 SIFT, ORB, BRISK, AKAZE 위주로 진행해 보려고 한다.

- **SIFT (Scale-Invariant Feature Transform)**: 이미지 크기와 회전에 불변성을 가지며, 다양한 상황에서 안정적인 성능을 보여준다. DoG(Difference of Gaussians)를 사용하여 다양한 스케일에서 특징점을 추출하고, 각 특징점을 128차원의 벡터로 설명한다.
- **SURF (Speeded-Up Robust Features)**: SIFT의 계산 속도를 개선한 알고리즘으로, 빠른 속도를 제공하면서도 좋은 성능을 유지한다. 해시안 행렬의 결정을 기반으로 특징점을 추출하고, 64차원 또는 128차원 벡터로 특징을 설명한다.
- **ORB (Oriented FAST and Rotated BRIEF)**: 빠르고 효율적인 특징점 검출 및 디스크립터 알고리즘으로, 특히 실시간 응용 프로그램에 적합하다. FAST 검출기와 회전에 강인한 BRIEF 설명자를 결합하여 사용한다.
- **BRISK (Binary Robust Invariant Scalable Keypoints)**: 스케일 공간에서 패치를 이용해 특징점을 검출하고, 이를 바이너리 디스크립터로 기술한다. AGAST 알고리즘을 사용하여 코너를 검출하고, 특징의 방향성을 식별하여 회전 불변성을 제공한다.
- **AKAZE (Accelerated-KAZE)**: KAZE의 변형으로, 더 빠른 처리를 가능하게 하면서도 특징점 검출 후, 해당 특징점의 방향에 따라 디스크립터를 생성한다. Fast Explicit Diffusion(FED) 프레임워크를 사용하여 계산 효율성을 높인다.

## 2. 크기 불변 특징점 검출

<div style="text-align: center;">
  <img src="./image/feature extraction/pyramid_space.png" alt="Pyramid_Scale_Space"/>
  <p>Image Pyramid and Scale Space</p>
</div>

SIFT, ORB, BRISK, AKAZE 등 다양한 특징점 검출 알고리즘은 스케일 스페이스(Scale Space) 또는 이미지 피라미드(image pyramid)를 구성하여 크기 불변 특징점을 검출한다. 스케일 스페이스는 이미지의 주요 구조를 다양한 스케일에서 포착하여, 크기 변화에 대해 불변적인 특징을 얻기 위해 사용된다. 스케일 스페이스는 보통 가우시안 블러링(Gaussian Blurring)을 반복 적용하여 구현된다. 이는 이미지의 세부 정보를 점점 더 많이 제거하고, 큰 구조만 남기게 된다. 이 과정을 통해 이미지의 다양한 스케일 버전을 생성한다.

## 3. 알고리즘 비교 분석
먼저 예제를 실행 해보기 전, 아래 논문에서 비교 분석한 내용을 바탕으로 각 알고리즘에 대해 정리 해보았다. 

["A Comparative Analysis of SIFT, SURF, KAZE,
AKAZE, ORB, and BRISK"](https://www.researchgate.net/publication/323561586_A_comparative_analysis_of_SIFT_SURF_KAZE_AKAZE_ORB_and_BRISK).

### 1. 특징점 검출 성능
- **ORB**가 가장 많은 특징점을 검출했음
- **BRISK**는 두 번째로 많은 특징점을 검출했으며, **SIFT**, **SURF**, **KAZE**, **AKAZE**보다 많은 특징점을 검출했음
- **SURF**는 **SIFT**보다 많은 특징점을 검출했음
- **AKAZE**는 일반적으로 **KAZE**보다 많은 특징점을 검출했음
- **KAZE**는 가장 적은 특징점을 검출했음
- **SURF(64D)**는 **SURF(128D)**보다 많은 특징을 매칭했음

### 2. 특징점 검출 시간
- **ORB**가 가장 효율적이며, **BRISK**도 효율적임
- **KAZE**는 가장 높은 계산 비용을 요구하며, **SIFT**의 비용은 **KAZE**의 절반 이하임
- **SURF(64D)**와 **SURF(128D)**는 **SIFT(128D)**보다 효율적임
- **AKAZE**는 **SIFT**, **SURF**, **KAZE**보다 효율적이지만 **ORB**와 **BRISK**보다 비쌈

### 3. 특징점 매칭 시간
- **SURF(128D)**는 가장 높은 매칭 비용을 요구하며
- **ORB(1000)**가 가장 적은 매칭 비용을 요구함
- **AKAZE**의 매칭 비용은 **KAZE**보다 낮음

### 4. 총 이미지 매칭 시간
- **ORB(1000)**와 **BRISK(1000)**가 가장 빠름
- **SURF(64D)**는 **SIFT**보다 빠르지만 **SURF(128D)**는 그렇지 않음
- **KAZE**는 **SIFT**, **SURF**보다 빠름

### 5. 이미지 매칭 정확도
- **SIFT**는 스케일, 회전 및 아핀 변화에 대해 가장 정확한 알고리즘으로 나타남
- **BRISK**는 스케일 및 회전 변화에 대해 두 번째로 정확했음
- **AKAZE**는 회전 및 스케일 변화에 대해 **BRISK**와 유사한 정확성을 보였음

### 6. 결론
- **특징점 검출 성능**: ORB > BRISK > SURF > SIFT > AKAZE > KAZE
- **특징점 검출 시간**: ORB > ORB(1000) > BRISK > BRISK(1000) > SURF(64D) > SURF(128D) > AKAZE > SIFT > KAZE
- **특징점 매칭 시간**: ORB(1000) > BRISK(1000) > AKAZE > KAZE > SURF(64D) > ORB > BRISK > SIFT > SURF(128D)
- **총 이미지 매칭 시간**: ORB(1000) > BRISK(1000) > AKAZE > KAZE > SURF(64D) > SIFT > ORB > BRISK > SURF(128D)

SIFT와 BRISK는 모든 종류의 기하학적 변환에 대해 가장 높은 정확도를 보인다. ORB와 BRISK는 많은 특징점을 검출할 수 있지만, 매칭 시간 때문에 전체 이미지 매칭 시간이 길어질 수 있다. 반면, ORB(1000)과 BRISK(1000)는 가장 빠른 이미지 매칭을 제공하지만 정확도가 다소 낮아진다는 것을 확인할 수 있었다. 

## 4. 특징점 추출 예제
순서대로 SIFT, ORB, BRISK, AKAZE에 대한 특징점 검출을 진행하는 예제이다.  
각 알고리즘마다 두 이미지에 대한 특징점을 추출하고 Plot 하도록 구성되어있다.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
import os

# 이미지 로드
img1 = cv2.imread('ex1.jpg', cv2.IMREAD_GRAYSCALE)
img2 = cv2.imread('ex2.jpg', cv2.IMREAD_GRAYSCALE)

# 이미지 크기를 맞추기 위해 최소 높이에 맞춰 조정
height = min(img1.shape[0], img2.shape[0])
if img1.shape[0] != height:
    ratio = height / img1.shape[0]
    img1 = cv2.resize(img1, (int(img1.shape[1] * ratio), height))
if img2.shape[0] != height:
    ratio = height / img2.shape[0]
    img2 = cv2.resize(img2, (int(img2.shape[1] * ratio), height))

# 특징점 검출기 리스트
detectors = {
    'SIFT': cv2.SIFT_create(),
    'ORB': cv2.ORB_create(nfeatures=1000),
    'BRISK': cv2.BRISK_create(),
    'AKAZE': cv2.AKAZE_create()
}

results = []

# 각 특징점 검출기마다 특징점 검출 및 저장
for name, detector in detectors.items():
    # 키포인트 및 디스크립터 검출
    kp1, des1 = detector.detectAndCompute(img1, None)
    kp2, des2 = detector.detectAndCompute(img2, None)

    # 특징점을 이미지에 그리기
    img1_with_keypoints = cv2.drawKeypoints(img1, kp1, None, color=(0, 255, 0), flags=cv2.DrawMatchesFlags_DRAW_RICH_KEYPOINTS)
    img2_with_keypoints = cv2.drawKeypoints(img2, kp2, None, color=(0, 255, 0), flags=cv2.DrawMatchesFlags_DRAW_RICH_KEYPOINTS)

    # 두 이미지를 나란히 결합
    combined_img = np.hstack((img1_with_keypoints, img2_with_keypoints))
    results.append((name, combined_img))

# 결과 Plot
plt.figure(figsize=(20, 20))
for i, (name, combined_img) in enumerate(results):
    plt.subplot(2, 2, i + 1)
    plt.imshow(cv2.cvtColor(combined_img, cv2.COLOR_BGR2RGB))
    plt.title(f'{name} Keypoints')
    plt.axis('off')

plt.tight_layout()
plt.show()
```
## 5. 특징점 추출 결과
<div style="text-align: center;">
  <img src="./image/feature extraction/feature extraction.png" alt="Feature Extraction Using OpenCV"/>
  <p>Feature Extraction Using OpenCV</p>
</div>

예제 실행 결과는 위와 같다.  
각 알고리즘을 통하여 특징점 추출을 완료하였다.  
앞서 비교 분석한 논문의 결과와는 조금 상이한 부분이 있지만 대체적으로 성공적으로 추출된 것을 볼 수 있다.  

다음 포스트에서는 추출된 특징점에 대하여 특징점 매칭(Feature Matching)을 진행해 보려 한다.
