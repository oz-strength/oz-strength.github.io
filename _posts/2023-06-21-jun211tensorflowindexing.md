---
layout: single
title: "Tensorflow_Indexing"
categories: python
tag: [colab, ai]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# 인덱싱 (indexing)

Python의 list / Numpy Array의 인덱싱 방법과 비슷 

0부터 시작, 마지막 인덱스는 -1로 

인덱스에 해당하는 범위를 지정하여 여러개의 요소로 슬라이싱(slicing) 추출도 가능 O



```python
# 텐서플로 불러오기
import tensorflow as tf
```

```python
# 1 차원 벡터의 인덱싱 

# 벡터 정의하기
vec = tf.constant([10, 20, 30, 40, 50])
print(vec)

# 0번째 요소의 스칼라 값 출력
print(vec[0])
# 마지막 요소의 스칼라값 출력 
print(vec[4])
print(vec[-1])
# 0 ~ 2번째까지의 해당하는 요소 모두 출력 
print(vec[0:3])
print(vec[:3]) # 처음부터 시작이라면 앞부분은 조건없이 : 만 사용해도 가능 O
```

```
tf.Tensor([10 20 30 40 50], shape=(5,), dtype=int32)
tf.Tensor(10, shape=(), dtype=int32)
tf.Tensor(50, shape=(), dtype=int32)
tf.Tensor(50, shape=(), dtype=int32)
tf.Tensor([10 20 30], shape=(3,), dtype=int32)
tf.Tensor([10 20 30], shape=(3,), dtype=int32)
```

```python
# 2차원 행렬(matrix) 텐서
# 행방향 인덱스와 열방향 인덱스를 지정하는 방식

# (2행, 4열) 크기의 랭크-2 텐서 정의
matrix = tf.constant([[1,2,3,4],
                     [5,6,7,8]])
print(matrix)
print(tf.rank(matrix))

# [0행, 2열]
print(matrix[0, 2])
# [0행, 모든 열]
print(matrix[0, :])
# [모든 행, 1열]
print(matrix[:,1])
# [모든 행, 모든 열]
print(matrix[:,:])
print(matrix)
```

```
tf.Tensor(
[[1 2 3 4]
 [5 6 7 8]], shape=(2, 4), dtype=int32)
tf.Tensor(2, shape=(), dtype=int32)
tf.Tensor(3, shape=(), dtype=int32)
tf.Tensor([1 2 3 4], shape=(4,), dtype=int32)
tf.Tensor([2 6], shape=(2,), dtype=int32)
tf.Tensor(
[[1 2 3 4]
 [5 6 7 8]], shape=(2, 4), dtype=int32)
tf.Tensor(
[[1 2 3 4]
 [5 6 7 8]], shape=(2, 4), dtype=int32)
```

```python
# 랭크-3 텐서

# 랭크-3 텐서 이상(고차원 텐서)의 인덱싱은
# 2차원 행렬텐서(Matrix)의 개념을 확장하는 느낌 

# (3,3,4) 크기의 텐서를 만들기 (1부터 시작)
tensor = tf.constant([[[1,2,3,4],
                       [5,6,7,8],
                       [9,10,11,12]],
                      [[13,14,15,16],
                       [17,18,19,20],
                       [21,22,23,24]],
                      [[25,26,27,28],
                       [29,30,31,32],
                       [33,34,35,36]]])
# 0번째의 모든 행, 모든 열
print(tensor[0, :, :])
# 1번째의 1번째 행까지, 2번째 열까지
print(tensor[1, :2,:3])
# 0 ~ 1번째의 2번째행까지, 2~3번째 열까지
print(tensor[:2, :3,2:4])
```

```
tf.Tensor(
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]], shape=(3, 4), dtype=int32)
tf.Tensor(
[[13 14 15]
 [17 18 19]], shape=(2, 3), dtype=int32)
tf.Tensor(
[[[ 3  4]
  [ 7  8]
  [11 12]]

 [[15 16]
  [19 20]
  [23 24]]], shape=(2, 3, 2), dtype=int32)
```

# 형태변환 (tf.reshape())

Numpy의 reshape와 상당히 유사함 

tf.reshape(값, [형태]) 



```python
import tensorflow as tf

# 랭크1-텐서(vector) 만들기 - 숫자 1 ~ 24까지 24개의 요소를 갖도록
tensor = tf.constant(range(1, 25))
print(tensor)
```

```
tf.Tensor([ 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24], shape=(24,), dtype=int32)

```

```python
# 벡터(vector) : (24, ) 형태
# 행렬(Matrix) : (4, 6) 형태로 reshape
tensor1 = tf.reshape(tensor, [4,6]) # 변환 전과 후의 요소 개수는 동일하게!
print(tensor1)
```

```
tf.Tensor(
[[ 1  2  3  4  5  6]
 [ 7  8  9 10 11 12]
 [13 14 15 16 17 18]
 [19 20 21 22 23 24]], shape=(4, 6), dtype=int32)
```

```python
# (4, 6) => (3, 8) 형태로 reshape
# tensor2 = tf.reshape(tensor1, [3,8])
tensor2 = tf.reshape(tensor1, [-1,8])
print(tensor2)

# 행 인덱스에 -1이 있는 경우 : 열을 기준으로 나누되, 행이 몇개가 되어도 상관없다는 의미
#   대신, 열의 인덱스 값은 지정이 되어있어야! 
#   요소의 개수와 배열의 형태에 따라서 행의 크기가 알아서 결정됨 
```

```
tf.Tensor(
[[ 1  2  3  4  5  6  7  8]
 [ 9 10 11 12 13 14 15 16]
 [17 18 19 20 21 22 23 24]], shape=(3, 8), dtype=int32)
```

```python
# (3, 8) => (24, )
tensor3 = tf.reshape(tensor2, [24]) # 요소의 개수를 정확히 알고 있는 경우 
tensor3 = tf.reshape(tensor2, [-1]) # 배열에 요소가 너무 많거나, 
                                    # 배열을 구성하고 있는 구조가 너무 복잡한 경우 
print(tensor3)
```

```
tf.Tensor([ 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24], shape=(24,), dtype=int32)

```



# 변수 (tf.Variable)

Tensorflow : 계산처리[그래프]라는 객체를 구축 후, 그 그래프를 실행하는 것 

변수 : 텐서플로를 활용해서 딥러닝 모델을 만들고, 학습을 시킬 때 정교한 연산을 제어하기 위해 사용 

변수에 담긴 값이 변할 가능성이 있음(바뀌어도 됨)

변수는 변화하는 학습률 값을 저장하는 그릇 



```python
# (2, 3)의 행렬 텐서 만들기
tensor = tf.constant([[10,20,30],
                      [40,50,60]])
tensor
```

```
<tf.Tensor: shape=(2, 3), dtype=int32, numpy=
array([[10, 20, 30],
       [40, 50, 60]], dtype=int32)>
```

```python
# tensor를 Variable함수에 입력 => tensorflow의 변수가 생성 

var1 = tf.Variable(tensor) # tensor : 변수 초기값

print(var1)
# 변수를 업데이트하면 초기값은 다른값으로 변경될 수 있음 

# 상수(tf.constant) : 값을 한번 지정하면 변경할 수 없음
# 변수(tf.Variable) : 텐서 구조에 저장되어 있는 값이 변경될 수 있음 
```

```
<tf.Variable 'Variable:0' shape=(2, 3) dtype=int32, numpy=
array([[10, 20, 30],
       [40, 50, 60]], dtype=int32)>
```

```python
# assign() 함수 : tensorflow변수에 새로운 데이터를 할당하는 함수 
var1.assign([[100,200,300],
             [400,500,600]])
print(var1)

# ** 주의) 새로 입력하고자 하는 배열의 크기, 자료형이
#         원래 변수에 지정되어 있는 초기값의 배열의 크기, 자료형과 동일해야 한다!!!
```

```
<tf.Variable 'Variable:0' shape=(2, 3) dtype=int32, numpy=
array([[100, 200, 300],
       [400, 500, 600]], dtype=int32)>
```

```python
# tf.convert_to_tensor() : tensorflow변수를 tensor로 변환 
# ** 텐서로 변경하고 나면 텐서의 크기, 저장하고 있는 값을 변경이 불가능 => 상수화
tensor = tf.convert_to_tensor(var1)
print(tensor)
```

```
tf.Tensor(
[[100 200 300]
 [400 500 600]], shape=(2, 3), dtype=int32)
```

```python
# tensor와 마찬가지로 tensorflow변수 역시 연산자 사용이 가능 O
var1 + tensor
```

```
<tf.Tensor: shape=(2, 3), dtype=int32, numpy=
array([[ 200,  400,  600],
       [ 800, 1000, 1200]], dtype=int32)>
```

```py
tf.__version__ # tensorflow 버전 확인하기 
```

```
2.12.0
```

```python
import tensorflow.compat.v1 as tf # tensorflow 1버전 사용
tf.compat.v1.disable_eager_execution() # 에러 방지 

# 상수 정의하기 
a1 = tf.constant(10)
b1 = tf.constant(20)
c1 = tf.constant(30)

# 변수 정의하기 
v = tf.Variable(0)

# 데이터 플로우 그래프 정의하기 (데이터의 흐름을 연산하여 결과값을 도출하는 모델)
r = a1 + b1 + c1
assign = tf.assign(v, r)

# session 실행하기 
sess = tf.Session()
sess.run(assign)

# 변수 내용을 출력 
print(sess.run(v))
```

```
60

```

# 플레이스홀더 (placeholder)

딥러닝을 할 때 데이터 학습이 이루어져야만 하는데 

이 학습을 진행할 때 학습할 데이터를 꼭! 입력을 해줘야 함 

데이터를 입력받기 위해서 필요한 비어있는 변수



```python
# placeholder의 전달 파라미터
# 실행 X 
placeholder(
    dtype,          # dtype : data type, 반드시 적어야 함!
    shape=None,     # 입력데이터의 형태 (상수값 or 배열정보)
                    # None으로 설정한 경우 어떠한 형태라도 들어갈 수 있음 
    name=None       # name은 굳이 설정 안해도 됨 
)
```

```python
import tensorflow.compat.v1 as tf
tf.disable_v2_behavior() # v2 사용 안하겠다

ph1 = tf.placeholder(dtype=tf.float32)
ph2 = tf.placeholder(dtype=tf.float32)
ph3 = tf.placeholder(dtype=tf.float32)

rr = ph1 + ph2 + ph3

sess = tf.Session()
print(sess.run(rr, feed_dict={ph1 : 10, ph2: 20, ph3 : 30}))
sess.close()

# placeholder는 다른 tensor를 할당하는 것 => tensor를 매핑한다
# 할당을 위해서 사용하는 것 : feed_dict 
```

```
WARNING:tensorflow:From /usr/local/lib/python3.10/dist-packages/tensorflow/python/compat/v2_compat.py:107: disable_resource_variables (from tensorflow.python.ops.variable_scope) is deprecated and will be removed in a future version.
Instructions for updating:
non-resource variables are not supported in the long term
60.0
```

# tf.Variable vs tf.placeholder

- tf.Variable : 데이터의 상태를 저장하기 위해 사용 

- tf.placeholder : 데이터를 입력하기 위해 사용 





