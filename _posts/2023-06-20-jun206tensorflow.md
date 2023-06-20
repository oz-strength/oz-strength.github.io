---
layout: single
title: "Tensorflow"
categories: python
tag: [colab, ai]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# Tensorflow(텐서플로, 텐서플로우)

대규모 숫자 계산을 할 수 있도록 해주는 라이브러리

Tensor(텐서) : 다차원 행렬 (딥러닝에서 데이터를 표현하는 방식)

Tensorflow는 계산식들을 미리 만들어놓고, 데이터를 하나하나 넣어가면서 실행을 하는 구조 

파이썬을 최우선으로 지원하며, 대부분의 편한 기능들이 파이썬 라이브러리 구현이 되어있어서...



# 텐서의 종류
  - 0차원 텐서 : 스칼라(Scalar)
  - 1차원 벡터 (Vector)
  - 2차원 행렬 (Matrix)
  - 3차원 텐서 (Tensor)
  - 4차원 텐서 (Tensor)

차수(Rank)가 1씩 증가하면서 데이터 구조가 확장 

점 -> 선 -> 면 -> 입체로 가는 느낌

차수(랭크의 수) : 텐서를 구성하는 벡터의 개수 



# 스칼라 (Scalar)

정수 및 실수와 같은 상수 (Constant Number)

양을 나타내기는 하지만, 방향성을 갖지는 않음

벡터가 존재하지 않으므로 차수는 0

'랭크(rank)-0 텐서' 라고 부름 



```python
import tensorflow as tf 

# scalar 정의
#   scalar tensor는 constant() 함수에 상수값을 입력해서 만들기 

a = tf.constant(10)
b = tf.constant(20)

print(a)
print(b)

# tf.Tensor(10, shape=(), dtype=int32)
# tf.Tensor(20, shape=(), dtype=int32)
#   정수 10, 20은 텐서(tf.Tensor)로 변환
#   배열을 의미하는 shape=()에 나타내는 값이 없음 => 0차원
```

```
tf.Tensor(10, shape=(), dtype=int32)
tf.Tensor(20, shape=(), dtype=int32)
```

```python
# 랭크 확인
print(tf.rank(0)) # rank가 0인 스칼라 텐서 
```

```
tf.Tensor(0, shape=(), dtype=int32)
```

```python
# tensor 자료형 변환 :  tf.cast(바꿀 변수명, 바꿀 자료형)

a = tf.cast(a, tf.float32)
b = tf.cast(b, tf.float32)
print(a.dtype)
print(b.dtype)
# tensorflow 딥러닝 연산에서는 float32가 숫자형 데이터의 기본 자료형 
```

```
<dtype: 'float32'>
<dtype: 'float32'>
```

```python
# math모듈 : 다양한 수학적인 함수가 정의되어 있음 

# 덧셈
print(tf.math.add(a, b))

# 뺄셈
print(tf.math.subtract(a,b))

# 곱셈
print(tf.math.multiply(a,b))

# 나눗셈
print(tf.math.divide(a,b))

# 나머지
print(tf.math.mod(a,b))
```

```
tf.Tensor(30.0, shape=(), dtype=float32)
tf.Tensor(-10.0, shape=(), dtype=float32)
tf.Tensor(200.0, shape=(), dtype=float32)
tf.Tensor(0.5, shape=(), dtype=float32)
tf.Tensor(10.0, shape=(), dtype=float32)
```



# 벡터 (Vector)

스칼라 값 여러개를 요소로 갖는 1차원 배열 

스칼라 여러개가 동일한 축 방향으로 나열됨 

요소로 구성되는 다양한 값들이 모여 하나의 대표성을 갖는 값 

각 요소값들의 크기, 요소들이 나열되는 순서 두가지 모두 의미가 있음 

벡터는 방향성(하나의 축)을 가지고 있기 때문에 Rank 1, 랭크-1 텐서라고도 부름 

```python
import numpy as np
import tensorflow as tf

# 1차원 배열 정의하기 
#   constant() 함수로 1차원 배열을 정의하면, 1차원 텐서인 벡터로 변환 
#   python의 list와 numpy array 둘 다 사용 가능 O
#   vector의 shape=(요소갯수, ) 형태로 표시 됨 

l = [10, 20, 30] # python의 list
a = np.array([100., 200., 300.]) # numpy array

# 텐서 변환
v1 = tf.constant(l, dtype=tf.float32)
v2 = tf.constant(a, dtype=tf.float32)

print(v1)
print(v2)

# shape=(3,) : 1개의 축에 3개의 요소가 있다는 뜻 
```

```
tf.Tensor([10. 20. 30.], shape=(3,), dtype=float32)
tf.Tensor([100. 200. 300.], shape=(3,), dtype=float32)
```

```python
# rank 확인
print(tf.rank(v1))
print(tf.rank(v2))

# v1, v2 벡터 모두 랭크가 1인 랭크-1 텐서 
```

```
tf.Tensor(1, shape=(), dtype=int32)
tf.Tensor(1, shape=(), dtype=int32)
```

```python
# 연산 
add = tf.math.add(v1, v2)
print(add)
print(tf.rank(add))

# element-by-element 벡터연산
# 벡터 연산 : 같은 위치에 있는 요소들끼리 짝을 이루어서 계산 
# 결과물 : 요소 3개를 갖는 벡터(랭크-1텐서) 형태가 그대로 유지 
```

```
tf.Tensor([110. 220. 330.], shape=(3,), dtype=float32)
tf.Tensor(1, shape=(), dtype=int32)
```

```python
# 파이썬 내장 연산자 사용 가능 
print(v1 + v2)
print(v1 - v2)
print(v1 * v2)
print(v1 / v2)
print(v1 // v2)
print(v1 % v2)
```

```
tf.Tensor([110. 220. 330.], shape=(3,), dtype=float32)
tf.Tensor([ -90. -180. -270.], shape=(3,), dtype=float32)
tf.Tensor([1000. 4000. 9000.], shape=(3,), dtype=float32)
tf.Tensor([0.1 0.1 0.1], shape=(3,), dtype=float32)
tf.Tensor([0. 0. 0.], shape=(3,), dtype=float32)
tf.Tensor([10. 20. 30.], shape=(3,), dtype=float32)
```

```python
# reduce_sum() : vector를 구성하는 요소들의 합계 
#     그 합계는 Scalar로 표현 
print(tf.reduce_sum(v1))
print(tf.reduce_sum(v2))
```

```
tf.Tensor(60.0, shape=(), dtype=float32)
tf.Tensor(600.0, shape=(), dtype=float32)
```

```python
# square() : 거듭제곱 연산처리
#     각 요소들을 거듭제곱한 값 반환 
print(tf.math.square(v1))

# 내장 연산자로 거듭제곱
print(v1 ** 2)
```

```
tf.Tensor([100. 400. 900.], shape=(3,), dtype=float32)
tf.Tensor([100. 400. 900.], shape=(3,), dtype=float32)
```

```python
# sqrt() : 제곱근 구하기
#     각 요소들을 제곱근한 값 반환 
print(tf.math.sqrt(v2))

# 내장 연산자로 제곱근 구하기
print(v2 ** 0.5)
```

```
tf.Tensor([10.       14.142136 17.320509], shape=(3,), dtype=float32)
tf.Tensor([10.       14.142136 17.320509], shape=(3,), dtype=float32)
```

```python
# tensor는 numpy 배열의 브로드캐스팅 연산 지원 

# 브로드캐스팅 연산 : 일정 조건을 부합하는 다른 형태의 배열끼리 연산을 수행하는 것 
print(v1 + 10)

# vector에 숫자 10을 더하면, 형태는 그대로 유지된 상태에서 
# 각각의 요소에 10씩 더한 값을 반환 
```

```
tf.Tensor([20. 30. 40.], shape=(3,), dtype=float32)

```

# 행렬 (Matrix)

차수(Rank)가 1인 Vector를 같은 축 방향으로 나열하는 개념 

여러개의 1차원 vector를 요소로 갖는 2차원 배열 

랭크-2 텐서 

```python
import tensorflow as tf

# 2차원 배열 정의(list)
li_in_li=[[1,2],[3,4]] # (2,2)

# tensor 변환
matrix1 = tf.constant(li_in_li) # constant 함수에 2차원 배열 입력 

# 출력
print(matrix1)

# rank 확인
print(tf.rank(matrix1))
```

```
tf.Tensor(
[[1 2]
 [3 4]], shape=(2, 2), dtype=int32)
tf.Tensor(2, shape=(), dtype=int32)
```

```python
# stack() : 행렬 만들기
# 1차원 vector를 2개 만들어서 stack()를 이용해서 결합하는 방식

# 1차원 vector 2개 정의
v1 = tf.constant([10,20])
v2 = tf.constant([-10,30])

# tensor 변환 
matrix2 = tf.stack([v1, v2])

print(matrix2)

print(tf.rank(matrix2))
```

```
tf.Tensor(
[[ 10  20]
 [-10  30]], shape=(2, 2), dtype=int32)
tf.Tensor(2, shape=(), dtype=int32)
```

```python
# elelment-by-element 벡터 연산
calc = tf.math.multiply(matrix1, matrix2)
print(calc)
```

```
tf.Tensor(
[[ 10  40]
 [-30 120]], shape=(2, 2), dtype=int32)
```

```python
# 브로드캐스팅 연산
calc2 = tf.math.multiply(matrix1, 10)
print(calc2)\

# 2차원 행렬에 10을 곱하면, 행렬에 있는 모든 요소 * 10
#     => 행렬의 요소가 모두 10으로 구성되어있는 2차원 행렬 모양으로 확장 후 연산 진행 
```

```
tf.Tensor(
[[10 20]
 [30 40]], shape=(2, 2), dtype=int32)

```

```python
# matmul() : 선형대수에서 다루는 행렬곱 연산
# 행렬곱 : 벡터의 선형 결합
mat_mul = tf.matmul(matrix1, matrix2)
print(mat_mul)
```

```
tf.Tensor(
[[-10  80]
 [-10 180]], shape=(2, 2), dtype=int32)
```

# 고차원 텐서(Tensor)

고차원 텐서 : 축이 3개 이상 

3차원 구조를 가지는 랭크-3 텐서 

랭크-1텐서를 같은 축 방향으로 결합시키면 : 랭크-2텐서

랭크-2텐서를 같은 축 방향으로 결합시키면 : 랭크-3텐서 

=> 1차원 벡터를 나열하면 2차원 행렬이 되고, 2차원 행렬을 나열하면 3차원 텐서가 됨 

```python
import tensorflow as tf
import numpy as np

# 2차원 행렬 3개 생성

matrix3 = [[10, 20, 30, 40],
           [50, 60, 70, 80]]
matrix4 = [[11, 22, 33, 44],
           [55, 66, 77, 88]]
matrix5 = [[100, 200, 300, 400],
           [500, 600, 700, 800]]
          
# tensor 변환 - constant()

t = tf.constant([matrix3, matrix4, matrix5])

print(t)
print(tf.rank(t))

# 2차원 행렬을 요소로 갖는 1차원 벡터를 만들어서
# 랭크-3 텐서(2,4)크기의 행렬이 3개 결합이 되었음
# stack() 함수로 2차원 배열을 차곡차곡 쌓기
```

```
tf.Tensor(
[[[ 10  20  30  40]
  [ 50  60  70  80]]

 [[ 11  22  33  44]
  [ 55  66  77  88]]

 [[100 200 300 400]
  [500 600 700 800]]], shape=(3, 2, 4), dtype=int32)
tf.Tensor(3, shape=(), dtype=int32)
```







