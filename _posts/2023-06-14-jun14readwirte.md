---
layout: single
title: "Read & Write"
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
```

# 1. csv파일 읽고 쓰기

# 1-1. 쓰기(to_csv)

데이터프레임을 만들고 csv 파일에 저장

```python
df = pd.DataFrame({'A':[0,1,2], 'B':[1,2,3], 'C':[4,5,6], 'D':[7,8,9]})
df.to_csv('test.csv', index=False)
df

# index=False 옵션은 데이터프레임에 자동으로 추가된 인덱스가 없이 저장
# header 옵션의 별도 지정이 없으면 첫 행의 데이터가 열의 이름

	A	B	C	D
0	0	1	4	7
1	1	2	5	8
2	2	3	6	9
```

# 1-2. 읽기 (read_csv)

```python
df1 = pd.read_csv('test.csv')
df1

	A	B	C	D
0	0	1	4	7
1	1	2	5	8
2	2	3	6	9
```

```python
df2 = pd.read_csv('test.csv', header=None)
df2

# read_csv() 함수는 첫번째 행을 header(열이름)로 지정해서 불러옴
# 불러올 데이터에 header가 없다면 header=None 옵션을 줘야함! 

	0	1	2	3
0	A	B	C	D
1	0	1	4	7
2	1	2	5	8
3	2	3	6	9
```

```python
df3 = pd.read_csv('test.csv', index_col='C')
df3

# index_col 옵션 디폴트값이 None이고, 이 때 새로운 정수 0, 1, 2, ... 를 생성
# index_col = 'C'를 옵션으로 준 경우, C열을 인덱스로 불러서 사용

	A	B	D
C			
4	0	1	7
5	1	2	8
6	2	3	9
```

```python
df4 = pd.read_csv('test.csv', names=['1열', '2열', '3열', '4열'])
df4

# names=['열이름1', '열이름2',...] 옵션을 통해 열 이름을 따로 설정할 수 있음 

	1열	2열	3열	4열
0	A	B	C	D
1	0	1	4	7
2	1	2	5	8
3	2	3	6	9
```

# 1-3. 다양한 옵션 

read_csv()의 다양한 옵션은 판다스 공식홈페이지에서 상세하게 보여줌 

많이 쓰이는 옵션들 간단하게 정리...(default기준)

- sep(',') : 자료의 구분 기준을 선정
- header('infer') : 첫 행을 열 이름으로 쓸 것인지 
- names : 열 이름을 리스트로 입력해 줄 수 있음 
- index_col(None) : 특정 열 이름을 인덱스로 지정 
- dtype : 개별 열 or 모든 열의 자료형을 지정 
- na_values : 결측값(NaN)으로 인식할 문자 지정
- key_default_na(True) : 결측값을 포함할지 말지 여부결정
- na_filter(True) : 결측값 감지 
- skip_blank_lines(True) : 비어있는 줄은 결측값으로 판단하지 않고 건너뜀
- nrow : 읽을 파일의 행(row) 수



