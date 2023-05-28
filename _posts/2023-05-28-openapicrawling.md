---
layout: single
title: "open API 크롤링"
categories: python
tag: [crawling, openAPI, jupyternotebook]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---





## Open API(Rest API)를 활용한 크롤링

### Open API(Rest API)란?
 - **API:** Application Programming Interface의 약자로, 특정 프로그램을 만들기 위해 제공되는 모듈(함수 등)을 의미
 - **Open API:** 공개 API라고도 불리우며, 누구나 사용할 수 있도록 공개된 API (주로 Rest API 기술을 많이 사용함)
 - **Rest API:** Representational State Transfer API의 약자로, HTTP프로토콜을 통해 서버 제공 기능을 사용할 수 있는 함수를 의미
   - 일반적으로 XML, JSON의 형태로 응답을 전달(원하는 데이터 추출이 수월)
   - [참고 - RestAPI란](http://hyunalee.tistory.com/1)

### JSON 이란?
 - JavaScript Object Notation 줄임말
 - 웹환경에서 서버와 클라이언트 사이에 데이터를 주고 받을때 많이 사용
   - Rest API가 주요한 예제
 - JSON 포멧 예 <br>
 { "id":"01", "language": "Java", "edition": "third", "author": "Herbert Schildt" }
  <br>
  <br>



```python
import json

# 네이버 쇼핑에서, android 라는 키워드로 검색한 상품 리스트 결과
data = """
{
    "lastBuildDate": "Sat, 22 Jun 2019 14:57:13 +0900",
    "total": 634151,
    "start": 1,
    "display": 10,
    "items": [
        {
            "title": "MHL 케이블 (아이폰, <b>안드로이드</b> 스마트폰 HDMI TV연결)",
            "link": "https://search.shopping.naver.com/gate.nhn?id=10782444869",
            "image": "https://shopping-phinf.pstatic.net/main_1078244/10782444869.5.jpg",
            "lprice": "16500",
            "hprice": "0",
            "mallName": "투데이샵",
            "productId": "10782444869",
            "productType": "2"
        },
        {
            "title": "파인디지털 파인드라이브 Q300",
            "link": "https://search.shopping.naver.com/gate.nhn?id=19490416717",
            "image": "https://shopping-phinf.pstatic.net/main_1949041/19490416717.20190527115824.jpg",
            "lprice": "227050",
            "hprice": "359000",
            "mallName": "네이버",
            "productId": "19490416717",
            "productType": "1"
        },
        {
            "title": "주파집 USB NEW 마그네틱 5핀 <b>안드로이드</b> 자석 고속충전케이블",
            "link": "https://search.shopping.naver.com/gate.nhn?id=16222651410",
            "image": "https://shopping-phinf.pstatic.net/main_1622265/16222651410.20181120154423.jpg",
            "lprice": "6500",
            "hprice": "11900",
            "mallName": "네이버",
            "productId": "16222651410",
            "productType": "1"
        },
        {
            "title": "MHL 케이블 아이폰 <b>안드로이드</b> HDMI 미러링",
            "link": "https://search.shopping.naver.com/gate.nhn?id=11859583359",
            "image": "https://shopping-phinf.pstatic.net/main_1185958/11859583359.1.jpg",
            "lprice": "12500",
            "hprice": "0",
            "mallName": "가가넷",
            "productId": "11859583359",
            "productType": "2"
        },
        {
            "title": "아이폰삼각대 / ios&amp;<b>Android</b> 호환 가능",
            "link": "https://search.shopping.naver.com/gate.nhn?id=16341221561",
            "image": "https://shopping-phinf.pstatic.net/main_1634122/16341221561.4.jpg",
            "lprice": "31900",
            "hprice": "0",
            "mallName": "포시즌몰",
            "productId": "16341221561",
            "productType": "2"
        },
        {
            "title": "뷰잉 (viewing)",
            "link": "https://search.shopping.naver.com/gate.nhn?id=13030462232",
            "image": "https://shopping-phinf.pstatic.net/main_1303046/13030462232.20190306144248.jpg",
            "lprice": "87110",
            "hprice": "180000",
            "mallName": "네이버",
            "productId": "13030462232",
            "productType": "1"
        },
        {
            "title": "샤오미 Mi Box (TELEBEE)",
            "link": "https://search.shopping.naver.com/gate.nhn?id=12302122742",
            "image": "https://shopping-phinf.pstatic.net/main_1230212/12302122742.20170920112004.jpg",
            "lprice": "54900",
            "hprice": "99000",
            "mallName": "네이버",
            "productId": "12302122742",
            "productType": "1"
        },
        {
            "title": "MHL 케이블 아이폰 <b>안드로이드</b> HDMI 미러링 TV연결",
            "link": "https://search.shopping.naver.com/gate.nhn?id=8678305242",
            "image": "https://shopping-phinf.pstatic.net/main_8678305/8678305242.5.jpg",
            "lprice": "5500",
            "hprice": "0",
            "mallName": "글로벌텐교",
            "productId": "8678305242",
            "productType": "2"
        },
        {
            "title": "파인디지털 파인드라이브 Q30",
            "link": "https://search.shopping.naver.com/gate.nhn?id=18711239261",
            "image": "https://shopping-phinf.pstatic.net/main_1871123/18711239261.20190415105108.jpg",
            "lprice": "199000",
            "hprice": "315640",
            "mallName": "네이버",
            "productId": "18711239261",
            "productType": "1"
        },
        {
            "title": "이노아이오 스마트빔 3",
            "link": "https://search.shopping.naver.com/gate.nhn?id=14032821135",
            "image": "https://shopping-phinf.pstatic.net/main_1403282/14032821135.20180413144450.jpg",
            "lprice": "259870",
            "hprice": "387000",
            "mallName": "네이버",
            "productId": "14032821135",
            "productType": "1"
        }
    ]
}
"""

json_data = json.loads(data)
print (json_data['items'][0]['title'])
print (json_data['items'][0]['link'])

```

    MHL 케이블 (아이폰, <b>안드로이드</b> 스마트폰 HDMI TV연결)
    https://search.shopping.naver.com/gate.nhn?id=10782444869


### 네이버 검색 Open API를 이용한 크롤링 초간단 실습
 - https://developers.naver.com/main/

- postman 설치 (https://www.getpostman.com/downloads/) 

- 사용법
   1. Sign Up in Postman
   
   2. Insert https://openapi.naver.com/v1/search/news.json?query=스마트 into GET
   
   3. Add X-Naver-Client-Id(key), <font color="blue">CsODwdUTyG9vOI1uIeIf</font>(value) into Headers
   
   4. Add X-Naver-Client-Secret(key), <font color="blue">YmIx0GW8JG</font>(value) into Headers
   
   5. Send
   
     ![postman](/images/2023-05-28-openapicrawling/postman.png)
   
- [참고: 네이버 Open API HTTP 응답 상태 에러 코드 목록1](https://developers.naver.com/docs/common/openapiguide/#/errorcode.md)

- [참고: 일반적인 HTTP 응답 상태 코드](http://ooz.co.kr/260) 

### 네이버 Open API 사용하기


```python
import requests
import pprint

client_id = '아이디'
client_secret = '비번'

naver_open_api = 'https://openapi.naver.com/v1/search/shop.json?query=갤럭시워치'
header_params = {"X-Naver-Client-Id":client_id, "X-Naver-Client-Secret":client_secret}
res = requests.get(naver_open_api, headers=header_params)

if res.status_code == 200:
    data = res.json()
    for index, item in enumerate(data['items']):
        print(index+1, "-" ,item['title'], item['link'])
else: 
    print("Error Code:", res.status.code)
```

    1 - 삼성전자 <b>갤럭시 워치</b>5 44mm 알루미늄 https://search.shopping.naver.com/gate.nhn?id=34117898619
    2 - 삼성전자 <b>갤럭시워치</b>4 클래식 46mm SM-R890N  (블루투스) https://search.shopping.naver.com/gate.nhn?id=28669924554
    3 - 삼성전자 <b>갤럭시 워치</b>5 40mm 알루미늄 WIFI https://search.shopping.naver.com/gate.nhn?id=34117828618
    4 - 삼성전자 <b>갤럭시 워치</b>5 프로 45mm 티타늄 https://search.shopping.naver.com/gate.nhn?id=34117898620
    5 - 삼성전자 <b>갤럭시 워치</b>5 44mm 골프에디션 https://search.shopping.naver.com/gate.nhn?id=34118826619
    6 - 삼성전자 <b>갤럭시워치</b>4 44mm SM-R870N (블루투스) https://search.shopping.naver.com/gate.nhn?id=28669873555
    7 - 삼성전자 <b>갤럭시 워치</b>5 프로 45mm 골프에디션 https://search.shopping.naver.com/gate.nhn?id=34120036618
    8 - 삼성전자 <b>갤럭시워치</b>4 클래식 42mm SM-R880N (블루투스) https://search.shopping.naver.com/gate.nhn?id=28669889554
    9 - 삼성전자 삼성 <b>갤럭시 워치</b>5 40mm 블루투스 + 보호필름 + 스트랩 https://search.shopping.naver.com/gate.nhn?id=36245968618
    10 - 삼성전자 <b>갤럭시 워치</b>4 골프 에디션 40mm https://search.shopping.naver.com/gate.nhn?id=29748903620

### 크롤링 후 엑셀파일로 저장하기

```python
import requests
import openpyxl

client_id = '아이디'
client_secret = '비번'
start, num = 1, 0

excel_file = openpyxl.Workbook()
excel_sheet = excel_file.active
excel_sheet.column_dimensions['B'].width = 100
excel_sheet.column_dimensions['C'].width = 100
excel_sheet.append(['랭킹', '제목', '링크'])

for index in range(10):
    start_number = start + (index * 100)
    naver_open_api = 'https://openapi.naver.com/v1/search/shop.json?query=아이폰&display=100&start=' + str(start_number)
    header_params = {"X-Naver-Client-Id":client_id, "X-Naver-Client-Secret":client_secret}
    res = requests.get(naver_open_api, headers=header_params)
    if res.status_code == 200:
        data = res.json()
        for item in data['items']:
            num += 1
            excel_sheet.append([num, item['title'], item['link']])
    else:
        print ("Error Code:", res.status_code)

excel_file.save('IT.xlsx')
excel_file.close()
```



