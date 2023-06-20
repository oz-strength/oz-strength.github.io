---
layout: single
title: "Clustering"
categories: python
tag: [colab, ai]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# 1. 군집(Cluster)

군집분석 비지도 학습 방법 : 정답을 따로 알려주지 않고 비슷한 데이터끼리 묶어놓는 것 

군집의 수, 속성(label)이 사전에 알려져 있지 않을 때 사용하는 분석 방법 

clustering 이란? 데이터 간의 유사도를 정의하고 그 유사도에 가가운 것부터 순서대로 합쳐가는 방법 

사전에 label에 대한 정보를 모르기 때문에 각 개체가 어떤 군집에 들어갈가 예측하기 보다는 이렇게 나뉘어질수 있구나~ 정도의 지식을 발견하는 것에 적합 

군집은 애초에 label(category)가 없기 때문에 순수 데이터상의 특징으로 유사도를 정의해서 그룹을 만든다

# 2. 분류(Classification)

지도 학습 방법, 각 개체별로 그룹의 label이 사전에 알려져 있을 때 사용하는 분석 방법 

기존에 존재하는 데이터간의 관계를 파악하고, 새롭게 관측된 데이터의 category를 스스로 판별하는 과정 

# 3. 둘의 차이점

군집은 각 개체의 범주가 군집의 정보를 모를 때, 데이터 자체의 특성에 대해 알고자 하는 목적으로, 

분류는 label이 있을 때, 새로운 데이터의 그룹을 예측하기 위한 목적으로 하는 분석기법 

# 군집 분석 특징 
  - 종속 변수가 없는 데이터 마이닝 기법(비지도 학습)
  - 전체적인 데이터 구조를 파악하는데 이용
  - 분석 결과에 대해서 가설 검증 방법이 없음 
  - ex) 고객 DB -> 알고리즘 적용 -> 패턴 추출 -> 군집 형성

# K-means 군집화

대표적인 군집 알고리즘 

비지도학습이기 때문에 레이블이 없는 데이터들로만 작동 

데이터를 k개의 클러스터(cluster, 무리)로 분류 

=> 우리가 분류하고 싶은 클래스의 개수가 k 이다. 

모든 샘플들은 공간상에 뿌리고, k개의 중심점(centroid) 위치를 랜덤하게 설정 

k개의 클러스터 중심으로부터 모든 데이터가 얼마나 떨어져있는지 계산한 후에, 가장 가까운 클러스터의 중심을 각 데이터의 클러스터로 정해줌 

k개의 중심점을 기준으로 공간을 나눈 다음, 나누어진 각 공간에 속한 점들의 평균을 계산한 후에 가장 가까운 위치로 중심점을 이동시킨다 

계속 이 과정을 반복하다가 클러스터 중심이 이동하지 않으면 알고리즘을 종료 

# 이미지 분할 => 색상 분할 

물체 감지, 추적시스템에 사용할 수 있는 기술 

이미지 자체를 기계(컴퓨터)가 해석을 하는게 아니고 

이미지 자체를 조금 더 의미가 있는 이미지로 변경하는 것이 목표 

```python
!pip install opencv-python
```

```python
import cv2
import os
from sklearn.cluster import KMeans
from imageio import imread 
```

```python
# 이미지 하나 구하기 ! 
img = imread('cat2.jpg')
print(img) # BGR로 표현된 상태
```

```
[[[208 249 255]
  [208 249 255]
  [208 249 255]
  ...
  [208 249 255]
  [208 249 255]
  [208 249 255]]

 [[208 249 255]
  [208 249 255]
  [208 249 255]
  ...
  [208 249 255]
  [208 249 255]
  [208 249 255]]

 [[208 249 255]
  [208 249 255]
  [208 249 255]
  ...
  [208 249 255]
  [208 249 255]
  [208 249 255]]

 ...

 [[208 249 255]
  [208 249 255]
  [208 249 255]
  ...
  [208 249 255]
  [208 249 255]
  [208 249 255]]

 [[208 249 255]
  [208 249 255]
  [208 249 255]
  ...
  [208 249 255]
  [208 249 255]
  [208 249 255]]

 [[208 249 255]
  [208 249 255]
  [208 249 255]
  ...
  [208 249 255]
  [208 249 255]
  [208 249 255]]]
<ipython-input-5-27a077c6bc0e>:2: DeprecationWarning: Starting with ImageIO v3 the behavior of this function will switch to that of iio.v3.imread. To keep the current behavior (and make this warning disappear) use `import imageio.v2 as imageio` or call `imageio.v2.imread` directly.
  img = imread('cat2.jpg')
```

# 색을 표현하는 방법 : RGB (red, green, blue) : 이 세가지를 섞거나 빼서 원하는 색을 만드는 방식 

각각의 색상 0 ~ 255 사이의 값으로 표현 

값이 커질수록 해당하는 색상의 빛이 밝아짐 

RGB = (255, 255, 255) = 흰색

RGB = (0, 0, 0) = 검정색

=+==+==+==+==+==+==+==+==+==+==+==+==+==+==+==+==+==+=

OpenCV에서는 BGR : RGB와 정반대

빨간색 : RGB(255, 0, 0) / BGR(0, 0, 255)

```python
# BGR => RGB로 convert 해야 !!
convert_img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
# print(convert_img)

print(img.shape)
# (240, 427, 3) : 차원의 크기 (높이, 너비, 컬러채널 갯수)
# 컬러 채널 : 빨강, 초록, 파랑 채널
#             각각의 픽셀에 RGB 강도를 담은 3D Vector
```

```
(240, 427, 3)
```

```python
import numpy as np

vec = img.reshape(-1, 3)
vec = np.float32(vec)
vec
```

```
Array([[208., 249., 255.],
       [208., 249., 255.],
       [208., 249., 255.],
       ...,
       [208., 249., 255.],
       [208., 249., 255.],
       [208., 249., 255.]], dtype=float32)
```

```python
criteria = (cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER, 10, 1.0)

# criteria : 반복에 대한 설정, 기준에 충족되는 순간 알고리즘 반복이 멈추도록 
# 반복을 멈추게하는 종류 - 3가지 
# cv2.TERM_CRITERIA_EPS         : 특정 정확도에 이르게 되면 알고리즘 중지 
# cv2.TERM_CRITERIA_MAX_ITER    : 특정 반복 횟수가 지나면 알고리즘 중지 
# cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER
#                               : 두 조건 중 하나라도 만족하면 알고리즘 중지 

# max iter : 최대 반복 횟수를 지정하는 [정수]
# epsilon : 요구되는 특정 정확도 

K = 4 # cluster 의 수 (색상 수)
attempts = 10 # 알고리즘 실행 횟수 
ret, label, center = cv2.kmeans(vec, K, None, criteria, attempts, cv2.KMEANS_PP_CENTERS)

# ret : 밀집도 
# label : 어느 클러스터에 속하게 되는지 
# center : 클러스터의 중심 좌표 

# data  : 학습 데이터행렬(np.float32)
# K     : 군집의 개수 (cluster의 수)
# beat Labels : 각 샘플의 군집번호 행렬 - None 
# criteria : 종료기준 (type, max count, epsilon) 튜플 
# attempts : 반복 횟수 / 값이 커질수록 색상이 더 모여있는 것처럼 보임 
# flags : 초기 중앙 설정법 
# cv2.KMEANS_PP_CENTERS : 시간은 소요되지만, 정확도가 높음
# cv2.KMEANS_RANDOM_CENTERS : 정확도는 떨어지지만, 시간 소요가 덜 함 

center = np.uint8(center) # 군집화 결과를 이용해서 출력 내용을 생성함 
res = center[label.flatten()] # 중심점 좌표 받기 => 각 픽셀마다 K개 군집 중심의 색상 치환 
result = res.reshape((img.shape)) # 원본 이미지와 동일한 형태로 바꿔줌 
###################################################################################
K = 7 
attempts = 10 
ret, label, center = cv2.kmeans(vec, K, None, criteria, attempts, cv2.KMEANS_PP_CENTERS)

center = np.uint8(center) 
res = center[label.flatten()] 
result1 = res.reshape((img.shape))
###################################################################################
K = 10 
attempts = 10 
ret, label, center = cv2.kmeans(vec, K, None, criteria, attempts, cv2.KMEANS_PP_CENTERS)

center = np.uint8(center) 
res = center[label.flatten()]
result2 = res.reshape((img.shape)) 
```

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 10))

plt.subplot(2,2,1)
plt.imshow(img)
plt.title('Original IMG'), plt.xticks([]), plt.yticks([])

plt.subplot(2,2,2)
plt.imshow(result)
plt.title('K = 4'), plt.xticks([]), plt.yticks([])

plt.subplot(2,2,3)
plt.imshow(result1)
plt.title('K = 7'), plt.xticks([]), plt.yticks([])

plt.subplot(2,2,4)
plt.imshow(result2)
plt.title('K = 10'), plt.xticks([]), plt.yticks([])

plt.savefig('CAT.png')
plt.show()
```

![image-20230620173931072](/images/2023-06-20-jun204clustering/image-20230620173931072.png)







