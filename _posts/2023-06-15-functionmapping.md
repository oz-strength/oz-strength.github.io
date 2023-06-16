---
layout: single
title: "Function Mapping"
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
import seaborn as sns 
```

```python
ms = pd.read_excel('myStudent.xlsx')
ms
```

| index | gender | group | education  | kor  | eng  | math | sum  | avg                 |
| ----- | ------ | ----- | ---------- | ---- | ---- | ---- | ---- | ------------------- |
| 0     | f      | a     | highscool  | 100  | 80   | 50   | 230  | 76\.66666666666667  |
| 1     | f      | b     | college    | 56   | 15   | 89   | 160  | 53\.333333333333336 |
| 2     | m      | c     | university | 12   | 68   | 52   | 132  | 44\.0               |
| 3     | f      | d     | highscool  | 85   | 65   | 15   | 165  | 55\.0               |
| 4     | m      | e     | college    | 80   | 100  | 50   | 230  | 76\.66666666666667  |
| 5     | m      | a     | university | 15   | 56   | 89   | 160  | 53\.333333333333336 |
| 6     | m      | b     | highscool  | 68   | 12   | 52   | 132  | 44\.0               |
| 7     | m      | c     | college    | 65   | 85   | 15   | 165  | 55\.0               |
| 8     | f      | d     | university | 100  | 50   | 100  | 250  | 83\.33333333333333  |
| 9     | m      | e     | highscool  | 50   | 89   | 56   | 195  | 65\.0               |
| 10    | f      | a     | college    | 89   | 52   | 12   | 153  | 51\.0               |
| 11    | m      | b     | university | 52   | 15   | 85   | 152  | 50\.666666666666664 |
| 12    | m      | c     | highscool  | 15   | 85   | 15   | 115  | 38\.333333333333336 |
| 13    | f      | d     | highscool  | 15   | 80   | 50   | 145  | 48\.333333333333336 |
| 14    | f      | e     | highscool  | 50   | 15   | 89   | 154  | 51\.333333333333336 |
| 15    | m      | a     | highscool  | 89   | 68   | 52   | 209  | 69\.66666666666667  |
| 16    | f      | b     | highscool  | 52   | 80   | 80   | 212  | 70\.66666666666667  |
| 17    | m      | c     | highscool  | 52   | 15   | 15   | 82   | 27\.333333333333332 |
| 18    | f      | d     | highscool  | 15   | 68   | 68   | 151  | 50\.333333333333336 |

```python
# kor, eng, math를 더한 값을 'sum'열에 출력
ms['sum']=ms['kor']+ms['eng']+ms['math']
ms
```

| index | gender | group | education  | kor  | eng  | math | sum  | avg                 |
| ----- | ------ | ----- | ---------- | ---- | ---- | ---- | ---- | ------------------- |
| 0     | f      | a     | highscool  | 100  | 80   | 50   | 230  | 76\.66666666666667  |
| 1     | f      | b     | college    | 56   | 15   | 89   | 160  | 53\.333333333333336 |
| 2     | m      | c     | university | 12   | 68   | 52   | 132  | 44\.0               |
| 3     | f      | d     | highscool  | 85   | 65   | 15   | 165  | 55\.0               |
| 4     | m      | e     | college    | 80   | 100  | 50   | 230  | 76\.66666666666667  |
| 5     | m      | a     | university | 15   | 56   | 89   | 160  | 53\.333333333333336 |
| 6     | m      | b     | highscool  | 68   | 12   | 52   | 132  | 44\.0               |
| 7     | m      | c     | college    | 65   | 85   | 15   | 165  | 55\.0               |
| 8     | f      | d     | university | 100  | 50   | 100  | 250  | 83\.33333333333333  |
| 9     | m      | e     | highscool  | 50   | 89   | 56   | 195  | 65\.0               |
| 10    | f      | a     | college    | 89   | 52   | 12   | 153  | 51\.0               |
| 11    | m      | b     | university | 52   | 15   | 85   | 152  | 50\.666666666666664 |
| 12    | m      | c     | highscool  | 15   | 85   | 15   | 115  | 38\.333333333333336 |
| 13    | f      | d     | highscool  | 15   | 80   | 50   | 145  | 48\.333333333333336 |
| 14    | f      | e     | highscool  | 50   | 15   | 89   | 154  | 51\.333333333333336 |
| 15    | m      | a     | highscool  | 89   | 68   | 52   | 209  | 69\.66666666666667  |
| 16    | f      | b     | highscool  | 52   | 80   | 80   | 212  | 70\.66666666666667  |
| 17    | m      | c     | highscool  | 52   | 15   | 15   | 82   | 27\.333333333333332 |
| 18    | f      | d     | highscool  | 15   | 68   | 68   | 151  | 50\.333333333333336 |

```python
# 어떤 값을 넣으면 그 값을 3으로 나눈 결과를 내보내주는 함수 
def divide_three(a):
  return a/3
divide_three(6)

2.0
```

```python
ms['avg'] = ms['sum'].apply(divide_three)
ms
```

| index | gender | group | education  | kor  | eng  | math | sum  | avg                 |
| ----- | ------ | ----- | ---------- | ---- | ---- | ---- | ---- | ------------------- |
| 0     | f      | a     | highscool  | 100  | 80   | 50   | 230  | 76\.66666666666667  |
| 1     | f      | b     | college    | 56   | 15   | 89   | 160  | 53\.333333333333336 |
| 2     | m      | c     | university | 12   | 68   | 52   | 132  | 44\.0               |
| 3     | f      | d     | highscool  | 85   | 65   | 15   | 165  | 55\.0               |
| 4     | m      | e     | college    | 80   | 100  | 50   | 230  | 76\.66666666666667  |
| 5     | m      | a     | university | 15   | 56   | 89   | 160  | 53\.333333333333336 |
| 6     | m      | b     | highscool  | 68   | 12   | 52   | 132  | 44\.0               |
| 7     | m      | c     | college    | 65   | 85   | 15   | 165  | 55\.0               |
| 8     | f      | d     | university | 100  | 50   | 100  | 250  | 83\.33333333333333  |
| 9     | m      | e     | highscool  | 50   | 89   | 56   | 195  | 65\.0               |
| 10    | f      | a     | college    | 89   | 52   | 12   | 153  | 51\.0               |
| 11    | m      | b     | university | 52   | 15   | 85   | 152  | 50\.666666666666664 |
| 12    | m      | c     | highscool  | 15   | 85   | 15   | 115  | 38\.333333333333336 |
| 13    | f      | d     | highscool  | 15   | 80   | 50   | 145  | 48\.333333333333336 |
| 14    | f      | e     | highscool  | 50   | 15   | 89   | 154  | 51\.333333333333336 |
| 15    | m      | a     | highscool  | 89   | 68   | 52   | 209  | 69\.66666666666667  |
| 16    | f      | b     | highscool  | 52   | 80   | 80   | 212  | 70\.66666666666667  |
| 17    | m      | c     | highscool  | 52   | 15   | 15   | 82   | 27\.333333333333332 |
| 18    | f      | d     | highscool  | 15   | 68   | 68   | 151  | 50\.333333333333336 |

# 함수 매핑(mapping)

시리즈나 데이터프레임의 개별 요소를 특정 함수에 1:1로 대응시키는 과정 

사용자가 직접 만든 함수(람다 포함)를 적용시킬 수 있기 때문에 for문이나 기본 함수로 처리하는 것보다 훨씬 효율적인 장점 

```python
# seaborn에서 타이타닉 데이터 불러와서 => 'age', 'fare'열만 인덱싱
#    => 'ten' 열을 만들어서 => 10 넣기 
titanic = sns.load_dataset('titanic')
df = titanic.loc[: , ['age', 'fare']]
df['ten'] = 10
df.head()
```

| index | age   | fare     | ten  |
| ----- | ----- | -------- | ---- |
| 0     | 22\.0 | 7\.25    | 10   |
| 1     | 38\.0 | 71\.2833 | 10   |
| 2     | 26\.0 | 7\.925   | 10   |
| 3     | 35\.0 | 53\.1    | 10   |
| 4     | 35\.0 | 8\.05    | 10   |

```python
# 값을 넣으면 10을 더해주는 함수
def add_10(x):
  return x+10
# 값을 2개 넣으면 그 둘을 곱해주는 함수
def mul_two_obj(x,y):
  return x * y
# 값을 3개 넣으면 그 셋을 더해주는 함수 
def add_three_obj(x,y,z):
  return x+y+z
print(add_10(15))
print(mul_two_obj(10, 20))
print(add_three_obj(10,20,30))

25
200
60
```

```python
# 이 함수들을 apply()함수들을 이용해서 시리즈객체의 개별요소에 적용
sr1 = df['age'].apply(add_10) # age의 모든 요소 + 10
sr1.head()

0    32.0
1    48.0
2    36.0
3    45.0
4    45.0
Name: age, dtype: float64
```

```python
sr2 = df['fare'].apply(mul_two_obj, y=0.9)
sr2.head()

0     6.52500
1    64.15497
2     7.13250
3    47.79000
4     7.24500
Name: fare, dtype: float64
```

```python
# 'ten' 열의 값들을 lamda 함수를 이용해서 add_10함수 적용 
(lambda a, b : print(a * b))(10, 20)
sr3 = df['ten'].apply(lambda x : add_10(x))
sr3.head()

200
0    20
1    20
2    20
3    20
4    20
Name: ten, dtype: int64
```

# apply() vs applymap()

```python
df_map = df.applymap(add_10) # 데이터프레임의 모든 요소에 함수를 적용함
df_map.head()
```

| index | age   | fare     | ten  |
| ----- | ----- | -------- | ---- |
| 0     | 32\.0 | 17\.25   | 20   |
| 1     | 48\.0 | 81\.2833 | 20   |
| 2     | 36\.0 | 17\.925  | 20   |
| 3     | 45\.0 | 63\.1    | 20   |
| 4     | 45\.0 | 18\.05   | 20   |

# 시리즈 객체에 함수 매핑

Dataframe객체.apply(매핑함수, axis=값)

열 또는 행에 대해서 함수를 적용시킬 수 있음 

```python
# 뭔가를 넣었을 때 결측값을 확인하는 함수 
def missing_value(series):
  return series.isnull()
```

```python
# 그 함수를 데이터프레임의 열에 매핑(axis =1)
result = df.apply(missing_value, axis=1)
print(result)
print()
print(type(result))
result.describe()

       age   fare    ten
0    False  False  False
1    False  False  False
2    False  False  False
3    False  False  False
4    False  False  False
..     ...    ...    ...
886  False  False  False
887  False  False  False
888   True  False  False
889  False  False  False
890  False  False  False

[891 rows x 3 columns]

<class 'pandas.core.frame.DataFrame'>
```

| index  | age   | fare  | ten   |
| ------ | ----- | ----- | ----- |
| count  | 891   | 891   | 891   |
| unique | 2     | 1     | 1     |
| top    | false | false | false |
| freq   | 714   | 891   | 891   |

# 3. 데이터프레임 객체에 함수를 매핑

Dataframe객체.pipe() 함수 

pipe()함수는 데이터프레임객체에 바로 적용시키는 함수

파라미터에 들어가는 매핑함수의 반환형태를 그대로 따라서 값을 반환 

```python
# 타이타닉 데이터셋 불러와서 => 'age', 'fare' 열만 나오게
titanic = sns.load_dataset('titanic')
df = titanic.loc[: , ['age', 'fare']]
df
```

| index | age   | fare     |
| ----- | ----- | -------- |
| 0     | 22\.0 | 7\.25    |
| 1     | 38\.0 | 71\.2833 |
| 2     | 26\.0 | 7\.925   |
| 3     | 35\.0 | 53\.1    |
| 4     | 35\.0 | 8\.05    |
| 5     | NaN   | 8\.4583  |
| 6     | 54\.0 | 51\.8625 |
| 7     | 2\.0  | 21\.075  |
| 8     | 27\.0 | 11\.1333 |
| 9     | 14\.0 | 30\.0708 |
| 10    | 4\.0  | 16\.7    |
| 11    | 58\.0 | 26\.55   |
| 12    | 20\.0 | 8\.05    |
| 13    | 39\.0 | 31\.275  |
| 14    | 14\.0 | 7\.8542  |
| 15    | 55\.0 | 16\.0    |
| 16    | 2\.0  | 29\.125  |
| 17    | NaN   | 13\.0    |
| 18    | 31\.0 | 18\.0    |

```python
# 값 비어있는가에 대한 함수
def missing_value2(x):
  return x.isnull()

# 각 열의 비어있는 요소의 총 갯수를 구해주는 함수 
def missing_count(x):
  return missing_value2(x).sum()

# 각 열의 비어있는 요소의 총 개수를 더한값을 구해주는 함수 
def total_number_missing(x):
  return missing_count(x).sum()
```

```python
# 순차적으로 적용
result_df = df.pipe(missing_value2)
print(result_df)

       age   fare
0    False  False
1    False  False
2    False  False
3    False  False
4    False  False
..     ...    ...
886  False  False
887  False  False
888   True  False
889  False  False
890  False  False

[891 rows x 2 columns]
```

```python
result_mc = df.pipe(missing_count)
print(result_mc)

age     177
fare      0
dtype: int64
```

```python
result_total = df.pipe(total_number_missing)
print(result_total)

177	
```









