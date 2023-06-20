---
layout: single
title: "NaiveBayes"
categories: python
tag: [colab, ai]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# 나이브베이즈 분류 (Naive Bayes Classification)

지도학습 중 분류기법의 하나 

대표적으로 사용되는 곳 : 스팸메일 구분하는 필터, 텍스트 분류, 추천 시스템, 감정분석, ...

머신러닝 - 지도학습 : 

Feature, Label 파악이 중요 

Label : 우리가 원하는 분류 예) 치마, 반바지, 긴바지, 모자 등등 

Feature : 디자인, 모양, 색깔, 원단, 질감, ...

```python
# 날씨에 따라서 긴바지를 입을지 반바지를 입을지 

# 'sunny', 'snow', 'overcast'
weather = ['sunny', 'sunny', 'overcast', 'snow', 'overcast', 'snow','snow',
           'sunny', 'overcast', 'overcast', 'snow', 'sunny', 'snow', 'sunny']

# 'mild', 'cool', 'cold'
temp = ['mild', 'cool', 'cold', 'cold', 'cold', 'cool', 'mild',
        'mild', 'cool', 'cold', 'cool', 'mild', 'mild', 'mild', 'cool']

# 긴바지는 : 'long', 반바지는 : 'short'
pants = ['short', 'short', 'long', 'long', 'long', 'short', 'long',
         'short', 'short', 'long', 'short', 'long', 'long', 'short']
```

# 컴퓨터가 이해하는 것 : 0, 1

어휘추출(feature encoding) - String을 int로 

자연어(text)로 되어있는 문서를 컴퓨터가 이해하는 숫자로 변환 !

```python
# feature encoding

from sklearn import preprocessing

# LabelEncoder() : 문자를 0부터 시작하는 정수형 문자로 바꿔주는 기능
le = preprocessing.LabelEncoder()

# fit_transform() : train_dataset에서만 사용
#   fit : 평균, 표준편차를 계싼
#   transform : 정규화 작업 
weather_encoder = le.fit_transform(weather)
print(weather_encoder)
# 'sunny' : 2
# 'overcast' : 0
# 'snow' : 1
```

```
[2 2 0 1 0 1 1 2 0 0 1 2 1 2]
```

```python
temp_encoder = le.fit_transform(temp)
print(temp_encoder)
# 'mild' : 2
# 'cool' : 1 
# 'cold' : 0
```

```
[2 1 0 0 0 1 2 2 1 0 1 2 2 2 1]
```

```python
label = le.fit_transform(pants)
print(label)
# 'short' : 1
# 'long' : 0
```

```
[1 1 0 0 0 1 0 1 1 0 1 0 0 1]
```

```python
# encoding된 weather와 temp를 결합 (feature-weather + temp)

features = zip(weather_encoder, temp_encoder)
features= list(features)
print(features)
```

```
[(2, 2), (2, 1), (0, 0), (1, 0), (0, 0), (1, 1), (1, 2), (2, 2), (0, 1), (0, 0), (1, 1), (2, 2), (1, 2), (2, 2)]
```

```python
# 모델 만들기 => 훈련 데이터를 fit => 성능 평가
from sklearn.naive_bayes import GaussianNB

# Naive Bayes : 각각의 특성을 개별적으로 취급해서 parameter를 학습시키고, 
#               그 특성에서 클래스별 통계를 단순 취합
# GaussianNB : 연속적으로 나오는 데이터가 있다면 적용 가능 
# BernoulliNB : 이진(Binary) 데이터에 적용 가능 
# MultinomiaNB : 카운트 데이터에 적용 가능 

# 모델 만들기 
model = GaussianNB()

# 훈련 데이터 fit
model.fit(features, label) # 훈련 데이터의 평균, 표준편차 구함

# 성능 평가 
predict = model.predict([[1, 0]]) # 눈이오고 추운날에는 무슨 옷을 입어야 할까?
print(predict) # 0 => 'long' 긴바지 !
```

```
[0]
```

# Iris 품종 

```python
from sklearn import datasets
iris = datasets.load_iris()
print(type(iris))
# Bunch 클래스 : {'data' : [], 'target' : [] }
# data : features
# target : label 
print(iris.data.shape)
print(iris.feature_names)
print()
print(iris.target_names)
```

```
<class 'sklearn.utils._bunch.Bunch'>
(150, 4)
['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']

['setosa' 'versicolor' 'virginica']
```

```python
X = iris.data
y = iris.target

X,y=datasets.load_iris(return_X_y=True)
# return_X_y=True : numpy, ndarray들의 튜플(data, target)을 리턴시켜라
```

```python
from sklearn.model_selection import train_test_split
# data 나누기
X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.2)
```

```python
# data 학습시키기
from sklearn.naive_bayes import GaussianNB
gg = GaussianNB()

gg.fit(X_train, y_train)
```

```
GaussianNB
GaussianNB()
```

```python
# 예측 
predict2 = gg.predict(X_test)
print(predict2)
print(y_test)
```

```
[0 0 2 1 0 0 2 1 0 1 2 0 1 0 2 1 1 0 1 0 0 1 1 0 2 2 0 1 1 0]
[0 0 2 1 0 0 2 1 0 1 2 0 1 0 2 1 1 0 1 0 0 1 1 0 2 2 0 1 2 0]
```

```python
print(X_test.shape[0])
print((y_test != predict2).sum()) # 실제 테스트용 정답과 예측이 같지 않는 부분의 수의 합계
```

# 단점 : 
  1. Feature간의 독립성 필수 => Feature간에 상관관계가 서로 없어야 함 
  2. 실생활에 바로 적용이 어려움 

# 장점 : 
  1. data양이 클 때 도움 
  2. 간단하고, 빠르고, 정확









