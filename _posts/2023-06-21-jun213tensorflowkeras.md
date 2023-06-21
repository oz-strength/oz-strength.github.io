---
layout: single
title: "Tensorflow_Keras"
categories: python
tag: [colab, ai]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# Tensorflow와 Keras

Tensorflow - 구글에서 만든 Framework

# Keras

고성능 딥러닝 라이브러리 

구글에서 만든 프레임워크인 Tensorflow 안에서 Keras가 동작

# Tensorflow가 있는데 Keras도 필요한 이유?

Tensorflow는 입문자에게는 상당히 고난이도 

반면에, Keras는 사용자(개발자) 친화적으로 만들어져있어서 상대적으로 사용이 간편함 

단순한 신경망 구성 등 기존에 있는 것만으로 개발이 가능하다면 Keras만으로도 충분하지만...

디테일한 조종 등에는 한계가 있어서 Tensorflow로 함께 사용하면 조금 더 좋은 개발이 가능 !! 



```python
import tensorflow as tf
```

```python
# mnist 데이터 확보
mnist = tf.keras.datasets.mnist
```

```python
# 케라스를 이용해서 train_data, test_data 나누기
train_data, test_data = tf.keras.datasets.mnist.load_data()
#  tf.keras.datasets.mnist.load_data() :
#     학습용 데이터, 테스트용 데이터를 각각 (feature, label) 형태로 반환 

(img_train, label_train) = train_data
(img_test, label_test) = test_data

# 데이터 확인
print(train_data) # 학습용 데이터
print('----------')
print(test_data) # 테스트용 데이터
```

```

       [[0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        ...,
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0]]], dtype=uint8), array([5, 0, 4, ..., 5, 6, 8], dtype=uint8))
----------
(array([[[0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        ...,
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0]],

       [[0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        ...,
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0]],

       [[0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        ...,
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0]],

       ...,

       [[0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        ...,
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0]],

       [[0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        ...,
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0]],

       [[0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        ...,
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0]]], dtype=uint8), array([7, 2, 1, ..., 4, 5, 6], dtype=uint8))

```

```python
print(img_train.shape)
print(label_train.shape)
print(img_test.shape)
print(label_test.shape)
```

```
(60000, 28, 28)
(60000,)
(10000, 28, 28)
(10000,)
```

```python
img = img_train[150]
img.shape
```

```
(28, 28)
```

```python
import matplotlib.pyplot as plt

plt.imshow(img, 'gray')
plt.show()
```

![img](data:image/png;base64,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5iHpbkIAAPQPfeoMCACQOgggAIAJAggAYIIAAgCY6DMBtGnTJo0dO1ZDhw5VYWGh3n33XeuWrronnnhCPp8vaowfP966rYTbu3ev7r77buXl5cnn82nnzp1R651zWr9+vXJzczVs2DCVlJToyJEjNs0m0OXmYdGiRRccH3PnzrVpNkEqKio0bdo0paWlKTs7W/PmzVNdXV3UNp2dnSorK9Pw4cN13XXXacGCBWptbTXqODG+zDzMnDnzguNh+fLlRh33rk8E0CuvvKLy8nJt2LBB7733nqZMmaI5c+boxIkT1q1ddRMmTNDx48cj429/+5t1SwnX0dGhKVOmaNOmTb2u37hxo5577jm98MIL2r9/v6699lrNmTNHnZ2dV7nTxLrcPEjS3Llzo46Pl1566Sp2mHg1NTUqKyvTvn379Prrr+vs2bOaPXu2Ojo6ItusWbNGr732mrZv366amhodO3ZM9957r2HX8fdl5kGSli5dGnU8bNy40ajji3B9wPTp011ZWVnkdXd3t8vLy3MVFRWGXV19GzZscFOmTLFuw5Qkt2PHjsjrnp4eFwwG3a9+9avIsvb2duf3+91LL71k0OHV8cV5cM65hQsXunvuucekHysnTpxwklxNTY1z7tx/+8GDB7vt27dHtvnnP//pJLna2lqrNhPui/PgnHPf/va33Y9//GO7pr6EpD8DOnPmjA4ePKiSkpLIsgEDBqikpES1tbWGndk4cuSI8vLyVFBQoAcffFBNTU3WLZlqbGxUS0tL1PERCARUWFjYL4+P6upqZWdn6+abb9aKFSvU1tZm3VJChUIhSVJmZqYk6eDBgzp79mzU8TB+/HiNHj06pY+HL87D57Zu3aqsrCxNnDhR69at0+nTpy3au6ikexjpF3366afq7u5WTk5O1PKcnBx9+OGHRl3ZKCwsVGVlpW6++WYdP35cTz75pG6//XZ98MEHSktLs27PREtLiyT1enx8vq6/mDt3ru69917l5+eroaFBP/3pT1VaWqra2loNHDjQur246+np0erVqzVjxgxNnDhR0rnjYciQIcrIyIjaNpWPh97mQZK+//3va8yYMcrLy9Phw4f1k5/8RHV1dXr11VcNu42W9AGE/ystLY38efLkySosLNSYMWP05z//WUuWLDHsDMng/vvvj/x50qRJmjx5ssaNG6fq6mrNmjXLsLPEKCsr0wcffNAvroNeysXmYdmyZZE/T5o0Sbm5uZo1a5YaGho0bty4q91mr5L+I7isrCwNHDjwgrtYWltbFQwGjbpKDhkZGbrppptUX19v3YqZz48Bjo8LFRQUKCsrKyWPj5UrV2r37t166623or6+JRgM6syZM2pvb4/aPlWPh4vNQ28KCwslKamOh6QPoCFDhmjq1KmqqqqKLOvp6VFVVZWKiooMO7N36tQpNTQ0KDc317oVM/n5+QoGg1HHRzgc1v79+/v98XH06FG1tbWl1PHhnNPKlSu1Y8cOvfnmm8rPz49aP3XqVA0ePDjqeKirq1NTU1NKHQ+Xm4feHDp0SJKS63iwvgviy3j55Zed3+93lZWV7h//+IdbtmyZy8jIcC0tLdatXVVr16511dXVrrGx0b399tuupKTEZWVluRMnTli3llAnT55077//vnv//fedJPfMM8+4999/3/3nP/9xzjn39NNPu4yMDLdr1y53+PBhd88997j8/Hz32WefGXceX5eah5MnT7qHHnrI1dbWusbGRvfGG2+4b3zjG+7GG290nZ2d1q3HzYoVK1wgEHDV1dXu+PHjkXH69OnINsuXL3ejR492b775pjtw4IArKipyRUVFhl3H3+Xmob6+3v3sZz9zBw4ccI2NjW7Xrl2uoKDAFRcXG3cerU8EkHPO/fa3v3WjR492Q4YMcdOnT3f79u2zbumqu++++1xubq4bMmSIu/766919993n6uvrrdtKuLfeestJumAsXLjQOXfuVuzHH3/c5eTkOL/f72bNmuXq6upsm06AS83D6dOn3ezZs92IESPc4MGD3ZgxY9zSpUtT7n/Sevv7S3JbtmyJbPPZZ5+5H/3oR+4rX/mKu+aaa9z8+fPd8ePH7ZpOgMvNQ1NTkysuLnaZmZnO7/e7G264wT388MMuFArZNv4FfB0DAMBE0l8DAgCkJgIIAGCCAAIAmCCAAAAmCCAAgAkCCABgggACAJgggAAAJgggAIAJAggAYIIAAgCYIIAAACb+ByHQpl0IDE42AAAAAElFTkSuQmCC)

# 주요 용어

# 하이퍼파라미터(hyper-parameter)

머신러닝이나 딥러닝에서 훈련을 시킬 때 

조금 더 나은 조건에서 훈련할 수 있도록 [사용자가 직접] 설정해주는 옵션 값 

학습 속도, 반복 횟수, ... 등은 사용자가 직접 설정 

---

어떤 값을 어떻게 설정하는지에 따라서 그 모델의 성능이나 결과가 달라짐 

사용자가 따로 건들지 않으면 자동적으로 default값으로 적용 



# 에포크 (epoch)

반복 횟수 지정

데이터를 학습시키는 과정을 몇 번 반복해서 모델이 최적의 가중치를 찾아낼 수 있도록 하는 방법 



# 과소적합(underfitting) vs 과대적합(overfitting)

모델 학습에 있어서 데이터는 크게 

학습용 데이터 / 테스트(예측)용 데이터로 구분

반복적으로 학습을 시키면 모델은 사람이 발견하기 어려운 패턴을 발견하게 되어서 사람의 예측 성능보다 더 우월한 모델 생성이 가능 

예측용 데이터가 학습시킨 모델과는 다른 데이터의 분포를 가지고 있거나, 학습시킨 데이터가 한 쪽으로 치우친(편향된) 데이터라면... => 그 모델은 예측 성능이 현저히 떨어지게 됨... 

- 과소적합 : 모델이 충분히 학습을 하지 못한 경우 => 예측 성능 저하 

- 과대적합 : 학습 데이터를 지나치게 많이 반복학습을 시킨 경우 

=> 이 두 문제를 최소화하면서 정확도를 높일 수 있도록 해야함 ! 



# 딥러닝 프로세스 (Deep Learning Process)

# 1. 데이터 로드 

# 2. 데이터 전처리 (Data Preprocessing)

Data를 Model에 입력하기 전 데이터를 '가공'하는 단계 

가지고 있는 데이터 종류, 모델에 적용하려는 훈련 방법 등에 따라서 전처리 방법은 천차만별!!

경우에 따라서는 배열의 차원에 변경이 이루어지기도 하고, 스케일의 조정이 이루어지기도 함 

데이터 전처리를 제대로 진행해야 다음에 나올 모델생성에서 올바르게 모델정의를 할 수 있음 



# 3. 데이터 분할(학습용, 테스트용)

# 4. 모델 생성 

모델의 구조를 파악하고 만들어내는 단계 
- 모델 생성 방법 
  - Sequential API : 순차적 구조의 모델
  - Functional API, Model Subclassing : 다중 입력, 출력을 가지고 있는 복잡한 구조의 모델 

# 5. 모델 컴파일 (compile)

만들어진 모델 훈련에 사용할 다양한 옵션 설정 

.compile() 함수를 사용
- 손실함수(loss)
- 옵티마이저(optimizer)
- 평가지표(metrics)

# 6. 모델 훈련

.fit() 함수에 모델 훈련에 필요한 정보를 파라미터로 전달 

- 훈련 데이터셋
- 검증 데이터셋
- 배치(batch)
- 크기
- 콜백(call-back)

# 7. 모델 검증 (evaluate)

훈련이 끝난 모델이 얼마나 정확한지 검증하는 단계 

모델 훈련시에 사용하지 않은 데이터셋을 모델에 입력시킨 후에, 그 모델이 예측한 값과 정답을 비교해서 평가지표를 내림

이 검증결과를 바탕으로 모델 생성단계로 돌아가서 컴파일, 모델 수정 등이 이루어짐 

목표한 성능에 도달할 때까지 계속 그 과정이 반복(4번부터)

# 8. 예측 (predict)

test용 데이터를 입력해서 모델 예측값을 얻는 과정

# 2, 3, 4, 5번의 경우에는 모델 학습을 위한 필수 프로세스로 절대 빼먹어서는 안됨!!!



```python
import tensorflow as tf
from tensorflow import keras
import numpy as np
import matplotlib.pyplot as plt
```

```python
# mnist dataset 가져오기
from tensorflow.keras import datasets
mnist = datasets.mnist
```

```python
# train용, test용 나누기 
(train_data, train_label), (test_data, test_label) = mnist.load_data()
```

```python
# 이미지 한장의 픽셀값의 최대, 최소값 
# 정규화 하기 전 
print(train_data.min()) # 최소값 : 0
print(train_data.max()) # 최소값 : 255
```

```
0
255
```

```python
# 이미지에 대한 내용을 정규화 
train_data, test_data = train_data / train_data.max(), test_data / train_data.max()

# 정규화 한 후에 train_data의 배열
train_data[0, 5:10, 5:10] # 정규화 후에 데이터는 모두 0 ~ 1 사이

# 정규화 : 데이터 값의 전체 범위를 0 ~ 1 사이의 값으로 조정하는 것  
```

```
array([[0.        , 0.        , 0.        , 0.        , 0.        ],
       [0.        , 0.        , 0.        , 0.11764706, 0.14117647],
       [0.        , 0.        , 0.19215686, 0.93333333, 0.99215686],
       [0.        , 0.        , 0.07058824, 0.85882353, 0.99215686],
       [0.        , 0.        , 0.        , 0.31372549, 0.61176471]])
```

```python
# 모델 생성 

# Sequential() : 순차적 구조의 모델 >> 구조를 설정하고 층(layer)을 설정하는 부분
# [] : 대괄호 안에 있는 부분이 layer!
model = tf.keras.models.Sequential([
    tf.keras.layers.Flatten(input_shape=(28, 28)), # 1차원(vector) 으로 평평하게 만들기
    # input_shape << 꼭 설정해야함!
    # (28, 28) 크기의 matrix가 들어갈테니 이걸 1차원 vector로 펴달라는 얘기! 

    tf.keras.layers.Dense(128, activation='relu'), # 은닉층(중간층)에 신경망을 만드는 단계
    # (128개짜리 퍼셉트론(뉴런)으로 구성되는 layer하나짜리)
    # activation='relu' : 은닉층의 활성화 함수 

    tf.keras.layers.Dropout(0.2), # 과대적합을 막아주는 역할
    # 128개의 신경망 중에 무작위로 20%는 0으로 만들어줌  

    tf.keras.layers.Dense(10, activation='softmax')
    # 10개의 퍼셉트론으로 만든 레이어
    # 10개의 값을 출력하는 이유 : 
    #   0 ~ 9까지 총 10개의 이미지가 어떤 숫자인지를 파악하기 위해서 
    # activation='softmax' : 마지막 층의 결과값을 다중분류를 위한 확률 값으로 계산할 수 있도록!

    # 활성화 함수(activation) : 결과값을 변환해서 다른층으로 보내주는 역할 
      # 'linear' : 기본값, 입력뉴런과 가중치로 계산된 결과값이 그대로 출력
      # 'relu' : (Rectified Linear Unit 함수), 은닉층에서 주로 사용
      # 'sigmoid' : 시그모이드 함수, 이진 분류 - 출력층에서 주로 사용 
      # 'softmax' : 소프트맥스 함수, 다중 분류 - 출력층에서 주로 사용      
])
print(model.summary()) # 모델의 구조 확인 
```

```
Model: "sequential"
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 flatten (Flatten)           (None, 784)               0         
                                                                 
 dense (Dense)               (None, 128)               100480    
                                                                 
 dropout (Dropout)           (None, 128)               0         
                                                                 
 dense_1 (Dense)             (None, 10)                1290      
                                                                 
=================================================================
Total params: 101,770
Trainable params: 101,770
Non-trainable params: 0
_________________________________________________________________
None
```

```python
# 컴파일
# .compile : 학습 방법에 대한 설정 
#   sequential에서 정해진 모델을 컴퓨터가 알아들을 수 있는 말로 컴파일하는 부분 
model.compile(
    optimizer='adam', # 최적화하기
    # 옵티마이저는 손실 함수를 통해서 얻어낸 손실값으로부터 모델을 업데이트하는 방식 
    # 그 중에서 adam 이라는 옵티마이저를 사용 
    loss='sparse_categorical_crossentropy',
    # 손실함수(loss function) : 모델을 최적화시킬때 사용하는 함수 
    # 손실함수는 신경망의 예측이 얼마나 잘 맞는지 측정하는 역할 
    # categorical_crossentropy : 다중분류 손실함수 (one-hot encoding 클래스)
    #   출력값이 one-hot encoding 된 결과 
    # sparse_categorical_crossentropy : 다중분류 손실함수 (one-hot encoding이 아닌)
    #   출력값이 integer type 클래스로 반환
    # binary_crossentropy : 클래스가 2개인 이진 분류 손실함수 
    #   label이 0 또는 1을 값으로 가질 때 사용 
    metrics = ['accuracy'] # 정확도 확인
)
```

```python
# training, evaluatiion (훈련, 검증)
# .fit() : model을 실제로 학습시키는 단계 
model.fit(train_data, train_label, epochs=10) # 6만개의 데이터를 10번 반복훈련 하겠다!

# 훈련데이터에서 사용하지 않았던 test용 데이터로 검증/평가
model.evaluate(test_data, test_label)
```

```
Epoch 1/10
1875/1875 [==============================] - 8s 3ms/step - loss: 0.2886 - accuracy: 0.9161
Epoch 2/10
1875/1875 [==============================] - 7s 4ms/step - loss: 0.1387 - accuracy: 0.9590
Epoch 3/10
1875/1875 [==============================] - 6s 3ms/step - loss: 0.1063 - accuracy: 0.9678
Epoch 4/10
1875/1875 [==============================] - 7s 4ms/step - loss: 0.0878 - accuracy: 0.9731
Epoch 5/10
1875/1875 [==============================] - 6s 3ms/step - loss: 0.0750 - accuracy: 0.9773
Epoch 6/10
1875/1875 [==============================] - 7s 4ms/step - loss: 0.0662 - accuracy: 0.9793
Epoch 7/10
1875/1875 [==============================] - 6s 3ms/step - loss: 0.0586 - accuracy: 0.9815
Epoch 8/10
1875/1875 [==============================] - 7s 4ms/step - loss: 0.0529 - accuracy: 0.9826
Epoch 9/10
1875/1875 [==============================] - 6s 3ms/step - loss: 0.0484 - accuracy: 0.9842
Epoch 10/10
1875/1875 [==============================] - 7s 4ms/step - loss: 0.0438 - accuracy: 0.9855
313/313 [==============================] - 1s 2ms/step - loss: 0.0738 - accuracy: 0.9783
[0.07384463399648666, 0.9782999753952026]
```

```python
# data 탐색

# 훈련용데이터의 길이만큼의 숫자 중에 랜덤한 숫자 하나 뽑아와서...
index = np.random.randint(len(train_data))

# 해당하는 숫자가 있는 train_data의 데이터를 img2라는 변수에 넣음 
img2 = train_data[index]

plt.imshow(img2, 'gray')
plt.title(train_label[index])
plt.show()
```

![img](data:image/png;base64,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FyorK0vLli1TRkaG5syZE8m5AQB9nOcA7d69W1dffXXo66KiIknSggULtG7dOt13331qaWnR7bffrqamJl1xxRXaunWrhg4dGrmpAQB9XoxzzlkP8UXBYFB+v996DPQiTz31lOc1d955ZxQm6VxX72+ezscffxyFSYDeJRAInPZ9ffNPwQEABiYCBAAwQYAAACYIEADABAECAJggQAAAEwQIAGCCAAEATBAgAIAJAgQAMEGAAAAmCBAAwAQBAgCY8PzrGICzkZmZ6XnNggULojBJ57Zv3+55TVNTU+QH6cTYsWM9r8nLy+vWY916663dWufVypUrPa/ZsGFDFCaBBc6AAAAmCBAAwAQBAgCYIEAAABMECABgggABAEwQIACACQIEADBBgAAAJggQAMAEAQIAmCBAAAATXIwUPeq5557zvCYxMdHzmvr6es9rJKmwsNDzmvb2ds9rfvCDH3he8+STT3pec/7553te05Neeuklz2smTpzoec2yZcs8r0H0cQYEADBBgAAAJggQAMAEAQIAmCBAAAATBAgAYIIAAQBMECAAgAkCBAAwQYAAACYIEADABAECAJjgYqToUXFxcT3yOIcPH+7WuqqqKs9r7rjjDs9rioqKPK/JzMz0vKa3i4mJ8bwmJycnCpPAAmdAAAATBAgAYIIAAQBMECAAgAkCBAAwQYAAACYIEADABAECAJggQAAAEwQIAGCCAAEATBAgAIAJLkaKfikpKalb65544gnPa+655x7PawYNGuR5TXe89tpr3VrX1tbmec3w4cM9r5kzZ47nNeg/OAMCAJggQAAAE54DtGPHDs2aNUsZGRmKiYnR5s2bw+6/+eabFRMTE7bNnDkzUvMCAPoJzwFqaWnR5MmTtXr16i73mTlzpg4dOhTaXn755bMaEgDQ/3j+EEJBQYEKCgpOu4/P51NaWlq3hwIA9H9ReQ+orKxMKSkpGj9+vBYvXqwjR450uW9bW5uCwWDYBgDo/yIeoJkzZ+rFF19UaWmpnnzySZWXl6ugoEAnTpzodP+SkhL5/f7QNnr06EiPBADohSL+94Cuv/760J8vueQSTZo0SePGjVNZWZmmT59+yv7FxcUqKioKfR0MBokQAAwAUf8Y9tixY5WcnKyqqqpO7/f5fEpISAjbAAD9X9QDdPDgQR05ckTp6enRfigAQB/i+SW4o0ePhp3N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zs6WJFVVVWncuHGn3O/z+eTz+bozBgCgD/MUIOec7rjjDm3atEllZWXKyso645q9e/dKktLT07s1IACgf/IUoMLCQq1fv15btmxRfHy8GhoaJEl+v1/Dhg1TdXW11q9fr+9///s699xztW/fPt11112aNm2aJk2aFJV/AABAH+XlfR918Trf2rVrnXPO1dbWumnTprmkpCTn8/ncBRdc4O69994zvg74RYFAwPx1SzY2Nja2s9/O9LM/5v+HpdcIBoPy+/3WYwAAzlIgEFBCQkKX93MtOACACQIEADBBgAAAJggQAMAEAQIAmCBAAAATBAgAYIIAAQBMECAAgAkCBAAwQYAAACYIEADABAECAJggQAAAEwQIAGCCAAEATBAgAIAJAgQAMEGAAAAmCBAAwAQBAgCYIEAAABMECABgggABAEwQIACAiV4XIOec9QgAgAg408/zXheg5uZm6xEAABFwpp/nMa6XnXJ0dHSovr5e8fHxiomJCbsvGAxq9OjRqqurU0JCgtGE9jgOJ3EcTuI4nMRxOKk3HAfnnJqbm5WRkaHY2K7Pcwb34ExfSWxsrEaNGnXafRISEgb0E+xzHIeTOA4ncRxO4jicZH0c/H7/GffpdS/BAQAGBgIEADDRpwLk8/m0fPly+Xw+61FMcRxO4jicxHE4ieNwUl86Dr3uQwgAgIGhT50BAQD6DwIEADBBgAAAJggQAMAEAQIAmOgzAVq9erXOP/98DR06VNnZ2XrvvfesR+pxDz30kGJiYsK2CRMmWI8VdTt27NCsWbOUkZGhmJgYbd68Oex+55wefPBBpaena9iwYcrLy9OBAwdsho2iMx2Hm2+++ZTnx8yZM22GjZKSkhJddtllio+PV0pKiubMmaPKysqwfVpbW1VYWKhzzz1XI0aM0Lx589TY2Gg0cXR8leOQm5t7yvNh0aJFRhN3rk8EaMOGDSoqKtLy5cv1/vvva/LkycrPz9fhw4etR+txF198sQ4dOhTa3nnnHeuRoq6lpUWTJ0/W6tWrO71/xYoVevrpp/Xss89q165dOuecc5Sfn6/W1tYenjS6znQcJGnmzJlhz4+XX365ByeMvvLychUWFmrnzp3atm2b2tvbNWPGDLW0tIT2ueuuu/T6669r48aNKi8vV319va677jrDqSPvqxwHSVq4cGHY82HFihVGE3fB9QFTp051hYWFoa9PnDjhMjIyXElJieFUPW/58uVu8uTJ1mOYkuQ2bdoU+rqjo8OlpaW5X/ziF6HbmpqanM/ncy+//LLBhD3jy8fBOecWLFjgZs+ebTKPlcOHDztJrry83Dl38t/9kCFD3MaNG0P7/POf/3SSXEVFhdWYUffl4+Ccc1dddZW788477Yb6Cnr9GdDx48e1Z88e5eXlhW6LjY1VXl6eKioqDCezceDAAWVkZGjs2LG66aabVFtbaz2SqZqaGjU0NIQ9P/x+v7Kzswfk86OsrEwpKSkaP368Fi9erCNHjliPFFWBQECSlJSUJEnas2eP2tvbw54PEyZMUGZmZr9+Pnz5OHzupZdeUnJysiZOnKji4mIdO3bMYrwu9bqrYX/ZJ598ohMnTig1NTXs9tTUVP3rX/8ymspGdna21q1bp/Hjx+vQoUN6+OGHdeWVV2r//v2Kj4+3Hs9EQ0ODJHX6/Pj8voFi5syZuu6665SVlaXq6mo98MADKigoUEVFhQYNGmQ9XsR1dHRo6dKluvzyyzVx4kRJJ58PcXFxSkxMDNu3Pz8fOjsOknTjjTdqzJgxysjI0L59+3T//fersrJSf/zjHw2nDdfrA4T/U1BQEPrzpEmTlJ2drTFjxujVV1/VbbfdZjgZeoPrr78+9OdLLrlEkyZN0rhx41RWVqbp06cbThYdhYWF2r9//4B4H/R0ujoOt99+e+jPl1xyidLT0zV9+nRVV1dr3LhxPT1mp3r9S3DJyckaNGjQKZ9iaWxsVFpamtFUvUNiYqIuuugiVVVVWY9i5vPnAM+PU40dO1bJycn98vmxZMkSvfHGG3r77bfDfn9YWlqajh8/rqamprD9++vzoavj0Jns7GxJ6lXPh14foLi4OE2ZMkWlpaWh2zo6OlRaWqqcnBzDyewdPXpU1dXVSk9Ptx7FTFZWltLS0sKeH8FgULt27Rrwz4+DBw/qyJEj/er54ZzTkiVLtGnTJr311lvKysoKu3/KlCkaMmRI2POhsrJStbW1/er5cKbj0Jm9e/dKUu96Plh/CuKreOWVV5zP53Pr1q1z//jHP9ztt9/uEhMTXUNDg/VoPeruu+92ZWVlrqamxr377rsuLy/PJScnu8OHD1uPFlXNzc3ugw8+cB988IGT5J566in3wQcfuA8//NA559zPf/5zl5iY6LZs2eL27dvnZs+e7bKystynn35qPHlkne44NDc3u3vuucdVVFS4mpoat337dvetb33LXXjhha61tdV69IhZvHix8/v9rqyszB06dCi0HTt2LLTPokWLXGZmpnvrrbfc7t27XU5OjsvJyTGcOvLOdByqqqrcI4884nbv3u1qamrcli1b3NixY920adOMJw/XJwLknHPPPPOMy8zMdHFxcW7q1Klu586d1iP1uPnz57v09HQXFxfnzjvvPDd//nxXVVVlPVbUvf32207SKduCBQuccyc/ir1s2TKXmprqfD6fmz59uqusrLQdOgpOdxyOHTvmZsyY4UaOHOmGDBnixowZ4xYuXNjv/iets39+SW7t2rWhfT799FP3k5/8xH3ta19zw4cPd3PnznWHDh2yGzoKznQcamtr3bRp01xSUpLz+XzuggsucPfee68LBAK2g38Jvw8IAGCi178HBADonwgQAMAEAQIAmCBAAAATBAgAYIIAAQBMECAAgAkCBAAwQYAAACYIEADABAECAJj4f/jqFU0Kr9N5AAAAAElFTkSuQmCC)



# 내가 만든 그림으로 확인하기 

그림판으로 숫자하나 그려서 저장

그림판의 크기 = (28, 28)



```python
import os
from PIL import Image
from google.colab import files

upload = files.upload()
```

```python
# 경로 설정
dir = os.getcwd()
imgPath = os.path.join(dir, 'four.png')

# 파일 읽기
nowImg = Image.open(imgPath)
# print(nowImg)

# 28 x 28 로 사이즈 변환
nowImg = nowImg.resize((28,28))
myImg = np.asarray(nowImg) # array 형식으로 형변환

# 컬러이미지의 경우 RGB 평균값으로 바꾸기
try:
  myImg = np.mean(myImg, axis=2)
except:
  pass

# 이미지의 RGB평균값을 절대값으로 계산해서 다시 이미지에 넣는 작업
myImg = np.abs(255 - myImg)
myImg = myImg.astype(np.float32) / 255.

plt.imshow(myImg, 'gray')
plt.show()
```

![img](data:image/png;base64,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eAYEADBBgAAAJggQAMAEAQIAmCBAAAATBAgAYIIAAQBMECAAgAkCBAAwQYAAACYIEADABAECAJjwOeec9RDfFg6HFQwGrcdICW1tbTEfEwqFkjBJ4sRzYdH169cnYZJHQ4p9effh8/msR0ASeZ6nQCAw4HaeAQEATBAgAICJmAN08uRJLV68WIWFhfL5fDpw4EDUduectm7dqoKCAo0ePVplZWW6dOlSouYFAGSImAPU1dWl2bNna+fOnf1u37Fjhz744APt2rVLp0+f1mOPPaZFixapu7t70MMCADJHzL8RtaKiQhUVFf1uc87p/fff169+9SstWbJEkvTRRx8pPz9fBw4c0EsvvTS4aQEAGSOhrwE1Nzervb1dZWVlkduCwaBKSkpUX1/f7zE9PT0Kh8NRCwCQ+RIaoPb2dklSfn5+1O35+fmRbd9VVVWlYDAYWRMnTkzkSACAFGX+LrgtW7bI87zIam1ttR4JADAEEhqgb34IsqOjI+r2jo6OAX9A0u/3KxAIRC0AQOZLaICKi4sVCoVUW1sbuS0cDuv06dMqLS1N5F0BANJczO+Cu3HjhhobGyMfNzc36/z588rNzdWkSZO0ceNG/eY3v9FTTz2l4uJivfPOOyosLNTSpUsTOTcAIM3FHKAzZ87of/7nfyIfb968WZK0atUqVVdX66233lJXV5fWrl2r69ev6/nnn9fhw4c1atSoxE0NAEh7XIx0iJSXl8d8zGeffZaESRKHC4umvhT78u6Di5FmNi5GCgBISQQIAGCCAAEATBAgAIAJAgQAMEGAAAAmCBAAwAQBAgCYIEAAABMECABgggABAEwQIACACQIEADDB1bDjEM+VrWtqamI+JicnJ+ZjAKSfr776KuZjtm/fHtd9VVdXx3VcPLgaNgAgJREgAIAJAgQAMEGAAAAmCBAAwAQBAgCYIEAAABMECABgggABAEwQIACACQIEADBBgAAAJrgYaRyam5tjPmby5MmJHwTAI6u7uzuu40aPHp3gSQbGxUgBACmJAAEATBAgAIAJAgQAMEGAAAAmCBAAwAQBAgCYIEAAABMECABgggABAEwQIACACQIEADAxwnqAdPThhx/GfExVVVUSJgHwqKqurrYeYdB4BgQAMEGAAAAmCBAAwAQBAgCYIEAAABMECABgggABAEwQIACACQIEADBBgAAAJggQAMAEAQIAmPA555z1EN8WDocVDAatxwAyQop9effh8/msR0ASeZ6nQCAw4HaeAQEATBAgAICJmAN08uRJLV68WIWFhfL5fDpw4EDU9ldffVU+ny9qlZeXJ2peAECGiDlAXV1dmj17tnbu3DngPuXl5Wpra4usmpqaQQ0JAMg8Mf9G1IqKClVUVNx3H7/fr1AoFPdQAIDMl5TXgE6cOKHx48dr+vTpWr9+va5duzbgvj09PQqHw1ELAJD5Eh6g8vJyffTRR6qtrdXvfvc71dXVqaKiQnfv3u13/6qqKgWDwciaOHFiokcCAKSgQf0ckM/n0/79+7V06dIB9/n3v/+tqVOn6ujRo1qwYEGf7T09Perp6Yl8HA6HiRCQIPwcECyZ/xzQlClTlJeXp8bGxn63+/1+BQKBqAUAyHxJD9Dly5d17do1FRQUJPuuAABpJOZ3wd24cSPq2Uxzc7POnz+v3Nxc5ebmavv27VqxYoVCoZCampr01ltvadq0aVq0aFFCBwcApDkXo+PHjztJfdaqVavczZs33cKFC924ceNcVlaWKyoqcmvWrHHt7e0P/fk9z+v387NYrNhXqrM+P6zkLs/z7vv3z8VIgQyWYl/effAmhMxm/iYEAAD6Q4AAACYIEADABAECAJggQAAAEwQIAGCCAAEATBAgAIAJAgQAMEGAAAAmCBAAwAQBAgCYIEAAABMECABgggABAEwQIACACQIEADBBgAAAJggQAMAEAQIAmCBAAAATBAgAYIIAAQBMECAAgAkCBAAwQYAAACYIEADABAECAJggQAAAEwQIAGCCAAEATBAgAIAJAgQAMEGAAAAmCBAAwAQBAgCYIEAAABMECABgggABAEwQIACACQIEADBBgAAAJggQAMAEAQIAmCBAAAATBAgAYIIAAQBMECAAgAkCBAAwQYAAACYIEADABAECAJggQAAAEwQIAGCCAAEATMQUoKqqKs2dO1fZ2dkaP368li5dqoaGhqh9uru7VVlZqbFjx+rxxx/XihUr1NHRkdChAQDpL6YA1dXVqbKyUqdOndKRI0d0584dLVy4UF1dXZF9Nm3apE8//VT79u1TXV2drly5ouXLlyd8cABAmnODcPXqVSfJ1dXVOeecu379usvKynL79u2L7PPll186Sa6+vv6hPqfneU4Si8VKwEp11ueHldzled59//4H9RqQ53mS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WpeTk+Pa29utRxtSv/jFL9yJEydcc3Oz+/vf/+7KyspcXl6eu3r1qvVoSdXZ2enOnTvnzp075yS59957z507d8795z//cc4599vf/tbl5OS4gwcPugsXLrglS5a44uJid+vWLePJE+t+56Gzs9O98cYbrr6+3jU3N7ujR4+673//++6pp55y3d3d1qMnzPr1610wGHQnTpxwbW1tkXXz5s3IPuvWrXOTJk1yx44dc2fOnHGlpaWutLTUcOrEe9B5aGxsdL/+9a/dmTNnXHNzszt48KCbMmWKmz9/vvHk0dIiQM4594c//MFNmjTJjRw50s2bN8+dOnXKeqQht3LlSldQUOBGjhzpnnzySbdy5UrX2NhoPVbSHT9+3Enqs1atWuWcu/dW7Hfeecfl5+c7v9/vFixY4BoaGmyHToL7nYebN2+6hQsXunHjxrmsrCxXVFTk1qxZk3H/k9bfn1+S2717d2SfW7duuddee8098cQTbsyYMW7ZsmWura3NbugkeNB5aGlpcfPnz3e5ubnO7/e7adOmuTfffNN5nmc7+Hfw6xgAACZS/jUgAEBmIkAAABMECABgggABAEwQIACACQIEADBBgAAAJggQAMAEAQIAmCBAAAATBAgAYIIAAQBM/C82ueKckNUOYAAAAABJRU5ErkJggg==)



```python
# shape를 변경해서 학습된 model과 비교
myImg = np.reshape(myImg, (1,28,28))
print(model.predict(myImg))

print('Model이 예측한 값은 {} !!'.format(np.argmax(model.predict(myImg))))
```

```
1/1 [==============================] - 0s 23ms/step
[[2.0703164e-04 4.9142255e-07 1.8021543e-06 5.9489048e-06 7.2535425e-01
  1.9333778e-02 3.7919203e-04 2.7410900e-02 6.9341063e-02 1.5796559e-01]]
1/1 [==============================] - 0s 25ms/step
Model이 예측한 값은 4 !!

```





















