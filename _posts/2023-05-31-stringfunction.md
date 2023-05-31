---
layout: single
title: "문자열 관련 함수"
categories: python
tag: [crawling, stringfunction, jupyternotebook]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---



## 문자열 관련 함수 알아보기

### 문자열에 있는 특정 문자 갯수 세기 (count 함수)


```python
data = 'Dave David'
data.count('v')    # 문자열에 D 가 몇 번 나올까요? 대소문자도 구별함
```




    2



* 간단 연습: string에 v 는 몇 번 나올까?
* 간단 연습: string에 vid 는 몇 번 나올까? (꼭 문자 하나만 되는 것이 아니라, 연결된 문자열도 가능)

### 문자열에 있는 특정 문자의 위치 알려주기 
### index 함수


```python
string = 'Dave ID is dave'
string.index('i')   # 맨 앞 자리부터 0, 1, ... 순으로 위치를 표시
```




    8



* 간단 연습: string에 있는 D의 위치 확인하기 (가장 먼저 나오는 위치를 알려줌)
* 간단 연습: string에 x의 위치 확인하기 (해당 문자가 없으면 에러가 남)


```python
string.index('x')
```


    ---------------------------------------------------------------------------
    
    ValueError                                Traceback (most recent call last)
    
    <ipython-input-8-6fa4791dc117> in <module>()
    ----> 1 string.index('x')


    ValueError: substring not found


### 해당 문자가 문자열에 없을 때, 에러를 안낼 수는 없을까? --> find 함수를 사용하면 됨
* find 함수는 해당 문자가 문자열에 없으면 -1 을 리턴함 (에러는 내지 않음)
* 이외에는 index 함수와 동일


```python
string = 'dave is david'
string.find('x')


```




    -1




```python
string = 'Dave ID is dave'
string.find('D ')    # 맨 앞 자리부터 0, 1, ... 순으로 위치를 표시

if string.find('x') == -1:
    print ("x는 스트링에 없습니다.")
```

    x는 스트링에 없습니다.


* 간단 연습: string에 d의 위치 확인하기

### 문자열 사이에 다른 문자 넣기


```python
string = "12345"
comma = ','
comma.join(string)     # 껴넣을 문자.join(문자열)
```




    '1,2,3,4,5'



* 간단 연습: 문자열 사이에 ... 넣기

### 문자열 앞뒤에 공백 지우기
* 실제 현실 세계에 있는 문자열 데이터를 정리하다보면, 문자열 앞뒤에 공백 또는 기호가 있는 경우가 많음


```python
data = " Dave "
data.strip()        # 앞 뒤 공백을 다 지움
```




    'Dave'




```python
string2 = "     David  "
string2.strip()      # 앞 뒤 공백이 여러개 있어도 한번에 다 지움
```




    'David'




```python
string2.lstrip()      # 앞쪽(문자열 왼쪽)만 지우기 left strip 의 약자
```




    'David  '




```python
string2.rstrip()      # 앞쪽(문자열 오른쪽)만 지우기 right strip 의 약자
```




    '     David'




```python
string = "      9999999999999999(Dave)888888888888888888     "
string.strip(" 98()")        # 앞 뒤 괄호를 다 지움
```




    'Dave'



### 영문자 대소문자로 변환하기
* 대소문자를 구별하기 때문에, 대소문자 구별하지 않고 처리를 하기 위해 임의로 대문자 또는 소문자로 다 바꿀 때가 있음

### 소문자를 대문자로 바꾸기


```python
string = 'Dave'
string.upper()        # 본래 대문자는 놔두고, 소문자인 문자만 대문자로 변경
```




    'DAVE'



### 대문자를 소문자로 바꾸기


```python
string = 'Dave'
string.lower()        # 본래 소문자는 놔두고, 대문자인 문자만 대문자로 변경
```




    'dave'



### 문자열을 나누기


```python
string = "Dave goes to Korea"
string.split()       # split() 인자를 넣지 않으면 디폴트로 스페이스를 기준으로 분리
```




    ['Dave', 'goes', 'to', 'Korea']




```python
string = "Dave goes to Korea"
string.split()[3]
```




    'Korea'




```python

```


```python

```


```python
string.split(' ')    # 명시적으로 스페이스를 기준으로 분리하겠다고 넣어줘도 됨
```




    ['Dave', 'goes', 'to', 'Korea']




```python
string = "Dave/goes/to/Korea"
string.split('/')   # split() 인자를 넣으면 해당 인자를 기준으로 분리
```




    ['Dave', 'goes', 'to', 'Korea']



<div class="alert alert-block alert-success">
<strong><font color="blue" size="4em">프로그래밍 연습</font></strong><br>
string = "10,11,22,33,44" 를 컴마(,) 로 분리해서 리스트 변수를 만들어 각 값을 정수형 리스트 데이터로 넣기
</div>
<pre>
int('11') 을 실행하면 숫자 11로 변환
</pre>


```python
string = "10,11,22,33,44" 
string.split(',')

```




    ['10', '11', '22', '33', '44']




```python
string = "10,11,22,33,44" 
listdata = string.split(',')

for index, listitem in enumerate(listdata):
    listdata[index] = int(listitem)
print (listdata)
```

    [10, 11, 22, 33, 44]



```python

```


```python

```


```python
string = "10,11,22,33,44"
split_string = string.split(',')
for index, split_item in enumerate(split_string):
    split_string[index] = int(split_item)
print (split_string)
```

    [10, 11, 22, 33, 44]


### 문자열 중 일부를 다른 문자로 바꾸거나, 삭제하기


```python
string = "David goes to Korea"
string.replace("David", "Dave")     # David 를 Dave 로 바꾸기
```




    'Dave goes to Korea'




```python
string2 = "(Dave)"
string2.replace("()", "")           # 연결된 문자열은 해당 문자열이 동일하게 매칭이 되어야 함
```




    '(Dave)'




```python
string2.replace("(", "")
```




    'Dave)'




```python
string2.replace(")", "")
```




    '(Dave'




```python
string2 = string2.replace("(", "")   # ( 가 삭제된 문자열로 변수 값을 덮어 씌운 후에 다시 ) 를 삭제하면 됨
string2.replace(")", "")
```




    'Dave'




```python
string2 = "(Dave)"

string2.replace("(", "").replace(")", "")
```




    'Dave'



<div class="alert alert-block alert-success">
<strong><font color="blue" size="4em">프로그래밍 연습</font></strong><br>
string = "10,11,22,33,44" 에서 컴마(,) 를 / 으로 대체하기
</div>


```python
string = "10,11,22,33,44"
string.replace(",", " ")
```




    '10 11 22 33 44'




```python

```
