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

```python
import pandas as pd
import numpy as np
import seaborn as sns
```

# 자료형(type) 변형, 범주형(category) 데이터 처리

# 1. 데이터 표준화

### 1-1. 단위 환산 : round()

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
# round() : 반올림 함수
t['fare'] = t['fare'].round(2) # 괄호안의 수가 표현할 소수점 자릿수가 됨
t.head()
```

| index | survived | pclass | sex    | age   | sibsp | parch | fare   | embarked | class | who   | adult\_male | deck | embark\_town | alive | alone |
| ----- | -------- | ------ | ------ | ----- | ----- | ----- | ------ | -------- | ----- | ----- | ----------- | ---- | ------------ | ----- | ----- |
| 0     | 0        | 3      | male   | 22\.0 | 1     | 0     | 7\.25  | S        | Third | man   | true        | NaN  | Southampton  | no    | false |
| 1     | 1        | 1      | female | 38\.0 | 1     | 0     | 71\.28 | C        | First | woman | false       | C    | Cherbourg    | yes   | false |
| 2     | 1        | 3      | female | 26\.0 | 0     | 0     | 7\.92  | S        | Third | woman | false       | NaN  | Southampton  | yes   | true  |
| 3     | 1        | 1      | female | 35\.0 | 1     | 0     | 53\.1  | S        | First | woman | false       | C    | Southampton  | yes   | false |
| 4     | 0        | 3      | male   | 35\.0 | 0     | 0     | 8\.05  | S        | Third | man   | true        | NaN  | Southampton  | no    | true  |

```python
# round()함수는 데이터프레임명.round(소수점자릿수) / round(데이터프레임명, 소수점자릿수)
# 괄호안에 아무것도 넣지 않으면, 소수점 첫번째 자리에서 반올림
print(round(1.6667))
print(round(1.6667, 3))

2
1.667
```

### 1-2. 자료형 변환 : astype() [+ replace()]

```python
# 자료형 확인
t.dtypes

survived          int64
pclass            int64
sex              object
age             float64
sibsp             int64
parch             int64
fare            float64
embarked         object
class          category
who              object
adult_male         bool
deck           category
embark_town      object
alive            object
alone              bool
dtype: object
```

```python
t['alive'].describe()

count     891
unique      2
top        no
freq      549
Name: alive, dtype: object
```

```python
t['alive'] = t['alive'].astype('category')
```

```python
t['alive'].dtype

CategoricalDtype(categories=['no', 'yes'], ordered=False)
```

```python
# .xls or .xlsx 확장자의 엑셀 파일을 읽어오려면 read_excel() 함수
# !pip install -- upgarde xlrd -- read_excel() 실행 안될 때

a = pd.read_excel('myStudent.xlsx')
a.head()
```

| index | gender | group | education  | kor  | eng  | math |
| ----- | ------ | ----- | ---------- | ---- | ---- | ---- |
| 0     | f      | a     | highscool  | 100  | 80   | 50   |
| 1     | f      | b     | college    | 56   | 15   | 89   |
| 2     | m      | c     | university | 12   | 68   | 52   |
| 3     | f      | d     | highscool  | 85   | 65   | 15   |
| 4     | m      | e     | college    | 80   | 100  | 50   |

```python
# 국어, 영어, 수학 점수 다 더한 값의 평균 값 계산해서 'avg' 열 생성
a['avg'] = (a['kor'] + a['eng'] + a['math']) / 3
a.head()
```

| index | gender | group | education  | kor  | eng  | math | avg                 |
| ----- | ------ | ----- | ---------- | ---- | ---- | ---- | ------------------- |
| 0     | f      | a     | highscool  | 100  | 80   | 50   | 76\.66666666666667  |
| 1     | f      | b     | college    | 56   | 15   | 89   | 53\.333333333333336 |
| 2     | m      | c     | university | 12   | 68   | 52   | 44\.0               |
| 3     | f      | d     | highscool  | 85   | 65   | 15   | 55\.0               |
| 4     | m      | e     | college    | 80   | 100  | 50   | 76\.66666666666667  |

```python
a.dtypes

gender        object
group         object
education     object
kor            int64
eng            int64
math           int64
avg          float64
dtype: object
```

```python
a['gender'] = a['gender'].astype('category')
a['group'] = a['group'].astype('category')
a['education'] = a['education'].astype('category')

a.dtypes

gender       category
group        category
education    category
kor             int64
eng             int64
math            int64
avg           float64
dtype: object
```

```python
a[['gender', 'group', 'education']].describe()
```

| index  | gender | group | education |
| ------ | ------ | ----- | --------- |
| count  | 110    | 110   | 110       |
| unique | 2      | 5     | 3         |
| top    | m      | a     | highscool |
| freq   | 61     | 22    | 41        |

```python
# replace() 함수 : 값 변경
# 다양한 값을 한번에...
# replace( {변경전:변경후, ...})
a['gender']
a['gender'].replace( {'f' : 'female', 'm' : 'male'}, inplace=True)
a['gender'].unique()

['female', 'male']
Categories (2, object): ['female', 'male']
```

# 2. 범주형(카테고리) 데이터 처리

구간 분할 : np.histogram(), pd.cut()

```python
aa = pd.DataFrame(a)
aa.head()
```

| index | gender | group | education  | kor  | eng  | math | avg                 |
| ----- | ------ | ----- | ---------- | ---- | ---- | ---- | ------------------- |
| 0     | f      | a     | highscool  | 100  | 80   | 50   | 76\.66666666666667  |
| 1     | f      | b     | college    | 56   | 15   | 89   | 53\.333333333333336 |
| 2     | m      | c     | university | 12   | 68   | 52   | 44\.0               |
| 3     | f      | d     | highscool  | 85   | 65   | 15   | 55\.0               |
| 4     | m      | e     | college    | 80   | 100  | 50   | 76\.66666666666667  |

```python
# np.histogram() 함수 : np.histogram(나눌 리스트, bins=나눌 구간) 의 형태
# 2개의 값 :
#   count : 각 구간에 속하는 값의 개수
#   bins_dividers : 경계값 리스트를 반환
count, bins_dividers = np.histogram(aa['avg'], bins=3)
print(count, '\n', bins_dividers)

[16 40 54] 
 [26.66666667 49.44444444 72.22222222 95.    
```

```python
# pd.cut() 함수 : 자동으로 범주형변수를 생성해주고, 간단함
bin_names = ['low', 'middle', 'high']
aa['aa_bin'] = pd.cut(x = aa['avg'],         # 데이터 배열
                      bins=bins_dividers,   # 경계값 리스트
                      labels=bin_names,     # 구간명
                      include_lowest=True   # 첫 경계값 포함 여부(구간의 하위값 )
                       )
aa.head()
```

| index | gender | group | education  | kor  | eng  | math | avg                 | aa\_bin |
| ----- | ------ | ----- | ---------- | ---- | ---- | ---- | ------------------- | ------- |
| 0     | f      | a     | highscool  | 100  | 80   | 50   | 76\.66666666666667  | high    |
| 1     | f      | b     | college    | 56   | 15   | 89   | 53\.333333333333336 | middle  |
| 2     | m      | c     | university | 12   | 68   | 52   | 44\.0               | low     |
| 3     | f      | d     | highscool  | 85   | 65   | 15   | 55\.0               | middle  |
| 4     | m      | e     | college    | 80   | 100  | 50   | 76\.66666666666667  | high    |













