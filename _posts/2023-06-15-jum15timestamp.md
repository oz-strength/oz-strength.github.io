---
layout: single
title: "Timestamp"
categories: python
tag: [colab, pandas]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# 시계열 데이터(Timeseries)

시계열 데이터 : 일정한 시간동안 수집된 일련의 순차적으로 정해진 데이터들의 집합 

시계열 자료형 : timestamp, period

```python
import pandas as pd
import seaborn as sns 
```

# timestamp

to_datetime() : 날짜 => 시계열 타입 

```python
flight = sns.load_dataset('flights')
flight.head()
```

| index | year | month | passengers |
| ----- | ---- | ----- | ---------- |
| 0     | 1949 | Jan   | 112        |
| 1     | 1949 | Feb   | 118        |
| 2     | 1949 | Mar   | 132        |
| 3     | 1949 | Apr   | 129        |
| 4     | 1949 | May   | 121        |

```python
flight.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 144 entries, 0 to 143
Data columns (total 3 columns):
 #   Column      Non-Null Count  Dtype   
---  ------      --------------  -----   
 0   year        144 non-null    int64   
 1   month       144 non-null    category
 2   passengers  144 non-null    int64   
dtypes: category(1), int64(2)
memory usage: 2.9 KB
```

```python
# 영어로 되어있는 달 => 숫자로 바꾸기 
months = {'Jan' : 1,
          'Feb' : 2,
          'Mar' : 3,
          'Apr' : 4,
          'May' : 5,
          'Jun' : 6,
          'Jul' : 7,
          'Aug' : 8,
          'Sep' : 9,
          'Oct' : 10,
          'Nov' : 11,
          'Dec' : 12
          }
# map함수를 이용하면, Series를 대상으로 원하는 내용으로 적용을 할 수 있음 
flight['month'] = flight['month'].map(months)
flight
```

| index | year | month | day  | passengers | date                |
| ----- | ---- | ----- | ---- | ---------- | ------------------- |
| 0     | 1949 | 1     | 1    | 112        | 1949-01-01 00:00:00 |
| 1     | 1949 | 2     | 1    | 118        | 1949-02-01 00:00:00 |
| 2     | 1949 | 3     | 1    | 132        | 1949-03-01 00:00:00 |
| 3     | 1949 | 4     | 1    | 129        | 1949-04-01 00:00:00 |
| 4     | 1949 | 5     | 1    | 121        | 1949-05-01 00:00:00 |
| 5     | 1949 | 6     | 1    | 135        | 1949-06-01 00:00:00 |
| 6     | 1949 | 7     | 1    | 148        | 1949-07-01 00:00:00 |
| 7     | 1949 | 8     | 1    | 148        | 1949-08-01 00:00:00 |
| 8     | 1949 | 9     | 1    | 136        | 1949-09-01 00:00:00 |
| 9     | 1949 | 10    | 1    | 119        | 1949-10-01 00:00:00 |
| 10    | 1949 | 11    | 1    | 104        | 1949-11-01 00:00:00 |
| 11    | 1949 | 12    | 1    | 118        | 1949-12-01 00:00:00 |
| 12    | 1950 | 1     | 1    | 115        | 1950-01-01 00:00:00 |
| 13    | 1950 | 2     | 1    | 126        | 1950-02-01 00:00:00 |
| 14    | 1950 | 3     | 1    | 141        | 1950-03-01 00:00:00 |
| 15    | 1950 | 4     | 1    | 135        | 1950-04-01 00:00:00 |
| 16    | 1950 | 5     | 1    | 125        | 1950-05-01 00:00:00 |
| 17    | 1950 | 6     | 1    | 149        | 1950-06-01 00:00:00 |
| 18    | 1950 | 7     | 1    | 170        | 1950-07-01 00:00:00 |

```python
# 열을 하나 추가
# 원하는 위치에 열을 추가하고 싶을 때 : insert(위치, 컬럼명, 값)
flight.insert(2, 'day', 1)
flight.head()
```

| index | year | month | day  | passengers |
| ----- | ---- | ----- | ---- | ---------- |
| 0     | 1949 | 1     | 1    | 112        |
| 1     | 1949 | 2     | 1    | 118        |
| 2     | 1949 | 3     | 1    | 132        |
| 3     | 1949 | 4     | 1    | 129        |
| 4     | 1949 | 5     | 1    | 121        |

```python
# year, month, day > 날짜
flight['date'] = pd.to_datetime(flight[['year', 'month', 'day']])
flight
```

| index | year | month | day  | passengers | date                |
| ----- | ---- | ----- | ---- | ---------- | ------------------- |
| 0     | 1949 | 1     | 1    | 112        | 1949-01-01 00:00:00 |
| 1     | 1949 | 2     | 1    | 118        | 1949-02-01 00:00:00 |
| 2     | 1949 | 3     | 1    | 132        | 1949-03-01 00:00:00 |
| 3     | 1949 | 4     | 1    | 129        | 1949-04-01 00:00:00 |
| 4     | 1949 | 5     | 1    | 121        | 1949-05-01 00:00:00 |
| 5     | 1949 | 6     | 1    | 135        | 1949-06-01 00:00:00 |
| 6     | 1949 | 7     | 1    | 148        | 1949-07-01 00:00:00 |
| 7     | 1949 | 8     | 1    | 148        | 1949-08-01 00:00:00 |
| 8     | 1949 | 9     | 1    | 136        | 1949-09-01 00:00:00 |
| 9     | 1949 | 10    | 1    | 119        | 1949-10-01 00:00:00 |
| 10    | 1949 | 11    | 1    | 104        | 1949-11-01 00:00:00 |
| 11    | 1949 | 12    | 1    | 118        | 1949-12-01 00:00:00 |
| 12    | 1950 | 1     | 1    | 115        | 1950-01-01 00:00:00 |
| 13    | 1950 | 2     | 1    | 126        | 1950-02-01 00:00:00 |
| 14    | 1950 | 3     | 1    | 141        | 1950-03-01 00:00:00 |
| 15    | 1950 | 4     | 1    | 135        | 1950-04-01 00:00:00 |
| 16    | 1950 | 5     | 1    | 125        | 1950-05-01 00:00:00 |
| 17    | 1950 | 6     | 1    | 149        | 1950-06-01 00:00:00 |
| 18    | 1950 | 7     | 1    | 170        | 1950-07-01 00:00:00 |

# Timestamp

python datetime과 유사 

특정 날짜를 표현 가능 

```python
# 위치로(year, month, day, hour, minute, second)
civil_defense = pd.Timestamp(2023, 6, 29, 9, 1, 15)
civil_defense

Timestamp('2023-06-29 09:01:15')
```

```python
# 파라미터명 명시해서 
civil_defense2 = pd.Timestamp(day=29, hour=9, month=6,
                              second=15, year=2023, minute=1)
civil_defense2

Timestamp('2023-06-29 09:01:15')
```

```python
civil_defense = pd.Timestamp(2023, 6, 29, 9, 1, 15)
print("민방위 날짜 : ", civil_defense)
print('연도 : ', civil_defense.year)
print('월 : ', civil_defense.month)
print('일자 : ', civil_defense.day)
print('시간 : ', civil_defense.hour)
print('분 : ', civil_defense.minute)
print('초 : ', civil_defense.second)

print('일년 내 며칠 째에 해당하는지 : ', civil_defense.dayofyear)
print('해당하는 달이 며칠까지 있는지 : ', civil_defense.daysinmonth)
print('몇 분기에 해당하는 지 : ', civil_defense.quarter)
print('몇 주째에 해당하는지 : ', civil_defense.week)

민방위 날짜 :  2023-06-29 09:01:15
연도 :  2023
월 :  6
일자 :  29
시간 :  9
분 :  1
초 :  15
일년 내 며칠 째에 해당하는지 :  180
해당하는 달이 며칠까지 있는지 :  30
몇 분기에 해당하는 지 :  2
몇 주째에 해당하는지 :  26
```

```python
print(civil_defense.date()) # 날짜 
print(civil_defense.time()) # 시간
print(pd.Timestamp.combine(civil_defense.date(), civil_defense.time()))
# combine()함수 : date() 함수와 time() 함수를 합쳐서 날짜/시간 객체 생성 

2023-06-29
09:01:15
2023-06-29 09:01:15
```

```python
# 현재 날짜/시간
print(pd.Timestamp.today())
print(pd.Timestamp.now())

2023-06-15 02:26:39.161633
2023-06-15 02:26:39.162498
```

# Timestamp => String (문자열) 

```python
civil_defense = pd.Timestamp(2023, 6, 29, 9, 1, 15)
civil_defense
# strftime(format) : timestamp => string 
print(civil_defense.strftime('%Y-%m-%d %H:%M:%S %j %U'))

2023-06-29 09:01:15 180 26
```

# 시간 형식 지정자 (format 지정자)

- %Y : 연도 네자리 : 2023
- %y : 연도 끝에 두자리 : 23
- %m : 월(숫자) : 06
- %B : 월(영문전체) : June
- %b : 월(영문줄여서) : Jun
- %d : 일 : 15
- %w : 요일(숫자) : 4 (0이 일요일 ~)
- %A : 요일(영문전체) : Thursday
- %a : 요일(영문줄여서) : Thu
- %H : 시간(24) : 11 
- %I : 시간(12) : 11
- %p : AM, PM : AM
- %M : 분 : 34
- %S : 초 : 19
- %j : (일년기준) 며칠째인지 : 166
- %U : (일년기준) 몇주째인지 : 24

```python
# 날짜를 index로 
dates = ['2023-06-15', '2023-06-16', '2023-06-17']
dateIndex = pd.to_datetime(dates)
dateIndex

DatetimeIndex(['2023-06-15', '2023-06-16', '2023-06-17'], dtype='datetime64[ns]', freq=None)

```

```python
datas = ['목요일', '금요일', '주말 ..언제쯤']
s = pd.Series(datas, index=dateIndex)
s

2023-06-15         목요일
2023-06-16         금요일
2023-06-17    주말 ..언제쯤
dtype: object
```

```python
d = pd.DataFrame(datas, columns=['06'], index=dateIndex)
d
```

| index               | 06              |
| ------------------- | --------------- |
| 2023-06-15 00:00:00 | 목요일          |
| 2023-06-16 00:00:00 | 금요일          |
| 2023-06-17 00:00:00 | 주말 \.\.언제쯤 |

# Period 

pd.Timestamp()와 pd.Period()의 차이

Timestamp는 하나의 시점을 뜻하고, Period는 지정한 날짜의 시작지점부터 종료시점까지의 범위를 포함함 

```python
p = pd.Period('2023-06-15')
p
print(p.start_time)
print(p.end_time)

2023-06-15 00:00:00
2023-06-15 23:59:59.999999999
```

```python
dates = ['2022-08-08', '2022-10-08', '2022-12-08']
datesD = pd.to_datetime(dates)
datesD

DatetimeIndex(['2022-08-08', '2022-10-08', '2022-12-08'], dtype='datetime64[ns]', freq=None)

```

```python
# timestamp -> period
forDay = datesD.to_period(freq='D')
forDay

PeriodIndex(['2022-08-08', '2022-10-08', '2022-12-08'], dtype='period[D]')

```

```python
forMonth = datesD.to_period(freq='M')
forMonth

PeriodIndex(['2022-08', '2022-10', '2022-12'], dtype='period[M]')

```

```python
forYear = datesD.to_period(freq='Y')
forYear

PeriodIndex(['2022', '2022', '2022'], dtype='period[A-DEC]')

```

# Timestamp 배열

date_range() 함수 

```python
pd.date_range('2022-12-08', periods=12, freq='2D')

DatetimeIndex(['2022-12-08', '2022-12-10', '2022-12-12', '2022-12-14',
               '2022-12-16', '2022-12-18', '2022-12-20', '2022-12-22',
               '2022-12-24', '2022-12-26', '2022-12-28', '2022-12-30'],
              dtype='datetime64[ns]', freq='2D')
```

```python
a = pd.date_range(start = '2022-12-08',     # 시작 날짜 
                  end = None,               # 끝 날짜 (None일때 생략 가능 O)
                  periods=12,               # 생성할 timestamp 개수
                  freq='2M')                # 시간 간격 
a

DatetimeIndex(['2022-12-31', '2023-02-28', '2023-04-30', '2023-06-30',
               '2023-08-31', '2023-10-31', '2023-12-31', '2024-02-29',
               '2024-04-30', '2024-06-30', '2024-08-31', '2024-10-31'],
              dtype='datetime64[ns]', freq='2M')
```

- Y : 연도

- M : 각 달의 마지막 날짜(일자)

- MS : 각 달의 첫 날 (1일)

- D : 일자별 

```python
# 기준 날짜 자유 / timestamp 10개 / 3달 간격 (시작 날짜)
pd.date_range('2023-06-15', periods=10, freq='3MS')

DatetimeIndex(['2023-07-01', '2023-10-01', '2024-01-01', '2024-04-01',
               '2024-07-01', '2024-10-01', '2025-01-01', '2025-04-01',
               '2025-07-01', '2025-10-01'],
              dtype='datetime64[ns]', freq='3MS')
```

# Period 배열

period_range() 함수

```python
# 두달 단위
twoM = pd.period_range('2009-07-12', periods=10, freq='2M')
twoM

PeriodIndex(['2009-07', '2009-09', '2009-11', '2010-01', '2010-03', '2010-05',
             '2010-07', '2010-09', '2010-11', '2011-01'],
            dtype='period[2M]')
```























