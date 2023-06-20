---
layout: single
title: "DecisionTree"
categories: python
tag: [colab, ai]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# Decision Tree(의사결정나무)

머신러닝 - 지도학습 - 분류 기법

입력값에 대한 예측값을 나무 형태로 나타내주는 모델

데이터의 분포를 나눠줌

이 데이터를 어떤 기준으로 분류했을 때 동일한 객체들이 잘 모아지게 할 수 있을까? 를 고려해서 분류기준을 찾는 것이 중요

예측모델 : 연속적인 질문을 해나가면서 예측 결과를 제공 

- 특징 
  1. 알고리즘 해석이 쉬운편
  2. 수치형, 범주(카테고리형) 데이터도 적용 가능 O
  3. 데이터 전처리 양이 적은 편 



```python
from sklearn import datasets

iris = datasets.load_iris() # 붓꽃 데이터 가져오기 
print(iris.feature_names) # feature (data, 열, columns) 이름
print(iris.target_names) # target (label) 이름 
```

```
['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']
['setosa' 'versicolor' 'virginica']
```



```python
# scikit-learn 이 제공하는 의사결정나무 모델 : DecisionTreeClassifier
from sklearn import tree
from sklearn.tree import DecisionTreeClassifier
```

```python
feature = iris.data     # feature 가져오기
labels = iris.target    # label 가져오기 (iris 품종 데이터)

p_feature = feature[:, 2:] # 붓꽃 꽃잎의 길이와 넓이

iris_tree = DecisionTreeClassifier(criterion='entropy', max_depth=3)
# DecisionTreeClassifier : 의사결정나무 모델 
# criterion='entropy' : 엔트로피를 불순도 계산방법으로 적용 
#   엔트로피 : 혼잡도, 학습 시작 단계에서는 분류할 것들이 많아서 엔트로피가 높은 상태 
#               하나씩 정리해가면서 엔트로피를 줄여 최종적으로 0에 가깝게 만드는 방식 
# criterion='gini' (default) : 분류를 잘못할 확률을 최소화 하기 위한 목적 
#                   > 분할이 가능한 모든 방법을 지니계수로 확인 > 가장 낮은 것을 선택하는 방식 
#     지니계수 : 얼마나 불확실한가의 지표 ,
#                 0에 가까어질수록 불확실성이 0에 가까워지는 것으로 
#                 같은 특성을 가진 객체들끼리 잘 모여있다는 뜻!
# max_depth = 깊이

# 어떤 기준으로 분류를 하는가에따라서 tree 모양도 변하고 깊이도 달라짐

# 독립변수 : 원인 / (시험지)
# 종속변수 : 결과값 / (답안지)
# 종속변수는 독립변수에 의해 영향을 받는다. 
iris_tree.fit(p_feature, labels) # .fit(독립변수, 종속변수)

```

```
DecisionTreeClassifier
DecisionTreeClassifier(criterion='entropy', max_depth=3)
```

```python
# dot 파일 만들기 
with open('iris.dot', mode='w') as f:
  tree.export_graphviz(iris_tree, out_file=f)

# graphviz: 코드의 구조를 다이어그램의 형태 그림으로 표현해주는 도구
# out_file : 파일 또는 문자열로 반환
```

```python
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
from sklearn.tree import export_graphviz
from subprocess import check_call
```

dot - Tpng [dot파일명].dot -o [만들png파일명].png

dot - [dot파일명].dot -Tpng -o [만들png파일명].png



```python
iris = load_iris() # iris dataset 가져오기 
x = iris.data[:, 2:] # 꽃잎의 길이, 넓이 (iris의 feature에 있는(행 전체, 두번째 열부터 ~))
y = iris.target # iris의 label

tree = DecisionTreeClassifier(max_depth=4)
tree.fit(x,y)

export_graphviz(
    tree,                       # 학습시킨 모델(모형)
    out_file='./iris2.dot',     # dot 저장 경로 설정 
    feature_names = iris.feature_names[2: ], # 사용한 열(변수, columns, feature) 이름
    class_names = iris.target_names, # 예측할 target 클래스 이름 
    filled = True, # 사각형 안의 색깔 채우기
    rounded = True # 사각형 모서리를 둥글게 
)

# png로 바꿔서 시각화 
# dot - Tpng [dot파일명].dot -o [만들png파일명].png
check_call(
    ['dot', '-Tpng', 'iris2.dot', '-o', 'iris.png']
)
```

![image-20230620112509903](/images/2023-06-20-jun201decisiontree/image-20230620112509903.png)













