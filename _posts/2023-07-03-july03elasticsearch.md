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



# Elastic Search (엘라스틱 서치)
엘라스틱서치는 Java 언어로 이루어진 오픈 소스 형태의 정보 검색 라이브러리 기반인
Apache Lucene(아파치 루씬)을 기반으로 만들어진 검색 엔진! 

초창기에는 Java만 사용해야 했지만, 추가적인 개발을 통해서 C, 파이썬 등 다른 프로그래밍 언어를
사용할 수 있도록 개발되었음 

**검색 엔진**이란 웹 상에 존재하는 정보와 웹 사이트를 검색하기 위한 프로그램 

그 프로그램으로 검색 서비스를 제공하는 곳이 '검색 사이트'이지만, 사실상 검색 엔진이라는 용어와 같이 사용되고 있다. 

ex) 네이버, 구글, 다음,...

엘라스틱서치는 검색 엔진이지만, NoSQL 데이터를 저장해서 이것들을 검색하기 때문에
NoSQL의 특성 역시도 가지고 있다

_용어_

- ES에서 사용하는 대부분의 개념은 대부분 RDBMS에도 있는 개념 



| ElasticSearch | OracleDB(RDBMS) |
| ------------- | --------------- |
| index         | Database        |
| type          | Table           |
| Field         | Column(Field)   |
| Document      | Row(data 한 줄) |



| ES HTTP METHOD | RDBMS SQL     |
| -------------- | ------------- |
| GET            | SELECT        |
| PUT            | INSERT        |
| POST           | UPDATE/SELECT |
| POST           | DELETE        |



1. 클러스터 (Cluster)

- ES에서 가장 큰 시스템을 의미
- 최소 하나 이상의 노드로 이루어진 노드들의 집합
- 여러대의 서버가 하나의 클러스터로 구성될 수도 있고, 하나의 서버에 여러 클러스터가 존재하기도 함 

2. 노드 (Node)

- MasterNode : 클러스터 관리 / 노드 추가하거나 제거
- DataNode : 실질적 데이터 저장, 검색과 통계등의 데이터 관련 작업 수행



<hr>

_장점_

- 오픈 소스의 검색엔진
- 전문 검색 (검색 성능이 빠르다)
- 정형화되지 않은 문서도 색인, 검색하는 것이 가능
- 시각화 (Kibana) : 키바나와 연동해서 실시간 로그 분석 / 시각화 가능
- Restful API : 요청이나 응답에 JSON을 이용 => HTTP 프로토콜을 통해 데이터를 검색, 추가, 수정, 삭제할 수 있음 
- 역색인 (Inverted Index) : 책 맨 뒤에 키워드마다 찾을 수 있도록 해놓는 부분과 같은 내용



_단점_

- 운영이 복잡하다
- 메모리 사용량이 높다
- 문서의 크기가 큰 경우 처리 속도가 느려질 수 있다 
- 완전하 실시간성을 보장하지 않는다 
  - ex) 업데이트 명령의 경우 : 기존 문서를 삭제하고 새로운 문서를 생성 



<hr>
### 실행

1. C:\elasticsearch-7.12.0\bin 에서 elasticsearch.bat 파일 실행
2. 상위 폴더(C:\elasticsearch-7.12.0)에서 cmd 실행 & ```curl -X GET "localhost:9200/?pretty" ```명령어 실행
3. 응답 결과를 JSON 형태로 보내는데, URL뒤에 ?pretty 를 추가하면 가독성이 좋은 형태로 결과 제공 ! 
   1. - curl: 다양한 통신 프로토콜을 지원하여 데이터를 전송할 수 있는 소프트웨어 
      - 주로 사용되는 옵션은  
        - -X : 요청 시 사용할 메소드를 정한다 [GET/POST/PUT/DELETE]
        - -d : 데이터를 전송할 수 있다. POST 요청에만 사용
        - -o : 받아온 데이터를 모니터에 보여주는게 아닌 파일 형태로 저장

4. 크롬에 localhost:9200 으로 접속해서 실행되는지 확인! 

<hr>
### 키바나 설치

ES의 관리, 모니터링, 솔루션을 총괄하는 메인 UI 이다!

ES에서 제공되는 시계열 데이터나 위치 분석같은 다양한 분석결과를 시각화



### 실행 

1. bin 경로안까지 가서 kibana.bat 파일 실행 
2. 정상적으로 켜졌으면 크롬에서 localhost:5601 주소로 요청!
   1. 엘라스틱 서치를 먼저 실행한 후 키바나를 실행 !!

3. 화면에 Add data와 Explore on my own 두가지 버튼이 있는데
   1. Add data : 샘플 데이터 불러오기
   2. Explore on my own : 홈으로 가기 

<hr>
### 엘라스틱 서치 Basic

ES의 모든 요청과 응답은 REST API 형태로 제공됨

REST는 'Representational State Transfer' 의 약자로 HTTP의 장점을 이용해 자원을 주고받는 형태이고,

REST API는 REST 를 기반으로 API를 서비스하는 것을 의미

REST API는 4가지 메소드 타입(GET/POST/PUT/DELETE)를 가지고 CRUD 작업을 진행



_키바나 콘솔 사용법_

키바나 화면의 Dev Tools에 있는 콘솔을 이용해 REST API를 호출할 것! 

키바나 콘솔을 이용하면 복잡한 앱 개발이나 프로그램 설치 없이 ES와 REST API로 통신할 수 있다 

왼쪽 입력창에서 ES에서 제공하는 REST API를 입력하고 재생버튼 or Ctrl + Enter를 누르면 HTTP 요청을 하게 되고, 오른쪽 출력창에서 HTTP의 응답을 확인할 수 있음 

ES API 자동완성 기능이 지원되기 때문에 외울 필요 X



_시스템 상태확인_

ES의 현재 상태를 빠르게 확인할 수 있는 방법으로 cat API를 일반적으로 사용함

cat은 'compact and aligned text' 의 약어로 콘솔에서 시스템 상태를 확인할 때 가독성을 높일 목적으로 만들어진 API => 키바나 콘솔에 GET _cat 입력 후 실행 

<hr>

도큐먼트는 반드시 하나의 인덱스에 포함되어야 함!

도큐먼트의 CRUD 동작을 하기 위해서는 반드시 인덱스가 있어야 한다 !!

**인덱스 생성**

> PUT index1

index1이라는 이름의 인덱스를 생성하는 API

PUT은 생성이나 수정을 위한 HTTP 메소드! 

**인덱스 확인**

> GET index1

확인해보면 index1의 인덱스 설정값 등을 확인할 수 있다 ! 

**인덱스 삭제**

> DELETE index1

지금은 인덱스 내에 도큐먼트가 하나도 없지만, 인덱스를 삭제하면 인덱스에 저장되어있는 도큐먼트들도 모두 삭제되니 주의!!

<hr>

### Document CRUD


1. 도큐먼트 생성

   ES에서 도큐먼트를 인덱스에 포함시키는 것을 indexing(인덱싱)이라고 함

   > PUT index2/_doc/1
   >
   > {
   >
   > "name": "oz",
   >
   > "age": 10,
   >
   > "gender": "male"
   >
   > }

로 요청을 하면 존재하지 않았던 index2라는 인덱스를 생성하면서 동시에 인덱스에 도큐먼트를 인덱싱

index2는 인덱스 이름, _doc은 엔드포인트 구분을 위한 예약어, 숫자 1은 인덱싱할 도큐먼트의 고유 아이디 

index2 인덱스가 정상적으로 생성되었는지 확인!

> GET index2 

우리가 직접 데이터 타입을 지정하지 않아도 ES는 필드와 값을 보고 자동으로 자료형을 지정하는데, 이걸 다이나믹매핑 (Dynamic Mapping)이라고 함 



index2 인덱스에 country라는 이름의 필드가 추가된 도큐먼트를 인덱싱

> PUT index2/_doc/2
>
> {
>
> "name": "oz",
>
> "country": "korea"
>
> }

2번 도큐먼트는 country 필드가 추가되었고, 기존에 있던 age, gender 필드는 사용하지 않지만 문제없이 인덱싱이 된다 

이번엔 데이터타입을 잘못 입력한 도큐먼트를 인덱싱

> PUT index2/_doc/3
>
> {
>
> "name": "Oh",
>
> "age" : "20",
>
> "gender" : "female"
>
> }

index2 인덱스는 age를 실수(long)타입으로 매핑했는데 3번 도큐먼트는 텍스트 타입으로 입력함 

RDBMS의 경우라면 오류가 발생했겠지만, 스키마에 유연하게 대처하는 ES는 타입을 변환해서 저장함 

ES는 혹시 모를 사용자의 입력 실수를 고려해서 자동으로 데이터 형변환을 진행하는데... 대표적으로 

- 숫자 필드에 문자열이 입력되면 숫자로 변환을 시도한다 ex) "age": "10" => 10
- 정수 필드에 소수가 입력되면 소수점 아래 자리를 무시한다 ex) "age" : 10.0 => 10



2. 도큐먼트 조회

- 도큐먼트의 아이디를 사용해서 조회

> GET index2/_doc/1

인덱스명과 도큐먼트 아이디를 이용해서 특정 도큐먼트의 데이터를 가져올 수 있음 

실제로는 도큐먼트를 하나씩 읽어오는 건 드물고, search라는 DSL 쿼리를 이용해서 도큐먼트 조회

> GET index2/_search



3. 도큐먼트 수정

   index2의 1번 도큐먼트의 특정 필드값을 변경

   > PUT index2/_doc/1
   >
   > {
   >
   > "name":"kim",
   >
   > "age":17,
   >
   > "gender":"female"
   >
   > }

기존 1번 도큐먼트의 필드값이 변경되었음 

사실 도큐먼트를 인덱싱하는 과정에서 같은 도큐먼트의 아이디가 있으면 덮어쓰기가 되는 형태인데 .. 마치 업데이트 된 것처럼 보임 

update API를 이용해서 업데이트를 할 수 도 있는데

> POST index2/_update/1
>
> {
>
> "doc":{
>
> "name":"lee
>
> }
>
> }

_update라는 엔드포인트를 추가해 특정 필드의 값만 업데이트 할 수 있다 



4. 도큐먼트 삭제

> DELETE index2/_doc/2



<hr>


















