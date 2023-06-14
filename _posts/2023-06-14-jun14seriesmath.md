---
layout: single
title: "Series & Math"
categories: python
tag: [colab, pandas]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# 1. 시리즈의 산술연산

```python
import pandas as pd
import seaborn as sns
import numpy as np
```

# Seaborn 내장 데이터셋

Seaborn은 Matplotlib을 기반으로 다양한 테마, 통계용 차트 등 기능을 추가한 시각화 패키지 

```python
# Seaborn 데이터셋 목록
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
# 데이터셋 불러오기
sns.load_dataset('car_crashes') # 자동차 사고 데이터

	total	speeding	alcohol	not_distracted	no_previous	ins_premium	ins_losses	abbrev
0	18.8	7.332	5.640	18.048	15.040	784.55	145.08	AL
1	18.1	7.421	4.525	16.290	17.014	1053.48	133.93	AK
2	18.6	6.510	5.208	15.624	17.856	899.47	110.35	AZ
3	22.4	4.032	5.824	21.056	21.280	827.34	142.39	AR
4	12.0	4.200	3.360	10.920	10.680	878.41	165.63	CA
5	13.6	5.032	3.808	10.744	12.920	835.50	139.91	CO
6	10.8	4.968	3.888	9.396	8.856	1068.73	167.02	CT
7	16.2	6.156	4.860	14.094	16.038	1137.87	151.48	DE
8	5.9	2.006	1.593	5.900	5.900	1273.89	136.05	DC
9	17.9	3.759	5.191	16.468	16.826	1160.13	144.18	FL
10	15.6	2.964	3.900	14.820	14.508	913.15	142.80	GA
11	17.5	9.450	7.175	14.350	15.225	861.18	120.92	HI
12	15.3	5.508	4.437	13.005	14.994	641.96	82.75	ID
13	12.8	4.608	4.352	12.032	12.288	803.11	139.15	IL
14	14.5	3.625	4.205	13.775	13.775	710.46	108.92	IN
15	15.7	2.669	3.925	15.229	13.659	649.06	114.47	IA
16	17.8	4.806	4.272	13.706	15.130	780.45	133.80	KS
17	21.4	4.066	4.922	16.692	16.264	872.51	137.13	KY
18	20.5	7.175	6.765	14.965	20.090	1281.55	194.78	LA
19	15.1	5.738	4.530	13.137	12.684	661.88	96.57	ME
20	12.5	4.250	4.000	8.875	12.375	1048.78	192.70	MD
21	8.2	1.886	2.870	7.134	6.560	1011.14	135.63	MA
22	14.1	3.384	3.948	13.395	10.857	1110.61	152.26	MI
23	9.6	2.208	2.784	8.448	8.448	777.18	133.35	MN
24	17.6	2.640	5.456	1.760	17.600	896.07	155.77	MS
25	16.1	6.923	5.474	14.812	13.524	790.32	144.45	MO
26	21.4	8.346	9.416	17.976	18.190	816.21	85.15	MT
27	14.9	1.937	5.215	13.857	13.410	732.28	114.82	NE
28	14.7	5.439	4.704	13.965	14.553	1029.87	138.71	NV
29	11.6	4.060	3.480	10.092	9.628	746.54	120.21	NH
30	11.2	1.792	3.136	9.632	8.736	1301.52	159.85	NJ
31	18.4	3.496	4.968	12.328	18.032	869.85	120.75	NM
32	12.3	3.936	3.567	10.824	9.840	1234.31	150.01	NY
33	16.8	6.552	5.208	15.792	13.608	708.24	127.82	NC
34	23.9	5.497	10.038	23.661	20.554	688.75	109.72	ND
35	14.1	3.948	4.794	13.959	11.562	697.73	133.52	OH
36	19.9	6.368	5.771	18.308	18.706	881.51	178.86	OK
37	12.8	4.224	3.328	8.576	11.520	804.71	104.61	OR
38	18.2	9.100	5.642	17.472	16.016	905.99	153.86	PA
39	11.1	3.774	4.218	10.212	8.769	1148.99	148.58	RI
40	23.9	9.082	9.799	22.944	19.359	858.97	116.29	SC
41	19.4	6.014	6.402	19.012	16.684	669.31	96.87	SD
42	19.5	4.095	5.655	15.990	15.795	767.91	155.57	TN
43	19.4	7.760	7.372	17.654	16.878	1004.75	156.83	TX
44	11.3	4.859	1.808	9.944	10.848	809.38	109.48	UT
45	13.6	4.080	4.080	13.056	12.920	716.20	109.61	VT
46	12.7	2.413	3.429	11.049	11.176	768.95	153.72	VA
47	10.6	4.452	3.498	8.692	9.116	890.03	111.62	WA
48	23.8	8.092	6.664	23.086	20.706	992.61	152.56	WV
49	13.8	4.968	4.554	5.382	11.592	670.31	106.62	WI
50	17.4	7.308	5.568	14.094	15.660	791.14	122.04	WY
```

# 1-1. 시리즈 vs 숫자

```python
student1 = pd.Series({'국어':100,'영어':80,'수학':90})
student1

print(student1 / 200)

국어    0.50
영어    0.40
수학    0.45
dtype: float64
```

# 1-2. 시리즈 vs 시리즈

시리즈와 시리즈간의 연산은 같은 인덱스를 가진 요소끼리 연산해서 새 시리즈로 반환

```python
student1 = pd.Series({'국어':100,'영어':80,'수학':90})
student2 = pd.Series({'수학':80, '국어':90, '영어':80})

print(student1)
print()
print(student2)

국어    100
영어     80
수학     90
dtype: int64

수학    80
국어    90
영어    80
dtype: int64
```

```python
# 사칙연산 ...!
add_data = student1 + student2
sub_data = student1 - student2
mul_data = student1 * student2
div_data = student1 / student2

print(add_data)
print()
print(sub_data)
print()
print(mul_data)
print()
print(div_data)
print()

국어    190
수학    170
영어    160
dtype: int64

국어    10
수학    10
영어     0
dtype: int64

국어    9000
수학    7200
영어    6400
dtype: int64

국어    1.111111
수학    1.125000
영어    1.000000
dtype: float64
```

```python
# 그 결과를 데이터프레임으로 합치기 (시리즈 => 데이터프레임)

result = pd.DataFrame([add_data, sub_data, mul_data, div_data],
                      index = ['덧셈', '뺄셈', '곱셈', '나눗셈'])
result

	국어	수학	영어
덧셈	190.000000	170.000	160.0
뺄셈	10.000000	10.000	0.0
곱셈	9000.000000	7200.000	6400.0
나눗셈	1.111111	1.125	1.0
```



# 1-3. NaN값이 있는 시리즈 연산 

```python
student1 = pd.Series({'국어':np.nan,'영어':80,'수학':90})
student2 = pd.Series({'수학':80, '국어':90})

print(student1)
print()
print(student2)
print()

# 이 둘을 결과는 어떻게...?
print(student1 + student2)
# NaN과의 연산결과는 NaN이고, 인덱스가 없는 경우에도 NaN으로 반환

국어     NaN
영어    80.0
수학    90.0
dtype: float64

수학    80
국어    90
dtype: int64

국어      NaN
수학    170.0
영어      NaN
dtype: float64
```

# 1-4. 연산함수(add(), sub(), mul(), div()) 와 fill_value()

위에서 사칙연산을 + - * / 로 직접 해줬는데... 연산함수도 존재 !

위에서 연산결과가 NaN이 되는 두가지 경우에 대해서 fill_value() 옵션으로 값을 대체

```python
add_data = student1.add(student2, fill_value=0)
sub_data = student1.sub(student2, fill_value=0)
mul_data = student1.mul(student2, fill_value=0)
div_data = student1.div(student2, fill_value=0)

result = pd.DataFrame([add_data, sub_data, mul_data, div_data],
                      index = ['덧셈', '뺄셈', '곱셈', '나눗셈'])
result

# inf는 무한대(infinite)의 의미
# '영어' 인덱스를 연산할 때 NaN을 0으로 대체해서 계산 
#   => 80 / 0 => inf

	국어	수학	영어
덧셈	90.0	170.000	80.0
뺄셈	-90.0	10.000	80.0
곱셈	0.0	7200.000	0.0
나눗셈	0.0	1.125	inf
```

# 2. 데이터프레임의 산술연산

# 2-1. 데이터프레임 vs 숫자

데이터프레임은 결국 시리즈의 확장이기 때문에 시리즈의 연산과 별다른 차이가 없다

```python
titanic = sns.load_dataset('titanic')
titanic.head()

	survived	pclass	sex	age	sibsp	parch	fare	embarked	class	who	adult_male	deck	embark_town	alive	alone
0	0	3	male	22.0	1	0	7.2500	S	Third	man	True	NaN	Southampton	no	False
1	1	1	female	38.0	1	0	71.2833	C	First	woman	False	C	Cherbourg	yes	False
2	1	3	female	26.0	0	0	7.9250	S	Third	woman	False	NaN	Southampton	yes	True
3	1	1	female	35.0	1	0	53.1000	S	First	woman	False	C	Southampton	yes	False
4	0	3	male	35.0	0	0	8.0500	S	Third	man	True	NaN	Southampton	no	True
```

```python
# age, fare 열의 데이터 출력
df = titanic[['age', 'fare']]
df

	age	fare
0	22.0	7.2500
1	38.0	71.2833
2	26.0	7.9250
3	35.0	53.1000
4	35.0	8.0500
...	...	...
886	27.0	13.0000
887	19.0	30.0000
888	NaN	23.4500
889	26.0	30.0000
890	32.0	7.7500
891 rows × 2 columns
```

```python
addition = df + 10
addition.head()

	age	fare
0	32.0	17.2500
1	48.0	81.2833
2	36.0	17.9250
3	45.0	63.1000
4	45.0	18.0500
```



# 2-2. 데이터프레임 vs 데이터프레임

```python
df.tail()

age	fare
886	27.0	13.00
887	19.0	30.00
888	NaN	23.45
889	26.0	30.00
890	32.0	7.75
```

```python
addition = df + 10
addition.tail()

	age	fare
886	37.0	23.00
887	29.0	40.00
888	NaN	33.45
889	36.0	40.00
890	42.0	17.75
```

```python
substraction = addition - df
substraction.tail()

	age	fare
886	10.0	10.0
887	10.0	10.0
888	NaN	10.0
889	10.0	10.0
890	10.0	10.0
```



