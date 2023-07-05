---
layout: single
title: "ElasticSearch"
categories: ai
tag: [machinlearning, java]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

### 매핑

ES에서는 JSON 형태의 데이터를 루씬이 이해할 수 있도록 바꿔주는 작업 ... => mapping

이 매핑을 엘라스틱 서치가 자동으로 하면 '다이나믹 매핑',

사용자가 직접 설명하면 '명시적 매핑'

1. 다이내믹 매핑 

index2에서는 따로 매핑 안해줬음 

그런데 도큐먼트가 인덱싱이 되었던 이유는 특별히 데이터 타입을 고민하지 않아도 

JSON 도큐먼트의 데이터 타입에 맞춰서 ES가 자동으로 인덱스 매핑을 해줌 

**_자료형_**

| 원본 데이터  타입 | 다이나믹매핑으로 변환된 데이터 타입         |
| ----------------- | ------------------------------------------- |
| null              | 필드 추가 X                                 |
| boolean           | boolean                                     |
| float             | float                                       |
| integer           | long                                        |
| object            | object                                      |
| string            | string 데이터에 따라 date or text / keyword |



예를 들어, JSON 도큐먼트에 "age" : "20" 이라는 값이 있다면 

ES는 20을 숫자(integer)로 인식을 함 

원본 소스가 integer 타입이기 때문에, 인덱스 생성시 age 필드를 long타입 필드로 매핑 

일단 편리하기는 한데... => 정수 숫자타입은 무조건 범위가 가장 넓은 long으로 매핑 

불필요한 메모리를 차지할 수 있으며, 문자열 같은 경우에는 검색, 정렬 등을 고려한 매핑이 제대로 되지 않는다 

ES는 인덱스 매핑값을 확인할 수 있는 mapping API를 제공함 

```json
GET index2/_mapping
```

2. 명시적 매핑

인덱스 매핑을 직접 정의하는 것을 명시적 매핑(Explicit Mapping)

인덱스를 생성할 때 mappings 정의하거나, mapping API를 이용해서 매핑을 지정할 수 있음 

```json
PUT index3
{
  "mappings":{
    "properties":{
      "age":{"type":"short"},
      "name":{"type":"text"},
      "gender":{"type":"keyword"}
    }  
  }  
}
```

인덱스를 생성할 때 mappings, properties 아래에 필드명과 필드 타입을 지정해주면 됨 

index3라는 새로운 인덱스를 생성하면서 명시적으로 매핑을 지정하고 있는데 ...

데이터 타입 
텍스트 : text, keyword 
날짜 : date,
정수 : byte, short, integer, long
실수 : float, double
논리 : boolean
위치정보 : geo-point(위도/경도), geo-shape(지형)
객체 : object
배열 : nested

- text 타입 : 일반적으로 문장을 저장하는 매핑 타입으로 사용 

PUT text_index

```json
{
  "mappings": {
    "properties": {
      "contents":{"type":"text"}
    }
  }
}
```

생성 후 도큐먼트 인덱싱

```json
PUT text_index/_doc/1
{
  "contents":"beautiful day"
}
```

쿼리 불러오기

```json
GET text_index/_search
{
  "query": {
    "match": {
      "contents": "day"
    }
  }
}
```

match는 전문 검색을 할 수 있는 쿼리이고, contents 필드에 있는 용어 중에 일치하는 용어가 있는 도큐먼트를 찾아주는 쿼리문 ...!

- keyword 타입

키워드 타입은 카테고리나 사람 이름, 브랜드 등 규칙성이 있거나 유의미한 값들의 집합,

즉 범주형 데이터에 주로 사용 

```json
PUT keyword_index
{
  "mappings": {
    "properties": {
      "contents":{"type": "keyword"}
    }
  }
}
```

생성 후 도큐먼트 인덱싱 

```json
 PUT keyword_index/_doc/1
{
  "contents":"beautiful day"
}
```

쿼리 불러오기

```json
GET keyword_index/_search
{
  "query": {
    "match": {
      "contents": "beautiful"
    }
  }
}
```

쿼리의 결과 도큐먼트를 하나도 찾지 못함 

text_index와 다르게 keyword_index는 정확히 beautiful day라고 입력해야 도큐먼트를 찾음 

키워드 타입은 문자열 전체를 하나의 용어로 보고 인덱싱을 하기 때문에 텍스트가 정확히 일치하는 경우에만 값을 검색함 

- 멀티 필드 

멀티 필드는 단일 필드 입력에 대해 여러 하위 필드를 정의하는 기능

이 때 fields라는 매핑 파라미터가 사용되는데 하나의 필드를 여러 용도로 사용할 수 있게

```json
PUT multifield_index
{
  "mappings": {
    "properties": {
      "message":{"type":"text"},
      "contents":{
        "type":"text",
        "fields": {
          "keyword":{"type":"keyword"}
        }
      }
    }
  }
}
```

message, contents라는 2개의 필드를 가진 인덱스를 생성 

그 중 contents필드는 멀티타입으로 설정했고, 텍스트 타입이면서 키워드 타입을 갖는다 

생성 후 도큐먼트 인덱싱 

```json
PUT multifield_index/_doc/1
{
  "message":"1 document",
  "contents":"beautiful day"
}

PUT multifield_index/_doc/2
{
  "message":"2 document",
  "contents":"beautiful day"
}
PUT multifield_index/_doc/3
{
  "message":"3 document",
  "contents":"beautiful day"
}

```

match 쿼리를 이용해 도큐먼트 찾기 

```json
GET multifield_index/_search
{
  "query": {
    "match": {
      "contents": "day"
    }
  }
}
```

contents는 멀티 필드지만, 기본적으로 text 타입으로 매핑되어있음

그래서 모두 'day'라는 용어를 포함하고 있기 때문에 모두 검색이 되었다 

다른 방식으로 검색 ! 

```json
GET multifield_index/_search
{
  "query": {
    "term": {
      "contents.keyword": "day" 
    }
  }
}
```

term은 온전히 그 검색어와 일치하는 문서를 검색 

contents 뒤에 keyword를 추가해 하위 필드를 참조할 수 있다 

contents.keyword 타입은 키워드 타입이니까, 'beautiful day', 'wonderful day' 로 
검색해야 결과가 나올 것! 

키워드 타입은 집계에서 활용도가 큰 편

```json
GET multifield_index/_search
{
  "size":0,
  "aggs":{
    "contents":{
      "terms": {
        "field": "contents.keyword"
      }
    }
  }
}
```

size 옵션은 최대로 볼 인덱스 수를 나타내는데... => 결과만 볼거라서 0

aggs는 집계를 위한 쿼리이고, 결과에서 볼 수 있듯이 "beautiful day" 라는 도큐먼트 2개,
"wonderful day"라는 도큐먼트 1개가 그룹핑이 된다



<hr>
### 분석기

ES는 전문 검색을 지원하기 위해 '역인덱싱'이라는 기술을 사용함 

전문 검색은 장문의 문자열에서 부분 검색을 수행하는 것이고, 

역인덱싱은 장문의 문자열을 분석해서 작은 단위로 쪼개서 인덱싱을 하는 기술 

분석기는 입력 받은 문자열을 변경하거나 불필요한 문자들을 제거하는 **캐릭터 필터**, 

문자열을 토큰으로 분리하는 **토크나이저**,

분리된 토큰들의 필터작업(대소문자 구분, 형태소 분석)을 하는 **토큰 필터**

3가지의 구성요소로 나뉘고, 분석기에는 반드시!!! 하나의 토크나이저가 포함되어야 함

### 분석기 API

ES는 필터와 토크나이저를 테스트 해볼 수 있는 analyze라는 이름의 REST API를 제공함 

```json
POST _analyze
{
  "analyzer": "stop",
  "text":"The 10 most loving dog breeds."
}
```

analyzer에 원하는 분석기를 선택하고, text에 분석할 문자열을 입력 

stop 분석기는 소문자 변경 토크나이저와 스톱 토큰 처리 필터로 구성되어있음 

결과에서 [most, loving, dog, breeds] 4개의 토큰으로 분리되었고,
The, 10 은 불용어로써 처리되었다

그 이외에 standard, simple, whitespace 등 다양한 분석기가 있는데...!

standard : 제일 기본적인 분석기, 영문법을 기준으로 한 토크나이저와 소문자 변경필터, 스톱필터가 포함이 되어있음 

simple : 문자만 토큰화한다. 공백, 숫자 ,하이픈, 작은 따옴표 같은 문자는 토큰화하지 않음 

whitespace : 공백을 기준으로 구분하여 토큰화함 

stop : simple 분석기 + 불용어 제거 

### 토크나이저

분석기는 반드시 하나의 토크나이저를 포함해야 함 

토크나이저는 문자열을 분리해 토큰화하는 역할

분석기에 반드시 포함되어야 하기 때문에 형태에 맞는 토크나이저 선택이 중요함



**자주 쓰이는 토크나이저**

standard : 스탠다드 분석기가 사용하는 토크나이저, 특별한 설정이 없으면 기본 토크나이저로 사용

쉼표, 점 같은 기호를 제거하며, 텍스트 기반으로 토큰화



lowercase : 텍스트 기반으로 토큰화하며, 모든 문자를 소문자로 변경해 토큰화 



ngram : 텍스트 원문으로부터 N개의 연속된 글자 단위를 모두 토큰화

ex) '엘라스틱서치'라는 원문을 2gram으로 토큰화하면

=> [엘라, 라스, 스틱, 틱서, 서치] 와 같이 연속된 두 글자로 모두 추출 

원문으로부터 검색할 수 있는 거의 모든 조합을 얻어낼 수 있기 때문에 
정밀한 부분 검색에 강점이 있지만, 토크나이징을 수행한 N개 이하의 글자 수로는 검색이 불가능.

모든 조합을 추출하기 때문에 저장공간을 많이 차지함

uax_url_email : 스탠다드 분석기와 비슷하지만, URL이나 이메일을 토큰화하는데 강점 !

```json
POST _analyze
{
  "tokenizer": "uax_url_email",
  "text":"email: ollehbk@google.com"
}
```



### 필터

분석기는 하나의 토크나이저와 다수의 필터로 조합된다

분석기에서 필터는 옵션으로 하나 이상을 포함할 수는 있지만, 없어도 분석기를 돌리는데 큰 문제는 없다

필터가 없는 분석기는 토크나이저만 이용해서 토큰화 작업을 진행하는데, 

필터를 통해 더 세부적인 작업이 가능하기 때문에, 대부분 분석기들은 하나 이상의 필터를 포함

필터는 단독으로 사용할 수 없고, 반드시 토크나이저가 있어야 함 

```json
POST _analyze
{
  "tokenizer": "standard",
  "filter": ["uppercase"],
  "text": "show me the money"
}
```

스탠다드 토크나이저에 uppercase라는 토큰 필터를 적용 

필터는 [] 대괄호 안에 여러개를 적을 수 있는데, 적은 순서대로 필터가 적용됨 

결과를 확인하면 uppercase필터의 영향을 받아 모두 대문자로 변경된 것을 확인할 수 있음 



- 캐릭터 필터

  - 토크나이저 전에 위치하며, 문자들을 전처리해주는 역할을 하는데 

    HTML 문법을 제거/변경하거나 어떤 특정한 문자가 왔을 때 다른 문자로 대체하는 작업 

    예를 들어, HTML ```&nbsp;``` 문자가 오면 공백으로 바꾸는 작업 등을 캐릭터 필터에서 진행하면 편해짐 

    그런데 ES가 제공하는 대부분의 분석기는 캐릭터 필터가 포함되어 있지 않은데,

    캐릭터 필터를 사용하기 위해서는 커스텀 분석기를 만들어서 사용하는 것이 좋음 ! 

- 토큰 필터

  - 토크나이저에 의해 토큰화되어 있는 문자들에 필터를 적용
  - 토큰들을 변경하거나 삭제하고 추가하는 작업들이 가능
  - 토큰 필터 역시 종류가 다양하고, 자주 변경되기 때문에 자주 사용 할만한것 소개...
    - lowercase : 모든 문자를 소문자로 변경 <-> uppercase
    - stemmer : 영어 문법을 분석하는 필터
      - 형태소를 분석해 어간을 분석
      - 언어마다 고유한 문법이 있어서 필터 하나로 모든 언어에 대응하기는 힘들다 
      - 한글의 경우에는 아리랑, 노리 같은 필터가 있다 
    - stop : 기본 필터에서 제거하지 못한 특정한 단어(불용어)를 제거함 



### 커스텀 분석기

ES에서 제공하는 내장 분석기들 중 원하는 기능을 만족하는 분석기가 없을 때 

사용자가 직접 토크나이저, 필터 등을 조합해서 사용할 수 있는 분석기 

필터들의 조합이나 순서에 따라서 특별한 형태의 분석기가 될 수 있다 

```json
PUT customer_analyzer
{
  "settings": {
    "analysis": {
      "filter": {
        "my_stopwords":{
          "type":"stop",
          "stopwords":["lions"]
        }
      },
      "analyzer": {
        "my_analyzer":{
          "type":"custom",
          "char_filter":[],
          "tokenizer":"standard",
          "filter":["lowercase","my_stopwords"]
        }
      }
    }
  }
}
```



customer_analyzer 라는 새로운 인덱스를 생성

인덱스 설정(settings)에 analysis 파라미터를 추가하고,

그 밑에 필터(filter) 와 분석기(analyzer)를 만들었음 

먼저 분석기의 이름을 지정(my_analyzer)하고, 타입을 custom으로 지정하면 커스텀분석기를 의미

분석기에는 반드시 토크나이저가 하나 들어가야하는데 기본 스탠다드 토크나이저를 사용,

캐릭터 필터(char_filter)는 사용하지 않았음 

토큰 필터는 2개 사용했는데, 소문자 변경 필터(lowercase), 사용자가 만든 필터(my_stopwords)를 사용

사용자가 만든 필터는 analysis 파라미터 밑에 필터에서 이름을 지정하고, 

타입과 그 타입에 맞는 설정을 작성 

=> 스톱필터에 사용자가 지정한 'lions'라는 단어를 불용어로 인식하도록 설정

'ions' 단어가 나오면 불용어로 처리함 



분석기 API를 이용해서 토큰화가 어떻게 되는지 알아보자

```json
GET customer_analyzer/_analyze
{
  "analyzer": "my_analyzer",
  "text": "Cats Lions Tigers Dogs"
}
```

결과를 확인해보면 먼저 스탠다드 토크나이저에 의해 

[Cats, Lions, Tigers, Dogs] 로 토큰화되고, 소문자 변경 필터에 의해

[cats, lions, tigers, dogs]로 변경이 됨 

마지막으로 my_stopwords라는 커스텀 필터에 의해 [lions]가 불용어가 되어 제거됨 

필터의 적용 순서는 필터 배열의 첫번째 순서부터 필터가 적용되므로, 

필터를 여러개 사용한다면 필터의 순서에도 주의해야함!!!!



### 노리 분석기

노리 분석기는 한글 텍스트를 분석하기 위한 분석기 플러그인

1. ```C:\Windows\System32\cmd.exe``` 에서 cmd 실행 후 

```.\bin\elasticsearch-plugin install analysis-nori``` 실행해서 설치

2. 설치 후 ES / 키바나 종료 후 재실행
3. 노리 분석기 테스트

노리 분석기는 nori_tokenizer 라는 토크나이저와

nori_part_of_speech, nori_readingform이라는 2개의 필터로 구성되어있고,

형태소 분석에 특화되어있다!

```json
POST _analyze
{
  "tokenizer": "nori_tokenizer",
  "text":"김비버는 짱짱맨이다."
}
```

```json
{
  "tokens" : [
    {
      "token" : "김",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "비버",
      "start_offset" : 1,
      "end_offset" : 3,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "는",
      "start_offset" : 3,
      "end_offset" : 4,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "짱짱",
      "start_offset" : 5,
      "end_offset" : 7,
      "type" : "word",
      "position" : 3
    },
    {
      "token" : "맨",
      "start_offset" : 7,
      "end_offset" : 8,
      "type" : "word",
      "position" : 4
    },
    {
      "token" : "이",
      "start_offset" : 8,
      "end_offset" : 9,
      "type" : "word",
      "position" : 5
    },
    {
      "token" : "다",
      "start_offset" : 9,
      "end_offset" : 10,
      "type" : "word",
      "position" : 6
    }
  ]
}

```



```json
POST _analyze
{
  "tokenizer": "standard",
  "text":"김비버는 짱짱맨이다."
}
```

```json
{
  "tokens" : [
    {
      "token" : "김비버는",
      "start_offset" : 0,
      "end_offset" : 4,
      "type" : "<HANGUL>",
      "position" : 0
    },
    {
      "token" : "짱짱맨이다",
      "start_offset" : 5,
      "end_offset" : 10,
      "type" : "<HANGUL>",
      "position" : 1
    }
  ]
}

```



<hr>


### 범위 쿼리(날짜, 범위)

특정 날짜나 숫자의 범위를 지정해서 범위 안에 포함된 데이터를 검색할 때 사용

날짜 / 숫자 / IP 타입의 데이터는 범위 쿼리가 가능하지만, 문자형 / 키워드 타입에는 X !



**샘플 데이터 가져오기**

_키바나 홈으로 이동 - 우측 하단의 Try our sample data 링크로 이동_

_Sample fight data의 Add data 클릭 - 홈 - 콘솔로 이동_ 

kibana_sample_data_flights 인덱스의 timestamp 필드를 이용해서 쿼리 요청을 해보자! 

```json
GET kibana_sample_data_flights/_search
{
  "query": {
    "range": {
      "timestamp": {
        "gte": "2023-07-04",
        "lte": "2023-07-05"
      }
    }
  }
}
```



timestamp 필드는 yyyy-mm-dd 형식의 포맷을 사용하는데 

날짜 포맷 형태를 다르게 넣으면 parse_exception 오류가 발생

(ex: 2023/07/04)

검색 범위를 지정하는 파라미터로는 gte (이상) / gt (초과) / lte (이하) / lt (미만)

<hr>


### 키바나 맵스 

키바나는 ES에서 제공되는 시계열 데이터나 위치 분석같은 다양한 분석결과를 시각화한다

그 중에서도 위치 기반 데이터를 지도 위에 표현할 수 있는 Maps 시각화 기능을 볼 것 ! 

키바나 맵스를 통해 사용자는 위치 정보가 포함된 데이터를 지도에 올려서 시각화 할 수 있고,

멀티레이어 기능을 통해 다양한 형태의 지도를 화면에서 볼 수 있다 

_키바나 홈 좌측 메뉴 Maps - create map_



### 윈도우 웹서버 구축

레이어 추가하기에 앞서 사용자가 만든 GeoJSON 파일을 가져오려면 웹서버가 필요 !

**엔진엑스(nginx)**라는 것을 설치할건데 설치가 쉽고 빨라서 많이  사용되는 웹서버 프로그램!



웹서버 실행전에 설정파일 추가...!

conf 폴더의 nginx.conf 파일을 수정 ! 

_변경전_

```
location / {
            root   html;
            index  index.html index.htm;
        }
```

_변경후_

```
 location / {
            root   html;
			add_header Content-Security-Policy "default-src 'self';";
			add_header 'Access-Control-Allow-Origin' 'http://localhost:5601';
            index  index.html index.htm;
        }
```

첫 번째줄의 CSP(Content-Security-Policy)는 웹 보안 정책 중 하나로 각종 악성 스크립트를 막기 위해서 사용 

두 번째줄의 Access-Control-Allow-Origin 은 스프링 때도 봤듯이, 요청을 보내는 주소(키바나 서버)와 받는 주소(엔진엑스)가 다를 경우 CORS(Cross-Origin-Resource Sharing) 오류가 발생하는데... 이를 해결하고자 서버 응답 헤더에 프론트 주소를 적어주는 것!

이 두줄을 넣어주지 않으면 사용자 지도 파일을 가져올 수 없다 !  

**cf) 종료하고 싶다면**

> C:\nginx-1.24.0 에서 cmd 실행 후 
>
> ```nginx.exe -s stop``` 명령 실행하기 or 작업 관리자에서 작업 끝내기

웹서버가 구축되었다면 사용자 지정 타일맵과 GeoJSON을 적용!

키바나는 위치/지역 정보를 처리할 수 있는데, 위치는 단순 좌표이고,

지역은 위치가 모여서 만드는 폴리곤(polygon, 다각형)으로 키바나에서는 벡터 레이어라고 부름 

ES가 제공하는 벡터 레이어는 https://maps.elastic.co 사이트에서 확인할 수 있음 



전 세계 행정구역 정보를 Vector layers 라는 폴리곤 단위로 제공하는데,

한국에 대해서는 시도, 시군구 단위의 폴리곤을 제공함



**서울시 우편번호 폴리곤이 담겨있는 GeoJSON 파일을 벡터레이어로 사용해보자!**

> 받은 파일(TL_KODIS_BAS_11.geojson)을 .geojson파일을 ```C:\nginx-1.24.0\html``` 폴더에넣기 

html 폴더는 엔진엑스에서 root가 되는 웹사이트 경로 



> 그 다음에 키바나 설정파일 (kibana.yml)의 수정이 필요 (C:\kibana-7.12.0-windows-x86_64\config)

파일 제일 마지막에 아래 내용을 추가 후 저장 

```yaml
map.regionmap:
    layers:
      - name: "SEOUL ZIPCODE"
        url: "http://localhost/TL_KODIS_BAS_11.geojson"
        attribution: "INRAP"
        fields:
            - name: "BAS_MGT_SN"
              description: "zipcode number"


map.tilemap.url:
    "https://mt1.google.com/vt/lyrs=s&x={x}&y={y}&z={z}"

```

이 YAML 파일은 들여쓰기에 신경 써줘야 하는데

> http://www.yamllint.com/ 에서 입력오류가 없는지 확인 !!



map.regionmap은 사용자 벡터 레이어를 설정

layers 밑에 레이어 이름을 정하고,

URL을 적어주는데 키바나 서버와 도메인의 CORS가 가능해야 함

attribution은 GeoJSON 파일을 참고하는 방법을 정의한 것 

fields 는 GeoJSON 속성 중 노출할 필드를 정의한 것 

그 중 BAS_MGT_SN이라는 필드는 우편번호를 나타내는 고유값 



map.tilemap.url은 사용자 지정 타일 맵 => 구글에서 제공하는 위성 사진 타일맵



> 키바나 서버 껐다가 다시 실행 ! => 키바나 페이지는 홈으로 이동 => 키바나 맵스

위성사진 보는거 불편하시면 tilemap에 대한 내용을 주석처리하고 다시 실행할 것 ! 



### 클러스터와 그리드

- 클러스터
- 1. map에서 create map 으로 가서 add layer 누르고 cluster and grids 선택
  2. index pattern은 kibana_sample_data_flight 선택

- 그리드



### GeoJSON 적용하기

EMS(Elastic Map Service)에서 제공하는 벡터 레이어 이외에 사용자가 만든 벡터 레이어 등록 필요할 때 사용하는 기능 

GeoJSON은 위치 정보를 JSON 형태로 표현하는 포맷 

점, 선, 다각형 등을 지형지물과 연계해 위치를 표시하는 용도로 사용 

타입과 좌표를 이용해서 영역을 표시하고 프로퍼티에 설명도 적을 수 있음 



> Maps - Add layer - Configured GeoJSON 을 선택

서울 지역 우편번호 경계선을 표현하는 GeoJSON 파일! 

이제 이 벡터레이어와 조인할 도큐먼트가 필요 ! 

현재는 우편번호를 가진 도큐먼트가 없기 때문에 간단하게 인덱스 만들기 



> 새로운 크롬창 => 키바나 접속 => 콘솔 

```
PUT kibana_ems_index/_doc/1
{
  "BAS_MGT_SN":"1156000102"
}

PUT kibana_ems_index/_doc/2
{
  "BAS_MGT_SN":"1156000100"
}

PUT kibana_ems_index/_doc/3
{
  "BAS_MGT_SN":"1156000044"
}

GET kibana_ems_index/_search
```

3개의 인덱스가 잘 들어있는거 확인됨!

키바나에서 사용할 수 있는 인덱스 패턴도 지정

> 키바나 홈 - Manage - 화면 왼쪽에 Index Patterns - Create index pattern 버튼 - 패턴 이름을 kibana_ems_index로 지정하고 Next step - Create index pattern 버튼 

> Map으로 돌아와서 Add layer - Configured GeoJSON - SEOUL ZIPCODE - Term joins Add join 

<hr>


### 공공데이터 분석

cmd 열어서 

```pip install requests``` 

```pip install elasticsearch```



파이썬 코드 실행 전에...

한글 분석기와 위경도 데이터 타입을 이용해야하기 때문에 

**반드시 인덱스 매핑부터 진행해야함...!**

```
PUT seoul_wifi
{
  "settings": {
    "analysis": {
      "analyzer": {
        "korean":{
          "tokenizer": "nori_tokenizer"
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "gu_nm":{"type":"keyword"},
      "place_nm":{"type": "text", "analyzer": "korean"},
      "instl_xy":{"type":"geo_point"}
    }
  }
}
```

파이썬 코드 실행 ~~

> cmd 
>
> ```pip uninstall elasticseasrch```
>
> ```pip uninstall elastic-transport```
>
> ```pip install elasticsearch==7.12.0```

파이썬 코드 실행 ~~

```python
# -*- coding: utf-8 -*-

# 서울 열린 데이터광장 - 서울시 공공와이파이 서비스 
# http://openapi.seoul.go.kr:8088/(인증키)/xml/TbPublicWifiInfo/1/5/

# 데이터는 총 23304개
import urllib.request
from xml.etree.ElementTree import ElementTree, fromstring

from elasticsearch import Elasticsearch


# Elasticsearch() 함수는 ES설정과 관련한 다양한 파라미터를 지원하는데,
# 아무 설정도 하지 않으면 localhost와 9200번 포트에 접속을 함 
es = Elasticsearch()
for i in range(1, 25):
    iStart = (i - 1) * 1000 + 1
    iEnd = i * 1000

    url = 'http://openapi.seoul.go.kr:8088/(인증키)/xml/TbPublicWifiInfo/'
    url += str(iStart) + '/'
    url += str(iEnd) + '/'

    # GET방식 요청으로 얻은 데이터를 response 변수에 저장
    response = urllib.request.urlopen(url)

    # 한글 데이터가 있기 때문에 utf-8로 디코딩해서 변수에 저장
    xml_str = response.read().decode('UTF-8')

    # XML 파싱한 데이터를 변수에 넣기
    tree = ElementTree(fromstring(xml_str))

    # getroot()를 통해 최상단 루트태그를 얻을 것
    root = tree.getroot()

    # 루프 돌면서 좌표, 설치된 시군구, 설치장소 등을 doc변수에 저장
    for row in root.iter('row'):
        try:
            gu_nm = row.find('X_SWIFT_WRDOFC').text
            place_nm = row.find('X_SWIFT_MAIN_NM').text
            place_x = float(row.find('LAT').text)
            place_y = float(row.find('LNT').text)
            doc = {
                "gu_nm": gu_nm,
                "place_nm": place_nm,
                "instl_xy": {
                    "lat": place_y,
                    "lon": place_x
                }
            }
            # index 함수를 통해 seoul_wifi라는 인덱스에 방금 저장된 doc를 인덱싱
            res = es.index(index="seoul_wifi", doc_type="_doc", body=doc)
        except Exception as e:
            pass
    print("END", iStart , "~" , iEnd)
print("END")




```

place_nm은 데이터가 한글로 되어있다

한글의 경우 ES에서 제공하는 노리 한글 형태소 분석기를 사용해야 

제대로 된 검색을 할 수 있음 

와이파이 설치 위치인 위경도 데이터는 geo_point 데이터 타입으로 설정

여기서 install_xy필드를 geo_point 데이터 타입으로 매핑하고, 

실제 데이터 저장은 파이썬의 객체 형식을 이용함

```
 "instl_xy" : {
                "lat": place_y,
                "lon":place_x        
            }
```



도큐먼트에서 lat, lon 으로 배열을 만들어서 geo_point 필드에 인덱싱하면

위치 관련한 필드가 되므로 위치 관련 쿼리들을 수행할 수 있음



> ```GET seoul_wifi/_search```로 결과 확인!

10000개 이상(gte)의 데이터가 들어가 있는 것을 확인할 수 있음



> 키바나에서 seoul_wifi 라는 인덱스 패턴 만들기

> 키바나 홈 => Manage => 화면 왼쪽에 Index Patterns - Create index pattern 버튼 - 패턴이름을 seoul_wifi로 지정하고 Next step 버튼 - Create index pattern 버튼  

<hr>


> Map 열어서 서울 지도 열고

> Add layer 로 Document 항목 선택해서 seoul_wifi 고르기 

- Scailing 은 Show clusters when ~ 고르기
- add layer 버튼 누르기
- Filtering 쪽에서 gu_nm:"구이름"  and place_nm:"도서관" 골라서 조건 검색 => 결과 확인 







