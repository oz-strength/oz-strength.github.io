---
layout: single
title: "open API 크롤링2(정부데이터)"
categories: python
tag: [crawling, openAPI, jupyternotebook]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---



## 다양한 Open API 사용하기

### 정부3.0 공공 데이터 포털 API 사용하기

* 공공 데이터 포털 가입하기
  - https://www.data.go.kr
  - 회원가입 -> 로그인 -> '한국환경공단_대기오염정보' 으로 검색 후, 해당 Open API 페이지로 이동
     > 공공 데이터 포털은 공인된 기관임에도 아쉽게도, 메뉴와 Open API 가 링크 등이 수시로 바뀌고 있습니다. 
  - 해당 API 에서 활용신청을 통해 인증키를 발급받은 후, 샘플코드 또는 페이지에 링크되어 있는 문서 또는 가이드를 기반으로 테스트 진행
  
   <img src="https://www.fun-coding.org/00_Images/governapi.png" />  
  

- JSON 이외에 XML 포멧으로 데이터를 다루는 경우도 많음
  - XML 관련 내용은 '다양한 데이터 읽기 - XML 파일' 참조


```python
import requests

service_key = '서비스키'
params = '&numOfRows=100&sidoName=서울&pageNo=1'
open_api = 'http://apis.data.go.kr/B552584/ArpltnInforInqireSvc/getCtprvnRltmMesureDnsty?serviceKey=' + service_key + params

res = requests.get(open_api)

print(res.text)
```

    <?xml version="1.0" encoding="UTF-8"?>
    <response>
      <header>
        <resultCode>00</resultCode>
        <resultMsg>NORMAL_CODE</resultMsg>
      </header>
      <body>
        <items>
          <item>
            <so2Grade>1</so2Grade>
            <coFlag/>
            <khaiValue>47</khaiValue>
            <so2Value>0.003</so2Value>
            <coValue>0.4</coValue>
            <pm10Flag/>
            <o3Grade>1</o3Grade>
            <pm10Value>21</pm10Value>
            <khaiGrade>1</khaiGrade>
            <sidoName>서울</sidoName>
            <no2Flag/>
            <no2Grade>1</no2Grade>
            <o3Flag/>
            <so2Flag/>
            <dataTime>2023-05-31 10:00</dataTime>
            <coGrade>1</coGrade>
            <no2Value>0.026</no2Value>
            <stationName>중구</stationName>
            <pm10Grade>1</pm10Grade>
            <o3Value>0.028</o3Value>
          </item>
          <item>
            <so2Grade>1</so2Grade>
            <coFlag/>
            <khaiValue>54</khaiValue>
            <so2Value>0.003</so2Value>
            <coValue>0.4</coValue>
            <pm10Flag/>
            <o3Grade>1</o3Grade>
            <pm10Value>48</pm10Value>
            <khaiGrade>2</khaiGrade>
            <sidoName>서울</sidoName>
            <no2Flag/>
            <no2Grade>1</no2Grade>
            <o3Flag/>
            <so2Flag/>
            <dataTime>2023-05-31 10:00</dataTime>
            <coGrade>1</coGrade>
            <no2Value>0.021</no2Value>
            <stationName>한강대로</stationName>
            <pm10Grade>2</pm10Grade>
            <o3Value>0.029</o3Value>
          </item>
          <item>
            <so2Grade>1</so2Grade>
            <coFlag/>
            <khaiValue>54</khaiValue>
            <so2Value>0.003</so2Value>
            <coValue>0.4</coValue>
            <pm10Flag/>
            <o3Grade>2</o3Grade>
            <pm10Value>19</pm10Value>
            <khaiGrade>2</khaiGrade>
            <sidoName>서울</sidoName>
            <no2Flag/>
            <no2Grade>1</no2Grade>
            <o3Flag/>
            <so2Flag/>
            <dataTime>2023-05-31 10:00</dataTime>
            <coGrade>1</coGrade>
            <no2Value>0.016</no2Value>
            <stationName>종로구</stationName>
            <pm10Grade>1</pm10Grade>
            <o3Value>0.034</o3Value>
          </item>
          <item>
            <so2Grade>1</so2Grade>
            <coFlag/>
            <khaiValue>51</khaiValue>
            <so2Value>0.005</so2Value>
            <coValue>0.5</coValue>
            <pm10Flag/>
            <o3Grade>1</o3Grade>
            <pm10Value>28</pm10Value>
            <khaiGrade>2</khaiGrade>
            <sidoName>서울</sidoName>
            <no2Flag/>
            <no2Grade>1</no2Grade>
            <o3Flag/>
            <so2Flag/>
            <dataTime>2023-05-31 10:00</dataTime>
            <coGrade>1</coGrade>
            <no2Value>0.021</no2Value>
            <stationName>청계천로</stationName>
            <pm10Grade>2</pm10Grade>
            <o3Value>0.025</o3Value>
          </item>
          <item>
            <so2Grade>1</so2Grade>
            <coFlag/>
            <khaiValue>57</khaiValue>
            <so2Value>0.004</so2Value>
            <coValue>0.6</coValue>
            <pm10Flag/>
            <o3Grade>1</o3Grade>
            <pm10Value>26</pm10Value>
            <khaiGrade>2</khaiGrade>
            <sidoName>서울</sidoName>
            <no2Flag/>
            <no2Grade>2</no2Grade>
            <o3Flag/>
            <so2Flag/>
            <dataTime>2023-05-31 10:00</dataTime>
            <coGrade>1</coGrade>
            <no2Value>0.034</no2Value>
            <stationName>종로</stationName>
            <pm10Grade>1</pm10Grade>
            <o3Value>0.021</o3Value>
          </item>
       ...
        </items>
        <numOfRows>100</numOfRows>
        <pageNo>1</pageNo>
        <totalCount>40</totalCount>
      </body>
    </response>


### 공공데이터포털 Open API 예제
- https://www.data.go.kr/data/15000581/openapi.do


```python
import requests
from bs4 import BeautifulSoup

service_key = 'OeambYOBGAP4dLuMOK%2FeSe93%2FxcMh2j%2FPWKtR4U8kTXsKPdBThyoCSWrdQxIz3LNYyupCHjiAzePdfyVDAvA9Q%3D%3D'
params = '&numOfRows=100&sidoName=서울&pageNo=1'
open_api = 'http://apis.data.go.kr/B552584/ArpltnInforInqireSvc/getCtprvnRltmMesureDnsty?serviceKey=' + service_key + params

res = requests.get(open_api)
soup = BeautifulSoup(res.content, "html.parser")

data = soup.find_all("item")
for item in data:
    stationname = item.find("stationname")
    pm10grade = item.find('pm10grade')
    print(stationname.get_text(), pm10grade.get_text())
```

    중구 1
    한강대로 2
    종로구 1
    청계천로 1
    종로 1
    용산구 1
    광진구 1
    성동구 1
    강변북로 1
    중랑구 1
    동대문구 1
    홍릉로 1
    성북구 1
    정릉로 1
    도봉구 1
    은평구 1
    서대문구 1
    마포구 1
    신촌로 1
    강서구 1
    공항대로 1
    구로구 1
    영등포구 1
    영등포로 1
    동작구 1
    동작대로 중앙차로 1
    관악구 2
    강남구 1
    서초구 1
    도산대로 1
    강남대로 1
    송파구 1
    강동구 1
    천호대로 1
    금천구 1
    시흥대로 2
    강북구 1
    양천구 1
    노원구 2
    화랑로 2


## 다양한 데이터 읽기 - XML

* XML(Extensible Markup Language)
  - 특정 목적에 따라 데이터를 태그로 감싸서 마크업하는 범용적인 포멧
  - 마크업 언어는 태그 등을 이용하여 데이터의 구조를 기술하는 언어의 한 가지
  - 가장 친숙한 마크업 언어가 HTML
  - XML은 HTML과 마찬가지로 데이터를 계층 구조로 표현
  - XML 기본 구조
  <pre>
  <태그 속성="속성값">내용</태그>
  </pre>

    - 태그와 속성은 특정 목적에 따라 임의로 이름을 정해서 사용
  <product id="M001" price="300000">32인치 LCD 모니터</product>
    - 다른 요소와 그룹으로 묶을 수도 있음
  <products type="전자제품?>
      <product id="M001" price="300000">32인치 LCD 모니터</product>
      <product id="M002" price="210000">24인치 LCD 모니터</product>
  </products>
<div class="alert alert-block" style="border: 2px solid #E65100;background-color:#FFF3E0;padding:10px">
<font size="4em" style="font-weight:bold;color:#BF360C;">공공데이터포털 Open API 변경 관련 (2022년 업데이트)</font><br><br>
<font size="3em" style="color:#BF360C;">공공데이터포털은 정부 관련 기관으로, 공식적으로 Open API 를 제공합니다</font><br>
<font size="3em" style="color:#BF360C;">하지만, 예상치 못하게, 관련 Open API 가 완전히 주소가 바뀌었습니다</font><br>
<font size="3em" style="color:#BF360C;">이에 다음 코드와 같이 테스트가 가능하여, 코드를 업데이트하였습니다</font><br>
<font size="3em" style="color:#BF360C;">관련 코드는 https://www.data.go.kr/data/15073861/openapi.do 에서 변경된 가이드( (4) 대기질 예보통보 조회 상세기능명세 ) 그대로 따른 것이며, 상세 정보도 해당 사이트에서 확인이 가능합니다</font><br>
<br>
</div>

<div class="alert alert-block" style="border: 2px solid #E65100;background-color:#FFF3E0;padding:10px">
<font size="3em" style="color:#BF360C;">Open API 는 향후에도 데이터를 기술적으로 공유하는 가장 일반적인 방법이 될 수 있습니다<br> 그래서, 본 강의를 통해 이런 부분도 꼭 경험하시고, 또 찾고자 하시는 데이터가 있으면, 관련 Open API 를 검색하고, <br> 각 Open API 마다 사용법을 가이드하고 있으므로, 이를 이해하고 테스트하실 수 있는 역량도 갖추시는데 도움이 되었으면 좋겠습니다</font><br>
</div>


```python
import requests
from bs4 import BeautifulSoup

service_key = '자신이 발급받은 서비스키를 넣으시면 됩니다'
params = '&numOfRows=10&pageNo=1'
open_api = 'http://apis.data.go.kr/B552584/ArpltnInforInqireSvc/getMinuDustFrcstDspth?ServiceKey=' + service_key + params

res = requests.get(open_api)
soup = BeautifulSoup(res.content, 'html.parser')

data = soup.find_all('item')
for item in data:
    datatime = item.find('datatime')
    pm10grade = item.find('informcode')
    print (datatime.get_text(), pm10grade.get_text())
```
