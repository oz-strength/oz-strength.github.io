---
layout: single
title: "Duplication"
categories: python
tag: [colab, pandas]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# 결측치(누락 데이터)와 중복 데이터 처리 

```python
import pandas as pd
import seaborn as sns 
```

```python
# seaborn dataset 목록 보기
sns.get_dataset_names()

['anagrams',
 'anscombe',
 'attention',
 'brain_networks',
 'car_crashes',
 'diamonds',
 'dots',
 'dowjones',
 'exercise',
 'flights',
 'fmri',
 'geyser',
 'glue',
 'healthexp',
 'iris',
 'mpg',
 'penguins',
 'planets',
 'seaice',
 'taxis',
 'tips',
 'titanic']
```

```python
t = sns.load_dataset('titanic')
t.head()
```

| index | survived | pclass | sex    | age   | sibsp | parch | fare     | embarked | class | who   | adult\_male | deck | embark\_town | alive | alone |
| ----- | -------- | ------ | ------ | ----- | ----- | ----- | -------- | -------- | ----- | ----- | ----------- | ---- | ------------ | ----- | ----- |
| 0     | 0        | 3      | male   | 22\.0 | 1     | 0     | 7\.25    | S        | Third | man   | true        | NaN  | Southampton  | no    | false |
| 1     | 1        | 1      | female | 38\.0 | 1     | 0     | 71\.2833 | C        | First | woman | false       | C    | Cherbourg    | yes   | false |
| 2     | 1        | 3      | female | 26\.0 | 0     | 0     | 7\.925   | S        | Third | woman | false       | NaN  | Southampton  | yes   | true  |
| 3     | 1        | 1      | female | 35\.0 | 1     | 0     | 53\.1    | S        | First | woman | false       | C    | Southampton  | yes   | false |
| 4     | 0        | 3      | male   | 35\.0 | 0     | 0     | 8\.05    | S        | Third | man   | true        | NaN  | Southampton  | no    | true  |

```python
# 데이터 분석
t.info()
# 행 : 891, 열 : 15
# age, embarked, deck, embark_town 에 결측치 존재(= 비어있는 값이 있다)

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 891 entries, 0 to 890
Data columns (total 15 columns):
 #   Column       Non-Null Count  Dtype   
---  ------       --------------  -----   
 0   survived     891 non-null    int64   
 1   pclass       891 non-null    int64   
 2   sex          891 non-null    object  
 3   age          714 non-null    float64 
 4   sibsp        891 non-null    int64   
 5   parch        891 non-null    int64   
 6   fare         891 non-null    float64 
 7   embarked     889 non-null    object  
 8   class        891 non-null    category
 9   who          891 non-null    object  
 10  adult_male   891 non-null    bool    
 11  deck         203 non-null    category
 12  embark_town  889 non-null    object  
 13  alive        891 non-null    object  
 14  alone        891 non-null    bool    
dtypes: bool(2), category(2), float64(2), int64(4), object(5)
memory usage: 80.7+ KB
```

```python
# 'deck' 열의 고유값 개수 확인
# dropna=True(가 기본값) => dropna=False로 바꾸면
# 누락데이터(NaN, 결측치)의 개수도 함께 카운트 됨
t['deck'].value_counts(dropna=False)

NaN    688
C       59
B       47
D       33
E       32
A       15
F       13
G        4
Name: deck, dtype: int64
```





























