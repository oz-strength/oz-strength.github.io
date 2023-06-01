---
layout: single
title: "문자열 관련 함수"
categories: python
tag: [crawling, ecommerce, jupyternotebook]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

## Crawling & Crawing

### 1. 실전 예제: 크롤링
* 이커머스(지마켓) 베스트100 상품 타이틀/가격 추출하기


```python
import requests 
from bs4 import BeautifulSoup

res = requests.get("https://www.gmarket.co.kr/n/best?viewType=G&groupCode=G06")
soup = BeautifulSoup(res.content, "html.parser")

bestitems = soup.select_one("div.best-list")
products = bestitems.select('ul > li')

for index,product in enumerate(products):
    title = product.select_one('a.itemname')
    price = product.select_one("div.s-price > strong")
    
    res_info = requests.get(title["href"])
    soup_info = BeautifulSoup(res_info.content, 'html.parser')
    print(index+1, title.get_text(), price.get_text(), title['href'])
```

    1 [ASUS]긴급추가수량확보 ASUS ROG ALLY RC71L-NH001W 휴대용 게임기 Z1 Extreme/16GB/512GB/500Nits/Touch/UMPC 999,000원 http://item.gmarket.co.kr/Item?goodscode=2961459160&ver=20230531
    2 [대우](혜택가3.5만원) 2023년형 대우 3단 높이조절 리모컨 34cm형 써큘레이터 선풍기 / DEF-KC3040 서큘레이터 39,900원 http://item.gmarket.co.kr/Item?goodscode=2865607786&ver=20230531
    3 [유파]내일도착유파 선풍기리모컨초미풍발터치 TSK-3528CR 32,180원 http://item.gmarket.co.kr/Item?goodscode=2177829053&ver=20230531
    4 [대우](혜택가 2.9만원) 2023년형 대우 에어 써큘레이터 DEF-KC1020 스탠드 선풍기 공기순환 서큘레이터 33,300원 http://item.gmarket.co.kr/Item?goodscode=1833787752&ver=20230531
    5 [LG전자]혜택가 45만원대 LG전자 휘센 제습기 20L 블루 DQ202PBBC 489,000원 http://item.gmarket.co.kr/Item?goodscode=2428196089&ver=20230531
    6 [다이슨]다이슨 에어랩 멀티스타일러 롱 (니켈/코퍼) (10% 쿠폰할인) 749,000원 http://item.gmarket.co.kr/Item?goodscode=2841728540&ver=20230531
    7 [솔러스에어]욕실 화장실 천장형 캠핑용 선풍기 타프 실링팬 AIR701CF 28,900원 http://item.gmarket.co.kr/Item?goodscode=2892450550&ver=20230531
    8 [위닉스]위닉스 뽀송 제습기 17L DN3E170-LWK /RT 에너지소비효율1등급 제조사정품 422,800원 http://item.gmarket.co.kr/Item?goodscode=2514306486&ver=20230531
    9 [위닉스]공식인증점) 위닉스 제습기 12L {DXTE120-MPK} (~5/31 카드가 21.9만) 235,480원 http://item.gmarket.co.kr/Item?goodscode=2861939687&ver=20230531
    10 [신일]23년 신상] 신일 BLDC 서큘레이터 air S9 1set(SIF-CS30BG/SIF-CS30DG/SIF-CS30BL) 137,760원 http://item.gmarket.co.kr/Item?goodscode=2947091062&ver=20230531
    11 [위니아]전국지역/기본설치포함)인버터 벽걸이에어컨 ERV06GHP + 471,000원 http://item.gmarket.co.kr/Item?goodscode=2401170088&ver=20230531
    12 [한경희생활과학]한경희 저소음 13L 제습기 HE-D707 / 예약판매 6월 12일 발송 189,000원 http://item.gmarket.co.kr/Item?goodscode=2098489923&ver=20230531
    13 [LG전자]LG전자 2023 울트라PC 15UD40Q-GX5DK 혜택가 49만 가성비 라이젠 노트북 669,000원 http://item.gmarket.co.kr/Item?goodscode=2918818023&ver=20230531
    14 [윈드피아](최종2.6만)가정용 업소용 사무실 스탠드 키높이 선풍기 WA-170 28,600원 http://item.gmarket.co.kr/Item?goodscode=1298066886&ver=20230531
    15 [쿠쿠](혜택가 22.8만원) 쿠쿠 본사직영 CRP-CHP1010FD 10인용 IH전기압력 밥솥 260,110원 http://item.gmarket.co.kr/Item?goodscode=1689086718&ver=20230531
    16 [까미또]갤럭시S23 S22 S21 S20 플러스 5G 노트20울트라 A53 A23 A52 A32 아이폰14 13 12 프로 맥스 핸드폰 케이스 6,900원 http://item.gmarket.co.kr/Item?goodscode=225532316&ver=20230531
    17 [마이크론]마이크론 2300 M.2 nvMe Gen3 512GB 38,000원 http://item.gmarket.co.kr/Item?goodscode=2945769074&ver=20230531
    18 [르젠]내일도착르젠 앱연동 BLDC 선풍기 LZEF-DC180 화이트 76,800원 http://item.gmarket.co.kr/Item?goodscode=1859564938&ver=20230531
    19 [대웅]대웅 가정용 스탠드선풍기  키높이선풍기 29,540원 http://item.gmarket.co.kr/Item?goodscode=1783779831&ver=20230531
    20 [신일전자]2023년 NEW  BLDC 서큘레이터 air S9 (베이지/딥그린/블루) 142,800원 http://item.gmarket.co.kr/Item?goodscode=2948035919&ver=20230531
    21 [윈드피아](최종3만)가정용 업소용 스탠드 리모컨 선풍기 인기상품 1700R 39,900원 http://item.gmarket.co.kr/Item?goodscode=730730680&ver=20230531
    22 [이노크아든]이노크 저소음 스탠드선풍기 14형 특가 IA-FBI7942 28,700원 http://item.gmarket.co.kr/Item?goodscode=1428376659&ver=20230531
    23 [닌텐도]젤다의 전설 티어스 오브 더 킹덤 (한글판) I 81,500원 http://item.gmarket.co.kr/Item?goodscode=2935472887&ver=20230531
    24 [대우](혜택가 2.9만원) 2023년형 대우 3단 높이조절 34cm형 써큘레이터 선풍기 / DEF-KC2030 서큘레이터 33,300원 http://item.gmarket.co.kr/Item?goodscode=2868268543&ver=20230531
    25 [테팔]테팔 믹서기 블렌더 블렌드포스 플러스 BL4258KR BL4258 블랙 46,410원 http://item.gmarket.co.kr/Item?goodscode=2310724665&ver=20230531
    26 [르젠]르젠 스탠드형 리모컨 써큘레이터 선풍기 LZEF-AR03 51,900원 http://item.gmarket.co.kr/Item?goodscode=2554812512&ver=20230531
    27 [쿠쿠]쿠쿠 국내정품 6인용 IH트윈프레셔 밥솥 CRP-LHTR0610FW 309,860원 http://item.gmarket.co.kr/Item?goodscode=2810660898&ver=20230531
    28 [신일전자]신일 스탠드 선풍기 저소음 12인치 SIF-12PNX 59,900원 http://item.gmarket.co.kr/Item?goodscode=1324935106&ver=20230531
    29 [코슬리]코슬리 핸디 스팀다리미 유니크 아메리카노 기프티콘 증정 36,700원 http://item.gmarket.co.kr/Item?goodscode=2005499762&ver=20230531
    30 [대우](혜택가 3.5만원) 2023년형 대우 2단 높이조절 리모컨 40cm형 써큘레이터 선풍기 / DEF-NS1400 서큘레이터 39,900원 http://item.gmarket.co.kr/Item?goodscode=2950107808&ver=20230531
    31 [쿠쿠]쿠쿠 정품인증 10인용 풀스테인리스 IH트윈프레셔 밥솥 CRP-KHTS1060FD 277,550원 http://item.gmarket.co.kr/Item?goodscode=2861619916&ver=20230531
    32 [솔러스에어]솔러스에어 탁상용 선풍기 타이머 좌우회전 수면풍 자연풍 무선 무소음 미니 탁상형 소형 충전식 AIR604TF 34,800원 http://item.gmarket.co.kr/Item?goodscode=2877456477&ver=20230531
    33 [유닉스]유닉스 전문가용 매직기 미용실 고데기 볼륨 스트레이트너 세라믹 앞머리 고대기 판고데기 추천 29,700원 http://item.gmarket.co.kr/Item?goodscode=2606062725&ver=20230531
    34 [윈드피아](최종2.3만)가정용선풍기 업소용선풍기 스탠드 선풍기 WA-370 27,900원 http://item.gmarket.co.kr/Item?goodscode=1382089049&ver=20230531
    35 [레드울프]갤럭시 S23 S22 울트라 노트20/노트10/S21/S20/S10/A53/A32/A23/A33/A71/A82 플러스 가죽 지갑형 폰케이스 19,900원 http://item.gmarket.co.kr/Item?goodscode=268988080&ver=20230531
    36 [OLLY(가전)]올리 멀티 에어프라이어 그릴 OLAG09B 에어프라이기 스테이크 그릴 89,000원 http://item.gmarket.co.kr/Item?goodscode=2839853819&ver=20230531
    37 [삼성전자]삼성 무풍에어컨 벽걸이 슬림(AR07C9150HZT) 719,200원 http://item.gmarket.co.kr/Item?goodscode=2958178866&ver=20230531
    38 [르젠]르젠 앱연동 스마트 BLDC 리모컨 선풍기 LZEF-DC02 78,500원 http://item.gmarket.co.kr/Item?goodscode=2843298236&ver=20230531
    39 [보만]카드혜택가 3.6만)32만개 판매 국민 스팀다리미 보만 핸디형 가정용 휴대용 여행용 의류스팀기 DB8231 42,700원 http://item.gmarket.co.kr/Item?goodscode=1224898468&ver=20230531
    40 [삼성전자]갤럭시 워치4 44mm / SM-R870N 최종가 169000원 179,000원 http://item.gmarket.co.kr/Item?goodscode=2248339963&ver=20230531
    41 [캐리어]1등급 클라윈드 20L 제습기 CDHM-C020LLPL 299,000원 http://item.gmarket.co.kr/Item?goodscode=2413161576&ver=20230531
    42 [신일전자][신일] 2022년형 BLDC air S8 써큘레이터 (베이지/딥그린/라이트핑크) 118,400원 http://item.gmarket.co.kr/Item?goodscode=2451351419&ver=20230531
    43 토트넘 훗스퍼 스마트워치 TOT-SW231 (2종 택1) 129,000원 http://item.gmarket.co.kr/Item?goodscode=2962116010&ver=20230531
    44 [캐리어]6평형 인버터 벽걸이 에어컨 ORCD061FAWWSD (전국_기본설치비 포함)(추천) 479,790원 http://item.gmarket.co.kr/Item?goodscode=2424340425&ver=20230531
    45 [피스넷]골전도 블루투스 이어폰 피스넷 프리본 시즌2 / 2023년 5월 신제품 49,900원 http://item.gmarket.co.kr/Item?goodscode=1656073805&ver=20230531
    46 [쿠쿠](혜택가 5.9만원) 쿠쿠 본사직영 20L 전자레인지 CMW-A201DW 67,510원 http://item.gmarket.co.kr/Item?goodscode=2839783463&ver=20230531
    47 [쿠쿠](혜택가 20.9만원) 쿠쿠 본사직영 CRP-DHP0610FD 6인용 IH전기압력 밥솥 238,510원 http://item.gmarket.co.kr/Item?goodscode=1692190118&ver=20230531
    48 [ASUS]힐링쉴드 강화유리 필름 ROG ALLY 전용 14,800원 http://item.gmarket.co.kr/Item?goodscode=2971758521&ver=20230531
    49 [테팔]테팔 스팀다리미 무선다리미 프리무브 에어 FV6550 83,610원 http://item.gmarket.co.kr/Item?goodscode=2330377014&ver=20230531
    50 [다이슨]다이슨 에어랩 멀티 스타일러 컴플리트 롱 (코퍼/니켈) 749,000원 http://item.gmarket.co.kr/Item?goodscode=2935603979&ver=20230531
    51 [샤오미]샤오미 미지아 무선 선풍기 5세대 에어서큘레이터 BLDC 마그네틱 충전 돼지코 증정 BPLDS05DM 113,300원 http://item.gmarket.co.kr/Item?goodscode=2887042390&ver=20230531
    52 [위닉스]공식파트너_위닉스 뽀송 제습기 16L (DN2H160-IWK) 집중건조킷 포함 341,490원 http://item.gmarket.co.kr/Item?goodscode=2879470884&ver=20230531
    53 [위닉스]공식인증점)위닉스 컴팩트 미니건조기 플러스 {HS2E400-MGK} 오가닉그린 (최대 4KG) (~5/31 카드가 29.9만) 331,500원 http://item.gmarket.co.kr/Item?goodscode=2548106026&ver=20230531
    54 [신일전자]신일 BLDC 무소음 리모컨 선풍기 SIF-DC826 123,000원 http://item.gmarket.co.kr/Item?goodscode=2841795103&ver=20230531
    55 [캐리어]캐리어 10L 제습기 CDHM-C010LMOW 22년형 신제품 199,000원 http://item.gmarket.co.kr/Item?goodscode=2441792558&ver=20230531
    56 갤럭시 S23 S22 울트라 노트20/노트10/S21/S20/S10/A53/A32/A23/A33 플러스 핸드폰 휴대폰 가죽케이스 19,900원 http://item.gmarket.co.kr/Item?goodscode=826268928&ver=20230531
    57 [LG전자]LG전자 DQ132PWXC 지역별운송료상이 (가인) 398,920원 http://item.gmarket.co.kr/Item?goodscode=2494906253&ver=20230531
    58 [네고]2매+1 (이벤트참여) 강화유리 지문방지 갤럭시S23 S22 울트라 플러스 노트 플립 폴드 아이폰14 13 프로맥스 14,900원 http://item.gmarket.co.kr/Item?goodscode=637253436&ver=20230531
    59 [삼성전자]삼성 비스포크 무풍에어컨 윈도우핏 베이지 AW06C7155TWA 969,000원 http://item.gmarket.co.kr/Item?goodscode=2958178600&ver=20230531
    60 [필립스]최종13910원)PHILIPS 필립스 코털제거기 NT1620/15 콧털 정리 트리머 13,410원 http://item.gmarket.co.kr/Item?goodscode=1894866019&ver=20230531
    61 [대우]대우 써큘레이터형 BLDC 에코팬 리모컨 선풍기 73,800원 http://item.gmarket.co.kr/Item?goodscode=1863410114&ver=20230531
    62 [엔보우]모노 탁상용 무선 휴대용 선풍기 2세대 1+1(할인 행사) 29,900원 http://item.gmarket.co.kr/Item?goodscode=1812271382&ver=20230531
    63 [신일]신일 BLDC 에어서큘레이터 AIR S9 베이지 2세트(SIF-TS10RA) 255,960원 http://item.gmarket.co.kr/Item?goodscode=2962153901&ver=20230531
    64 [신일전자]프리미엄 선풍기 SIF-T14DC - 14인치 / 상하.좌우 3D입체회전 BLDC / 무소음 써큘레이터 159,000원 http://item.gmarket.co.kr/Item?goodscode=2893946602&ver=20230531
    65 [유닉스]유닉스 미니 웨이브 볼륨 뽕 멀티 고데기 UCI-B2304 7,900원 http://item.gmarket.co.kr/Item?goodscode=2610076235&ver=20230531
    66 [아이엔픽]핸드폰 갤럭시S23울트라 S22플러스 S21 S20 S10 노트20 노트10 노트9 노트8 A34 A53 A23 A32 A31 가죽 지갑 9,900원 http://item.gmarket.co.kr/Item?goodscode=1418909600&ver=20230531
    67 [LG전자]DQ202PSUA o클릭o LG전자/제습기 505,490원 http://item.gmarket.co.kr/Item?goodscode=2454689996&ver=20230531
    68 [닌텐도]닌텐도 스위치 젤다의 전설 티어스 오브 더 킹덤 일반판 +특전 74,800원 http://item.gmarket.co.kr/Item?goodscode=2847648865&ver=20230531
    69 [샤클라]샤클라 로봇 물걸레청소기 KGC001 199,000원 http://item.gmarket.co.kr/Item?goodscode=2225642928&ver=20230531
    70 [삼성전자]{신규런칭} NEW 갤럭시 A34 자급제 128GB SM-A346N/쿠폰(5%)추가할인/무이자(최대12개월)/당일발송 464,440원 http://item.gmarket.co.kr/Item?goodscode=2861319917&ver=20230531
    71 [위니아]인증 위니아 가정용제습기 EDH8DNWB 8L 182,000원 http://item.gmarket.co.kr/Item?goodscode=1817247637&ver=20230531
    72 [위닉스]공식인증점) 위닉스 NEW 타워엣지 공기청정기 {AT8E430-MWK} 화이트 224,730원 http://item.gmarket.co.kr/Item?goodscode=2882340090&ver=20230531
    73 [위닉스]{공식파트너} 위닉스 뽀송 제습기 10리터 DXAE100-JWK 215,690원 http://item.gmarket.co.kr/Item?goodscode=510161514&ver=20230531
    74 [제이엠더블유]JMW 터보 항공기 MG1800 PLUS 올화이트 59,000원 http://item.gmarket.co.kr/Item?goodscode=1564671373&ver=20230531
    75 [LG전자]LG전자 27ART10AKPL 스탠드형 스탠바이미 TV (H) 978,000원 http://item.gmarket.co.kr/Item?goodscode=2927118819&ver=20230531
    76 [솔러스에어]탁상용 소형 미니 무소음 무선 휴대용 탁상 선풍기 AIR601TF 29,900원 http://item.gmarket.co.kr/Item?goodscode=2839674262&ver=20230531
    77 [미홀]샤오미 MIWHOLE 20L 제습기 / 미홀 제습기 / 무료배송 124,000원 http://item.gmarket.co.kr/Item?goodscode=2544070928&ver=20230531
    78 [샤오미]샤오미 미지아 LED 모니터 조명 1s MJGJD02YL 스크린바 무선리모컨 45,900원 http://item.gmarket.co.kr/Item?goodscode=2866332287&ver=20230531
    79 모닝컴 리모컨 초미세풍 발터치스탠드선풍기 23년도 신상품 32,900원 http://item.gmarket.co.kr/Item?goodscode=2923134780&ver=20230531
    80 [유닉스]20만개판매 유닉스 전문가용 드라이기 헤어드라이어 미용실 가정용 업소용 냉풍 찬바람 가성비 UN-A1770 41,940원 http://item.gmarket.co.kr/Item?goodscode=978590816&ver=20230531
    81 [필립스]카드가 12만원대) PHILIPS 전기면도기 SkinIQ 5000 S5582/36 오션블루 122,550원 http://item.gmarket.co.kr/Item?goodscode=2065305475&ver=20230531
    82 [삼성전자]삼성전자 정품 마이크로SD EVO Plus 512GB MB-MC512KA 33,960원 http://item.gmarket.co.kr/Item?goodscode=2258073651&ver=20230531
    83 [한경희이지라이프]한경희 제습기 HE-D720 대용량 20L (최종혜택가 21.5만 5월 신제품) /예약판매 249,000원 http://item.gmarket.co.kr/Item?goodscode=2923177563&ver=20230531
    84 [쿠쿠](혜택가 17.6만원) 쿠쿠 본사직영 10인용 IH압력 밥솥 CRP-HWF1060FDM 206,110원 http://item.gmarket.co.kr/Item?goodscode=1624106044&ver=20230531
    85 [캐리어]총알배송 전국무료배송 캐리어 고효율 1등급 20리터 제습기 386,130원 http://item.gmarket.co.kr/Item?goodscode=2880038660&ver=20230531
    86 [씽크웨이]씽크에어 DL12 제습기 138,570원 http://item.gmarket.co.kr/Item?goodscode=2151848191&ver=20230531
    87 핸드폰/갤럭시/S23/S22/노트20/S21/S20/노트10/플러스/노트9/8/A34/키즈/A32/울트라/점프/아이폰14프로맥스 9,800원 http://item.gmarket.co.kr/Item?goodscode=208542015&ver=20230531
    88 [신일전자]신일 에어써큘레이터 화이트 SIF-FA800B 103,600원 http://item.gmarket.co.kr/Item?goodscode=2955811170&ver=20230531
    89 [위닉스]공식인증점)위닉스 컴팩트 미니건조기 플러스 {HS2E400-MEK} 화이트베이지 (최대 4KG) (~5/31 카드 29.9만) 321,500원 http://item.gmarket.co.kr/Item?goodscode=2548097351&ver=20230531
    90 [쿠쿠]6인용 쿠쿠 고화력 밥솥 CRP-P0660FD+냉동용기 147,250원 http://item.gmarket.co.kr/Item?goodscode=2710412548&ver=20230531
    91 오피스 2021 Home  Student (홈앤스튜던트) PKC 106,000원 http://item.gmarket.co.kr/Item?goodscode=2428212761&ver=20230531
    92 [삼성전자]삼성전자 DDR4 16G PC4-25600 (정품) -Mrc 22년 11주 46,280원 http://item.gmarket.co.kr/Item?goodscode=2827306317&ver=20230531
    93 [뷰플]뷰플 갈바닉마사지기 얼굴리프팅 피부홈케어 EMS 딜링클 35,070원 http://item.gmarket.co.kr/Item?goodscode=2654791706&ver=20230531
    94 아이폰14 13 프로 실리콘 케이스 애플 로고 아이폰12 프로 아이폰XS 커플 파스텔 폰케이스 11,900원 http://item.gmarket.co.kr/Item?goodscode=2498816088&ver=20230531
    95 [로지텍]로지텍코리아 G102ic 2세대 LIGHTSYNC 마우스 23,900원 http://item.gmarket.co.kr/Item?goodscode=1828364877&ver=20230531
    96 [신일전자]신일 스탠드형 리모컨 선풍기 SIF-S14RWS 86,000원 http://item.gmarket.co.kr/Item?goodscode=1373779506&ver=20230531
    97 [LG전자]수도권충청대구부산 기본설치포함 휘센 멀티 에어컨 FQ17HCKWC2 1,630,870원 http://item.gmarket.co.kr/Item?goodscode=2423169668&ver=20230531
    98 [비쎌]비쎌 다용도습식청소기 스팟클린 3698V /본사직영 전용포뮬라포함 리뷰선물증정 179,000원 http://item.gmarket.co.kr/Item?goodscode=1807271089&ver=20230531
    99 [Lenovo]태블릿 p12 샤오신패드 2022 4+64g 10.6인치 WIFI 중국롬 167,000원 http://item.gmarket.co.kr/Item?goodscode=2580451792&ver=20230531
    100 [그랩더기타]유어피스 무선 가정용 제모기 바디쉐이버(+교체날1개) 11,900원 http://item.gmarket.co.kr/Item?goodscode=2957762470&ver=20230531


### 2. 실전 예제: 크롤링 & 크롤링
* 이커머스(지마켓) 베스트100 상품 타이틀/가격, 그리고 판매업체 추출하기


```python
import requests
from bs4 import BeautifulSoup

import re                          
link_re = re.compile('^http://')  

res = requests.get('https://www.gmarket.co.kr/n/best?viewType=G&groupCode=G06')
soup = BeautifulSoup(res.content, 'html.parser')
# 웹사이트 코드가 수시로 변경되면서, best-list class 를 가진 태그가 하나이기 때문에 해당 태그만 선택하도록 수정
bestitems = soup.select_one('div.best-list') # select_one() 은 해당 조건에 맞는 태그 하나만 선택하는 함수
products = bestitems.select('ul > li')

for index, product in enumerate(products):
    title = product.select_one('a.itemname')
    price = product.select_one('div.s-price > strong')
    if link_re.match(title['href']):                 
        res_info = requests.get(title['href'])
        soup_info = BeautifulSoup(res_info.content, 'html.parser')
        
   
        # 웹사이트가 변경되어, 다음과 같이 작성해야 정상 크롤링이 되는 것으로 보입니다. 
        # 기존 코드: provider_info = soup_info.select_one('div.item-topinfo > div.item-topinfo_headline > p > a > strong')
        provider_info = soup_info.select_one("div.item-topinfo_headline span.text__seller > a")
        
        print (title.get_text(), price.get_text(), provider_info.get_text())
```

    [ASUS]긴급추가수량확보 ASUS ROG ALLY RC71L-NH001W 휴대용 게임기 Z1 Extreme/16GB/512GB/500Nits/Touch/UMPC 999,000원 ASUS공식총판
    [대우](혜택가3.5만원) 2023년형 대우 3단 높이조절 리모컨 34cm형 써큘레이터 선풍기 / DEF-KC3040 서큘레이터 39,900원 스마일배송
    [유파]내일도착유파 선풍기리모컨초미풍발터치 TSK-3528CR 32,180원 CJ온스타일
    [대우](혜택가 2.9만원) 2023년형 대우 에어 써큘레이터 DEF-KC1020 스탠드 선풍기 공기순환 서큘레이터 33,300원 스마일배송
    [LG전자]혜택가 45만원대 LG전자 휘센 제습기 20L 블루 DQ202PBBC 489,000원 LG공식인증점삼정
    [다이슨]다이슨 에어랩 멀티스타일러 롱 (니켈/코퍼) (10% 쿠폰할인) 749,000원 다이슨코리아유한회사
    [솔러스에어]욕실 화장실 천장형 캠핑용 선풍기 타프 실링팬 AIR701CF 28,900원 에어로코리아
    [위닉스]공식인증점) 위닉스 제습기 12L {DXTE120-MPK} (~5/31 카드가 21.9만) 235,480원 위닉스공식파트너두림
    [위닉스]위닉스 뽀송 제습기 17L DN3E170-LWK /RT 에너지소비효율1등급 제조사정품 422,800원 로켓가전몰
    [신일]23년 신상] 신일 BLDC 서큘레이터 air S9 1set(SIF-CS30BG/SIF-CS30DG/SIF-CS30BL) 137,760원 현대홈쇼핑Hmall
    [위니아]전국지역/기본설치포함)인버터 벽걸이에어컨 ERV06GHP + 471,000원 딤채SHOP
    [한경희생활과학]한경희 저소음 13L 제습기 HE-D707 / 예약판매 6월 12일 발송 189,000원 인프라맥스
    [윈드피아](최종2.6만)가정용 업소용 사무실 스탠드 키높이 선풍기 WA-170 28,600원 윈드피아
    [LG전자]LG전자 2023 울트라PC 15UD40Q-GX5DK 혜택가 49만 가성비 라이젠 노트북 669,000원 LG총판(주)이좋은세상
    [쿠쿠](혜택가 22.8만원) 쿠쿠 본사직영 CRP-CHP1010FD 10인용 IH전기압력 밥솥 260,110원 쿠쿠전자(주)
    [마이크론]마이크론 2300 M.2 nvMe Gen3 512GB 38,000원 휴비딕공식
    [까미또]갤럭시S23 S22 S21 S20 플러스 5G 노트20울트라 A53 A23 A52 A32 아이폰14 13 12 프로 맥스 핸드폰 케이스 6,900원 까미또
    [르젠]내일도착르젠 앱연동 BLDC 선풍기 LZEF-DC180 화이트 76,800원 CJ온스타일
    [대웅]대웅 가정용 스탠드선풍기  키높이선풍기 29,540원 lainzone
    [윈드피아](최종3만)가정용 업소용 스탠드 리모컨 선풍기 인기상품 1700R 39,900원 윈드피아
    [이노크아든]이노크 저소음 스탠드선풍기 14형 특가 IA-FBI7942 28,700원 스마일배송
    [신일전자]2023년 NEW  BLDC 서큘레이터 air S9 (베이지/딥그린/블루) 142,800원 롯데  홈쇼핑
    [닌텐도]젤다의 전설 티어스 오브 더 킹덤 (한글판) I 81,500원 아이게임몰
    [대우](혜택가 2.9만원) 2023년형 대우 3단 높이조절 34cm형 써큘레이터 선풍기 / DEF-KC2030 서큘레이터 33,300원 스마일배송
    [테팔]테팔 믹서기 블렌더 블렌드포스 플러스 BL4258KR BL4258 블랙 46,410원 테팔공식판매
    [르젠]르젠 스탠드형 리모컨 써큘레이터 선풍기 LZEF-AR03 51,900원 주이앤에스인터내셔널
    [쿠쿠]쿠쿠 국내정품 6인용 IH트윈프레셔 밥솥 CRP-LHTR0610FW 309,860원 쿠쿠국내정품스토어
    [신일전자]신일 스탠드 선풍기 저소음 12인치 SIF-12PNX 59,900원 SHINIL
    [코슬리]코슬리 핸디 스팀다리미 유니크 아메리카노 기프티콘 증정 36,700원 코슬리
    [대우](혜택가 3.5만원) 2023년형 대우 2단 높이조절 리모컨 40cm형 써큘레이터 선풍기 / DEF-NS1400 서큘레이터 39,900원 스마일배송
    [쿠쿠]쿠쿠 정품인증 10인용 풀스테인리스 IH트윈프레셔 밥솥 CRP-KHTS1060FD 277,550원 쿠쿠정품스토어
    [솔러스에어]솔러스에어 탁상용 선풍기 타이머 좌우회전 수면풍 자연풍 무선 무소음 미니 탁상형 소형 충전식 AIR604TF 34,800원 에어로코리아
    [유닉스]유닉스 전문가용 매직기 미용실 고데기 볼륨 스트레이트너 세라믹 앞머리 고대기 판고데기 추천 29,700원 레브커머스
    [윈드피아](최종2.3만)가정용선풍기 업소용선풍기 스탠드 선풍기 WA-370 27,900원 윈드피아
    [레드울프]갤럭시 S23 S22 울트라 노트20/노트10/S21/S20/S10/A53/A32/A23/A33/A71/A82 플러스 가죽 지갑형 폰케이스 19,900원 레드울프
    [OLLY(가전)]올리 멀티 에어프라이어 그릴 OLAG09B 에어프라이기 스테이크 그릴 89,000원 에어로코리아
    [삼성전자]삼성 무풍에어컨 벽걸이 슬림(AR07C9150HZT) 719,200원 NS홈쇼핑
    [르젠]르젠 앱연동 스마트 BLDC 리모컨 선풍기 LZEF-DC02 78,500원 주이앤에스인터내셔널
    [보만]카드혜택가 3.6만)32만개 판매 국민 스팀다리미 보만 핸디형 가정용 휴대용 여행용 의류스팀기 DB8231 42,700원 레브커머스
    [삼성전자]갤럭시 워치4 44mm / SM-R870N 최종가 169000원 179,000원 삼성모바일전문몰
    [캐리어]1등급 클라윈드 20L 제습기 CDHM-C020LLPL 299,000원 캐리어공식판매
    [신일전자][신일] 2022년형 BLDC air S8 써큘레이터 (베이지/딥그린/라이트핑크) 118,400원 현대홈쇼핑Hmall
    토트넘 훗스퍼 스마트워치 TOT-SW231 (2종 택1) 129,000원 MEGA_STORE
    [캐리어]6평형 인버터 벽걸이 에어컨 ORCD061FAWWSD (전국_기본설치비 포함)(추천) 479,790원 캐리어공식판매
    [피스넷]골전도 블루투스 이어폰 피스넷 프리본 시즌2 / 2023년 5월 신제품 49,900원 피스넷
    [쿠쿠](혜택가 5.9만원) 쿠쿠 본사직영 20L 전자레인지 CMW-A201DW 67,510원 쿠쿠전자(주)
    [쿠쿠](혜택가 20.9만원) 쿠쿠 본사직영 CRP-DHP0610FD 6인용 IH전기압력 밥솥 238,510원 쿠쿠전자(주)
    [ASUS]힐링쉴드 강화유리 필름 ROG ALLY 전용 14,800원 ASUS공식총판
    [테팔]테팔 스팀다리미 무선다리미 프리무브 에어 FV6550 83,610원 테팔공식판매
    [샤오미]샤오미 미지아 무선 선풍기 5세대 에어서큘레이터 BLDC 마그네틱 충전 돼지코 증정 BPLDS05DM 113,300원 e직구-HK
    [다이슨]다이슨 에어랩 멀티 스타일러 컴플리트 롱 (코퍼/니켈) 749,000원 다이슨코리아유한회사
    [위닉스]공식파트너_위닉스 뽀송 제습기 16L (DN2H160-IWK) 집중건조킷 포함 341,490원 공식파트너위니텍
    [위닉스]공식인증점)위닉스 컴팩트 미니건조기 플러스 {HS2E400-MGK} 오가닉그린 (최대 4KG) (~5/31 카드가 29.9만) 331,500원 위닉스공식파트너두림
    [신일전자]신일 BLDC 무소음 리모컨 선풍기 SIF-DC826 123,000원 니코샵
    [캐리어]캐리어 10L 제습기 CDHM-C010LMOW 22년형 신제품 199,000원 이지스21매장
    갤럭시 S23 S22 울트라 노트20/노트10/S21/S20/S10/A53/A32/A23/A33 플러스 핸드폰 휴대폰 가죽케이스 19,900원 꾹이몰
    [네고]2매+1 (이벤트참여) 강화유리 지문방지 갤럭시S23 S22 울트라 플러스 노트 플립 폴드 아이폰14 13 프로맥스 14,900원 네고네고
    [LG전자]LG전자 DQ132PWXC 지역별운송료상이 (가인) 398,920원 가인리테일
    [삼성전자]삼성 비스포크 무풍에어컨 윈도우핏 베이지 AW06C7155TWA 969,000원 NS홈쇼핑
    [필립스]최종13910원)PHILIPS 필립스 코털제거기 NT1620/15 콧털 정리 트리머 13,410원 (주)필립스코리아
    [대우]대우 써큘레이터형 BLDC 에코팬 리모컨 선풍기 73,800원 스마일배송
    [엔보우]모노 탁상용 무선 휴대용 선풍기 2세대 1+1(할인 행사) 29,900원 엔보우
    [신일전자]프리미엄 선풍기 SIF-T14DC - 14인치 / 상하.좌우 3D입체회전 BLDC / 무소음 써큘레이터 159,000원 SK매직공식스토어
    [신일]신일 BLDC 에어서큘레이터 AIR S9 베이지 2세트(SIF-TS10RA) 255,960원 NS홈쇼핑
    [유닉스]유닉스 미니 웨이브 볼륨 뽕 멀티 고데기 UCI-B2304 7,900원 엘더스
    [아이엔픽]핸드폰 갤럭시S23울트라 S22플러스 S21 S20 S10 노트20 노트10 노트9 노트8 A34 A53 A23 A32 A31 가죽 지갑 9,900원 엘엘비
    [LG전자]DQ202PSUA o클릭o LG전자/제습기 505,490원 다락방가전쇼핑몰
    [닌텐도]닌텐도 스위치 젤다의 전설 티어스 오브 더 킹덤 일반판 +특전 74,800원 지피클럽
    [위닉스]공식인증점) 위닉스 NEW 타워엣지 공기청정기 {AT8E430-MWK} 화이트 224,730원 위닉스공식파트너두림
    [샤클라]샤클라 로봇 물걸레청소기 KGC001 199,000원 공식인증샵
    [삼성전자]{신규런칭} NEW 갤럭시 A34 자급제 128GB SM-A346N/쿠폰(5%)추가할인/무이자(최대12개월)/당일발송 464,440원 삼성전자공식총판점
    [위니아]인증 위니아 가정용제습기 EDH8DNWB 8L 182,000원 WINIADIMCHAE
    [위닉스]{공식파트너} 위닉스 뽀송 제습기 10리터 DXAE100-JWK 215,690원 공식파트너위니텍
    [솔러스에어]탁상용 소형 미니 무소음 무선 휴대용 탁상 선풍기 AIR601TF 29,900원 에어로코리아
    [제이엠더블유]JMW 터보 항공기 MG1800 PLUS 올화이트 59,000원 스마일배송
    [LG전자]LG전자 27ART10AKPL 스탠드형 스탠바이미 TV (H) 978,000원 호야라인
    [샤오미]샤오미 미지아 LED 모니터 조명 1s MJGJD02YL 스크린바 무선리모컨 45,900원 meetg9
    [미홀]샤오미 MIWHOLE 20L 제습기 / 미홀 제습기 / 무료배송 124,000원 홍콩리미티드
    [쿠쿠]6인용 쿠쿠 고화력 밥솥 CRP-P0660FD+냉동용기 147,250원 CJ온스타일플러스
    모닝컴 리모컨 초미세풍 발터치스탠드선풍기 23년도 신상품 32,900원 lainzone
    [유닉스]20만개판매 유닉스 전문가용 드라이기 헤어드라이어 미용실 가정용 업소용 냉풍 찬바람 가성비 UN-A1770 41,940원 레브커머스
    [쿠쿠](혜택가 17.6만원) 쿠쿠 본사직영 10인용 IH압력 밥솥 CRP-HWF1060FDM 206,110원 쿠쿠전자(주)
    [필립스]카드가 12만원대) PHILIPS 전기면도기 SkinIQ 5000 S5582/36 오션블루 122,550원 (주)필립스코리아
    [삼성전자]삼성전자 정품 마이크로SD EVO Plus 512GB MB-MC512KA 33,960원 에버랩스
    [한경희이지라이프]한경희 제습기 HE-D720 대용량 20L (최종혜택가 21.5만 5월 신제품) /예약판매 249,000원 인프라맥스
    [위닉스]공식인증점)위닉스 컴팩트 미니건조기 플러스 {HS2E400-MEK} 화이트베이지 (최대 4KG) (~5/31 카드 29.9만) 321,500원 위닉스공식파트너두림
    [캐리어]총알배송 전국무료배송 캐리어 고효율 1등급 20리터 제습기 386,130원 신세계몰
    [씽크웨이]씽크에어 DL12 제습기 138,570원 씽크웨이
    오피스 2021 Home  Student (홈앤스튜던트) PKC 106,000원 스마일배송
    핸드폰/갤럭시/S23/S22/노트20/S21/S20/노트10/플러스/노트9/8/A34/키즈/A32/울트라/점프/아이폰14프로맥스 9,800원 컴퍼니제이
    [신일전자]신일 에어써큘레이터 화이트 SIF-FA800B 103,600원 CJ온스타일LIVE
    [뷰플]뷰플 갈바닉마사지기 얼굴리프팅 피부홈케어 EMS 딜링클 35,070원 (주)아워코퍼레이션
    [비쎌]비쎌 다용도습식청소기 스팟클린 3698V /본사직영 전용포뮬라포함 리뷰선물증정 179,000원 비쎌공식몰
    [삼성전자]삼성전자 DDR4 16G PC4-25600 (정품) -Mrc 22년 11주 46,280원 미라클솔루션
    아이폰14 13 프로 실리콘 케이스 애플 로고 아이폰12 프로 아이폰XS 커플 파스텔 폰케이스 11,900원 짜롱이케이스
    [로지텍]로지텍코리아 G102ic 2세대 LIGHTSYNC 마우스 23,900원 로지텍G공식스토어
    [신일전자]신일 스탠드형 리모컨 선풍기 SIF-S14RWS 86,000원 니코샵
    JCP1 Apple 애플 펜슬 2세대 (MU8F2KH/A)  오늘출발 내일도착 195,900원 제이씨엠컴퍼니
    [LG전자]수도권충청대구부산 기본설치포함 휘센 멀티 에어컨 FQ17HCKWC2 1,630,870원 네고네고
    [Lenovo]태블릿 p12 샤오신패드 2022 4+64g 10.6인치 WIFI 중국롬 167,000원 SMARTA



```python
price.get_text().replace('원', '').replace(',', '')
```




    '81500'



### 3. 실전 예제: 크롤링 & 크롤링 + 엑셀
* 이커머스(지마켓) 베스트100 상품 타이틀/가격, 그리고 판매업체 추출하기


```python
import requests, openpyxl
from bs4 import BeautifulSoup

import re                          
link_re = re.compile('^http://')   

excel_file = openpyxl.Workbook()
excel_sheet = excel_file.active
excel_sheet.append(['랭킹', '상품명', '판매가격', '상품상세링크', '판매업체'])
excel_sheet.column_dimensions['B'].width = 80
excel_sheet.column_dimensions['C'].width = 20
excel_sheet.column_dimensions['D'].width = 80
excel_sheet.column_dimensions['E'].width = 20

res = requests.get('http://corners.gmarket.co.kr/Bestsellers?viewType=G&groupCode=G06')
soup = BeautifulSoup(res.content, 'html.parser')

# 웹사이트 코드가 수시로 변경되면서, best-list class 를 가진 태그가 하나이기 때문에 해당 태그만 선택하도록 수정
bestitems = soup.select_one('div.best-list') # select_one() 은 해당 조건에 맞는 태그 하나만 선택하는 함수
products = bestitems.select('ul > li')

for index, product in enumerate(products):
    title = product.select_one('a.itemname')
    price = product.select_one('div.s-price > strong')
    if link_re.match(title['href']):           
        res_info = requests.get(title['href'])
        soup_info = BeautifulSoup(res_info.content, 'html.parser')


        # 웹사이트가 변경되어, 다음과 같이 작성해야 정상 크롤링이 되는 것으로 보입니다. 
        # 기존 코드: provider_info = soup_info.select_one('div.item-topinfo > div.item-topinfo_headline > p > a > strong')
        provider_info = soup_info.select_one("div.item-topinfo_headline span.text__seller > a")

        print(index + 1, title.get_text(), price.get_text(), title['href'], provider_info.get_text())
        excel_sheet.append([index + 1, title.get_text(), price.get_text(), title['href'], provider_info.get_text()])
        excel_sheet.cell(row=index+2 , column=4).hyperlink = title['href']


cell_A1 = excel_sheet['A1'] # 셀 선택하기
cell_A1.alignment = openpyxl.styles.Alignment(horizontal='center') # 중앙정렬하기
cell_A1.font = openpyxl.styles.Font(color="01579B") # 폰트 색깔 바꾸기
# 색상값 찾기: https://material.io/design/color/#tools-for-picking-colors

cell_B1 = excel_sheet['B1'] # 셀 선택하기
cell_B1.alignment = openpyxl.styles.Alignment(horizontal='center') # 중앙정렬하기
cell_B1.font = openpyxl.styles.Font(color="01579B") # 폰트 색깔 바꾸기
# 색상값 찾기: https://material.io/design/color/#tools-for-picking-colors

cell_C1 = excel_sheet['C1'] # 셀 선택하기
cell_C1.alignment = openpyxl.styles.Alignment(horizontal='center') # 중앙정렬하기
cell_C1.font = openpyxl.styles.Font(color="01579B") # 폰트 색깔 바꾸기
# 색상값 찾기: https://material.io/design/color/#tools-for-picking-colors

cell_D1 = excel_sheet['D1'] # 셀 선택하기
cell_D1.alignment = openpyxl.styles.Alignment(horizontal='center') # 중앙정렬하기
cell_D1.font = openpyxl.styles.Font(color="01579B") # 폰트 색깔 바꾸기
# 색상값 찾기: https://material.io/design/color/#tools-for-picking-colors

cell_E1 = excel_sheet['E1'] # 셀 선택하기
cell_E1.alignment = openpyxl.styles.Alignment(horizontal='center') # 중앙정렬하기
cell_E1.font = openpyxl.styles.Font(color="01579B") # 폰트 색깔 바꾸기
# 색상값 찾기: https://material.io/design/color/#tools-for-picking-colors

excel_file.save('BESTPRODUCT_COM_GMARKET.xlsx')
excel_file.close()
```

    1 [ASUS]긴급추가수량확보 ASUS ROG ALLY RC71L-NH001W 휴대용 게임기 Z1 Extreme/16GB/512GB/500Nits/Touch/UMPC 999,000원 http://item.gmarket.co.kr/Item?goodscode=2961459160&ver=20230601 ASUS공식총판
    2 [대우](혜택가3.5만원) 2023년형 대우 3단 높이조절 리모컨 34cm형 써큘레이터 선풍기 / DEF-KC3040 서큘레이터 39,900원 http://item.gmarket.co.kr/Item?goodscode=2865607786&ver=20230601 스마일배송
    3 [유파]내일도착유파 선풍기리모컨초미풍발터치 TSK-3528CR 32,180원 http://item.gmarket.co.kr/Item?goodscode=2177829053&ver=20230601 CJ온스타일
    4 [대우](혜택가 2.9만원) 2023년형 대우 에어 써큘레이터 DEF-KC1020 스탠드 선풍기 공기순환 서큘레이터 33,300원 http://item.gmarket.co.kr/Item?goodscode=1833787752&ver=20230601 스마일배송
    5 [LG전자]혜택가 45만원대 LG전자 휘센 제습기 20L 블루 DQ202PBBC 489,000원 http://item.gmarket.co.kr/Item?goodscode=2428196089&ver=20230601 LG공식인증점삼정
    6 [다이슨]다이슨 에어랩 멀티스타일러 롱 (니켈/코퍼) (10% 쿠폰할인) 749,000원 http://item.gmarket.co.kr/Item?goodscode=2841728540&ver=20230601 다이슨코리아유한회사
    7 [위닉스]위닉스 뽀송 제습기 17L DN3E170-LWK /RT 에너지소비효율1등급 제조사정품 422,800원 http://item.gmarket.co.kr/Item?goodscode=2514306486&ver=20230601 로켓가전몰
    8 [신일]23년 신상] 신일 BLDC 서큘레이터 air S9 1set(SIF-CS30BG/SIF-CS30DG/SIF-CS30BL) 137,760원 http://item.gmarket.co.kr/Item?goodscode=2947091062&ver=20230601 현대홈쇼핑Hmall
    9 [위니아]전국지역/기본설치포함)인버터 벽걸이에어컨 ERV06GHP + 471,000원 http://item.gmarket.co.kr/Item?goodscode=2401170088&ver=20230601 딤채SHOP
    10 [마이크론]마이크론 2300 M.2 nvMe Gen3 512GB 38,000원 http://item.gmarket.co.kr/Item?goodscode=2945769074&ver=20230601 휴비딕공식
    11 [LG전자]LG전자 2023 울트라PC 15UD40Q-GX5DK 혜택가 49만 가성비 라이젠 노트북 723,240원 http://item.gmarket.co.kr/Item?goodscode=2918818023&ver=20230601 LG총판(주)이좋은세상
    12 [쿠쿠](혜택가 22.8만원) 쿠쿠 본사직영 CRP-CHP1010FD 10인용 IH전기압력 밥솥 260,110원 http://item.gmarket.co.kr/Item?goodscode=1689086718&ver=20230601 쿠쿠전자(주)
    13 [위닉스]공식인증점) 위닉스 제습기 12L {DXTE120-MPK} (~5/31 카드가 21.9만) 235,480원 http://item.gmarket.co.kr/Item?goodscode=2861939687&ver=20230601 위닉스공식파트너두림
    14 [한경희생활과학]한경희 저소음 13L 제습기 HE-D707 / 예약판매 6월 12일 발송 189,000원 http://item.gmarket.co.kr/Item?goodscode=2098489923&ver=20230601 인프라맥스
    15 [까미또]갤럭시S23 S22 S21 S20 플러스 5G 노트20울트라 A53 A23 A52 A32 아이폰14 13 12 프로 맥스 핸드폰 케이스 6,900원 http://item.gmarket.co.kr/Item?goodscode=225532316&ver=20230601 까미또
    16 [르젠]내일도착르젠 앱연동 BLDC 선풍기 LZEF-DC180 화이트 77,800원 http://item.gmarket.co.kr/Item?goodscode=1859564938&ver=20230601 CJ온스타일
    17 [윈드피아]가정용 업소용 사무실 스탠드 키높이 선풍기 WA-170 28,600원 http://item.gmarket.co.kr/Item?goodscode=1298066886&ver=20230601 윈드피아
    18 [대웅]대웅 가정용 스탠드선풍기  키높이선풍기 29,540원 http://item.gmarket.co.kr/Item?goodscode=1783779831&ver=20230601 lainzone
    19 [르젠]르젠 스탠드형 리모컨 써큘레이터 선풍기 LZEF-AR03 51,900원 http://item.gmarket.co.kr/Item?goodscode=2554812512&ver=20230601 주이앤에스인터내셔널
    20 [닌텐도]젤다의 전설 티어스 오브 더 킹덤 (한글판) I 81,500원 http://item.gmarket.co.kr/Item?goodscode=2935472887&ver=20230601 아이게임몰
    21 [대우](혜택가 2.9만원) 2023년형 대우 3단 높이조절 34cm형 써큘레이터 선풍기 / DEF-KC2030 서큘레이터 33,300원 http://item.gmarket.co.kr/Item?goodscode=2868268543&ver=20230601 스마일배송
    22 [윈드피아]가정용 업소용 스탠드 리모컨 선풍기 인기상품 1700R 39,900원 http://item.gmarket.co.kr/Item?goodscode=730730680&ver=20230601 윈드피아
    23 [코슬리]코슬리 핸디 스팀다리미 유니크 아메리카노 기프티콘 증정 36,700원 http://item.gmarket.co.kr/Item?goodscode=2005499762&ver=20230601 코슬리
    24 [쿠쿠]쿠쿠 국내정품 6인용 IH트윈프레셔 밥솥 CRP-LHTR0610FW 318,780원 http://item.gmarket.co.kr/Item?goodscode=2810660898&ver=20230601 쿠쿠국내정품스토어
    25 [신일전자]신일 스탠드 선풍기 저소음 12인치 SIF-12PNX 59,900원 http://item.gmarket.co.kr/Item?goodscode=1324935106&ver=20230601 SHINIL
    26 [유닉스]유닉스 전문가용 매직기 미용실 고데기 볼륨 스트레이트너 세라믹 앞머리 고대기 판고데기 추천 29,700원 http://item.gmarket.co.kr/Item?goodscode=2606062725&ver=20230601 레브커머스
    27 [쿠쿠]쿠쿠 정품인증 10인용 풀스테인리스 IH트윈프레셔 밥솥 CRP-KHTS1060FD 277,550원 http://item.gmarket.co.kr/Item?goodscode=2861619916&ver=20230601 쿠쿠정품스토어
    28 [이노크아든]이노크 저소음 스탠드선풍기 14형 특가 IA-FBI7942 28,700원 http://item.gmarket.co.kr/Item?goodscode=1428376659&ver=20230601 스마일배송
    29 [르젠]르젠 앱연동 스마트 BLDC 리모컨 선풍기 LZEF-DC02 78,500원 http://item.gmarket.co.kr/Item?goodscode=2843298236&ver=20230601 주이앤에스인터내셔널
    30 [솔러스에어]욕실 화장실 천장형 캠핑용 선풍기 타프 실링팬 AIR701CF 28,900원 http://item.gmarket.co.kr/Item?goodscode=2892450550&ver=20230601 에어로코리아
    31 [솔러스에어]솔러스에어 탁상용 선풍기 타이머 좌우회전 수면풍 자연풍 무선 무소음 미니 탁상형 소형 충전식 AIR604TF 34,800원 http://item.gmarket.co.kr/Item?goodscode=2877456477&ver=20230601 에어로코리아
    32 [쿠쿠]6인용 쿠쿠 고화력 밥솥 CRP-P0660FD+냉동용기 147,250원 http://item.gmarket.co.kr/Item?goodscode=2710412548&ver=20230601 CJ온스타일플러스
    33 [유닉스]유닉스 미니 웨이브 볼륨 뽕 멀티 고데기 UCI-B2304 7,900원 http://item.gmarket.co.kr/Item?goodscode=2610076235&ver=20230601 엘더스
    34 [쿠쿠](혜택가 20.9만원) 쿠쿠 본사직영 CRP-DHP0610FD 6인용 IH전기압력 밥솥 238,510원 http://item.gmarket.co.kr/Item?goodscode=1692190118&ver=20230601 쿠쿠전자(주)
    35 [쿠쿠](혜택가 5.9만원) 쿠쿠 본사직영 20L 전자레인지 CMW-A201DW 67,510원 http://item.gmarket.co.kr/Item?goodscode=2839783463&ver=20230601 쿠쿠전자(주)
    36 [삼성전자]삼성 무풍에어컨 벽걸이 슬림(AR07C9150HZT) 719,200원 http://item.gmarket.co.kr/Item?goodscode=2958178866&ver=20230601 NS홈쇼핑
    37 토트넘 훗스퍼 스마트워치 TOT-SW231 (2종 택1) 129,000원 http://item.gmarket.co.kr/Item?goodscode=2962116010&ver=20230601 MEGA_STORE
    38 [보만]카드혜택가 3.6만)32만개 판매 국민 스팀다리미 보만 핸디형 가정용 휴대용 여행용 의류스팀기 DB8231 42,700원 http://item.gmarket.co.kr/Item?goodscode=1224898468&ver=20230601 레브커머스
    39 [대우](혜택가 3.5만원) 2023년형 대우 2단 높이조절 리모컨 40cm형 써큘레이터 선풍기 / DEF-NS1400 서큘레이터 39,900원 http://item.gmarket.co.kr/Item?goodscode=2950107808&ver=20230601 스마일배송
    40 [ASUS]힐링쉴드 강화유리 필름 ROG ALLY 전용 14,800원 http://item.gmarket.co.kr/Item?goodscode=2971758521&ver=20230601 ASUS공식총판
    41 [캐리어]6평형 인버터 벽걸이 에어컨 ORCD061FAWWSD (전국_기본설치비 포함)(추천) 479,790원 http://item.gmarket.co.kr/Item?goodscode=2424340425&ver=20230601 캐리어공식판매
    42 모닝컴 리모컨 초미세풍 발터치스탠드선풍기 23년도 신상품 32,900원 http://item.gmarket.co.kr/Item?goodscode=2923134780&ver=20230601 lainzone
    43 [레드울프]갤럭시 S23 S22 울트라 노트20/노트10/S21/S20/S10/A53/A32/A23/A33/A71/A82 플러스 가죽 지갑형 폰케이스 19,900원 http://item.gmarket.co.kr/Item?goodscode=268988080&ver=20230601 레드울프
    44 [테팔]테팔 스팀다리미 무선다리미 프리무브 에어 FV6550 83,610원 http://item.gmarket.co.kr/Item?goodscode=2330377014&ver=20230601 테팔공식판매
    45 [삼성전자]갤럭시 워치4 44mm / SM-R870N 최종가 169000원 179,000원 http://item.gmarket.co.kr/Item?goodscode=2248339963&ver=20230601 삼성모바일전문몰
    46 [LG전자]LG전자 DQ132PWXC 지역별운송료상이 (가인) 398,920원 http://item.gmarket.co.kr/Item?goodscode=2494906253&ver=20230601 가인리테일
    47 [숲속바람]Forest Wind 저소음 5엽 스탠드 선풍기 28,710원 http://item.gmarket.co.kr/Item?goodscode=2106722895&ver=20230601 B2Bworld
    48 [샤오미]샤오미 미지아 무선 선풍기 5세대 에어서큘레이터 BLDC 마그네틱 충전 돼지코 증정 BPLDS05DM 113,300원 http://item.gmarket.co.kr/Item?goodscode=2887042390&ver=20230601 e직구-HK
    49 [위닉스]공식파트너_위닉스 뽀송 제습기 16L (DN2H160-IWK) 집중건조킷 포함 351,280원 http://item.gmarket.co.kr/Item?goodscode=2879470884&ver=20230601 공식파트너위니텍
    50 [피스넷]골전도 블루투스 이어폰 피스넷 프리본 시즌2 / 2023년 5월 신제품 49,900원 http://item.gmarket.co.kr/Item?goodscode=1656073805&ver=20230601 피스넷
    51 [신일]신일 BLDC 에어서큘레이터 AIR S9 베이지 2세트(SIF-TS10RA) 255,960원 http://item.gmarket.co.kr/Item?goodscode=2962153901&ver=20230601 NS홈쇼핑
    52 갤럭시 S23 S22 울트라 노트20/노트10/S21/S20/S10/A53/A32/A23/A33 플러스 핸드폰 휴대폰 가죽케이스 19,900원 http://item.gmarket.co.kr/Item?goodscode=826268928&ver=20230601 꾹이몰
    53 [신일전자]신일 BLDC 무소음 리모컨 선풍기 SIF-DC826 123,000원 http://item.gmarket.co.kr/Item?goodscode=2841795103&ver=20230601 니코샵
    54 [윈드피아]가정용선풍기 업소용선풍기 스탠드 선풍기 WA-370 27,900원 http://item.gmarket.co.kr/Item?goodscode=1382089049&ver=20230601 윈드피아
    55 [삼성전자]삼성전자 정품 마이크로SD EVO Plus 512GB MB-MC512KA 33,960원 http://item.gmarket.co.kr/Item?goodscode=2258073651&ver=20230601 에버랩스
    56 [캐리어]1등급 클라윈드 20L 제습기 CDHM-C020LLPL 299,000원 http://item.gmarket.co.kr/Item?goodscode=2413161576&ver=20230601 캐리어공식판매
    57 [LG전자]DQ202PSUA o클릭o LG전자/제습기 505,490원 http://item.gmarket.co.kr/Item?goodscode=2454689996&ver=20230601 다락방가전쇼핑몰
    58 [신일전자][신일] 2022년형 BLDC air S8 써큘레이터 (베이지/딥그린/라이트핑크) 125,800원 http://item.gmarket.co.kr/Item?goodscode=2451351419&ver=20230601 현대홈쇼핑Hmall
    59 [필립스]카드가 12만원대) PHILIPS 전기면도기 SkinIQ 5000 S5582/36 오션블루 122,550원 http://item.gmarket.co.kr/Item?goodscode=2065305475&ver=20230601 (주)필립스코리아
    60 [신일전자]프리미엄 선풍기 SIF-T14DC - 14인치 / 상하.좌우 3D입체회전 BLDC / 무소음 써큘레이터 159,000원 http://item.gmarket.co.kr/Item?goodscode=2893946602&ver=20230601 SK매직공식스토어
    61 [아이엔픽]핸드폰 갤럭시S23울트라 S22플러스 S21 S20 S10 노트20 노트10 노트9 노트8 A34 A53 A23 A32 A31 가죽 지갑 9,900원 http://item.gmarket.co.kr/Item?goodscode=1418909600&ver=20230601 엘엘비
    62 [샤클라]샤클라 로봇 물걸레청소기 KGC001 199,000원 http://item.gmarket.co.kr/Item?goodscode=2225642928&ver=20230601 공식인증샵
    63 [뷰플]뷰플 갈바닉마사지기 얼굴리프팅 피부홈케어 EMS 딜링클 40,500원 http://item.gmarket.co.kr/Item?goodscode=2654791706&ver=20230601 (주)아워코퍼레이션
    64 [테팔]테팔 믹서기 블렌더 블렌드포스 플러스 BL4258KR BL4258 블랙 46,410원 http://item.gmarket.co.kr/Item?goodscode=2310724665&ver=20230601 테팔공식판매
    65 [네고]2매+1 (이벤트참여) 강화유리 지문방지 갤럭시S23 S22 울트라 플러스 노트 플립 폴드 아이폰14 13 프로맥스 14,900원 http://item.gmarket.co.kr/Item?goodscode=637253436&ver=20230601 네고네고
    66 [다이슨]다이슨 에어랩 멀티 스타일러 컴플리트 롱 (코퍼/니켈) 749,000원 http://item.gmarket.co.kr/Item?goodscode=2935603979&ver=20230601 다이슨코리아유한회사
    67 [캐리어]캐리어 10L 제습기 CDHM-C010LMOW 22년형 신제품 199,000원 http://item.gmarket.co.kr/Item?goodscode=2441792558&ver=20230601 이지스21매장
    68 [삼성전자]{신규런칭} NEW 갤럭시 A34 자급제 128GB SM-A346N/쿠폰(5%)추가할인/무이자(최대12개월)/당일발송 464,440원 http://item.gmarket.co.kr/Item?goodscode=2861319917&ver=20230601 삼성전자공식총판점
    69 [신일전자]신일 에어써큘레이터 화이트 SIF-FA800B 103,600원 http://item.gmarket.co.kr/Item?goodscode=2955811170&ver=20230601 CJ온스타일LIVE
    70 [위닉스]공식인증점)위닉스 컴팩트 미니건조기 플러스 {HS2E400-MEK} 화이트베이지 (최대 4KG) (~5/31 카드 29.9만) 321,500원 http://item.gmarket.co.kr/Item?goodscode=2548097351&ver=20230601 위닉스공식파트너두림
    71 [솔러스에어]탁상용 소형 미니 무소음 무선 휴대용 탁상 선풍기 AIR601TF 29,900원 http://item.gmarket.co.kr/Item?goodscode=2839674262&ver=20230601 에어로코리아
    72 [삼성전자]삼성 비스포크 무풍에어컨 윈도우핏 베이지 AW06C7155TWA 969,000원 http://item.gmarket.co.kr/Item?goodscode=2958178600&ver=20230601 NS홈쇼핑
    73 [엔보우]모노 탁상용 무선 휴대용 선풍기 2세대 1+1(할인 행사) 29,900원 http://item.gmarket.co.kr/Item?goodscode=1812271382&ver=20230601 엔보우
    74 [대우]대우 써큘레이터형 BLDC 에코팬 리모컨 선풍기 73,800원 http://item.gmarket.co.kr/Item?goodscode=1863410114&ver=20230601 스마일배송
    75 [위닉스]{공식파트너} 위닉스 뽀송 제습기  10리터 DXAE100-JWK 224,070원 http://item.gmarket.co.kr/Item?goodscode=510161514&ver=20230601 공식파트너위니텍
    76 [LG전자]LG전자 27ART10AKPL 스탠드형 스탠바이미 TV (H) 978,000원 http://item.gmarket.co.kr/Item?goodscode=2927118819&ver=20230601 호야라인
    77 [필립스]최종13910원)PHILIPS 필립스 코털제거기 NT1620/15 콧털 정리 트리머 13,410원 http://item.gmarket.co.kr/Item?goodscode=1894866019&ver=20230601 (주)필립스코리아
    78 [위니아]인증 위니아 가정용제습기 EDH8DNWB 8L 182,000원 http://item.gmarket.co.kr/Item?goodscode=1817247637&ver=20230601 WINIADIMCHAE
    79 [위닉스]공식인증점) 위닉스 NEW 타워엣지 공기청정기 {AT8E430-MWK} 화이트 224,730원 http://item.gmarket.co.kr/Item?goodscode=2882340090&ver=20230601 위닉스공식파트너두림
    80 [비쎌]비쎌 다용도습식청소기 스팟클린 3698V /본사직영 전용포뮬라포함 리뷰선물증정 199,000원 http://item.gmarket.co.kr/Item?goodscode=1807271089&ver=20230601 비쎌공식몰
    81 [닌텐도]닌텐도 스위치 젤다의 전설 티어스 오브 더 킹덤 일반판 +특전 74,800원 http://item.gmarket.co.kr/Item?goodscode=2847648865&ver=20230601 지피클럽
    82 [샤오미]샤오미 미지아 LED 모니터 조명 1s MJGJD02YL 스크린바 무선리모컨 45,900원 http://item.gmarket.co.kr/Item?goodscode=2866332287&ver=20230601 meetg9
    83 [쿠쿠](혜택가 17.6만원) 쿠쿠 본사직영 10인용 IH압력 밥솥 CRP-HWF1060FDM 206,110원 http://item.gmarket.co.kr/Item?goodscode=1624106044&ver=20230601 쿠쿠전자(주)
    84 [한경희이지라이프]한경희 제습기 HE-D720 대용량 20L (최종혜택가 21.5만 5월 신제품) /예약판매 249,000원 http://item.gmarket.co.kr/Item?goodscode=2923177563&ver=20230601 인프라맥스
    85 [신일전자]신일 스탠드형 리모컨 선풍기 SIF-S14RWS 86,000원 http://item.gmarket.co.kr/Item?goodscode=1373779506&ver=20230601 니코샵
    86 [유닉스]유닉스 음이온 고출력 헤어 드라이어 2200W UN-A1470N 17,900원 http://item.gmarket.co.kr/Item?goodscode=2460495908&ver=20230601 엘더스
    87 [신일전자]2023년 NEW  BLDC 서큘레이터 air S9 (베이지/딥그린/블루) 142,800원 http://item.gmarket.co.kr/Item?goodscode=2948035919&ver=20230601 롯데  홈쇼핑
    88 [신일전자]신일 기본형 스탠드 선풍기 SIF-P128 스탠드형 60,900원 http://item.gmarket.co.kr/Item?goodscode=2897568954&ver=20230601 니코샵
    89 [위닉스]공식인증점)위닉스 컴팩트 미니건조기 플러스 {HS2E400-MGK} 오가닉그린 (최대 4KG) (~5/31 카드가 29.9만) 331,500원 http://item.gmarket.co.kr/Item?goodscode=2548106026&ver=20230601 위닉스공식파트너두림
    90 [삼성전자]삼성전자 DDR4 16G PC4-25600 (정품) -Mrc 22년 11주 46,280원 http://item.gmarket.co.kr/Item?goodscode=2827306317&ver=20230601 미라클솔루션
    91 [르젠]르젠 16인치 입체회전 BLDC선풍기 LZDF-BT620 99,800원 http://item.gmarket.co.kr/Item?goodscode=2921036939&ver=20230601 주이앤에스인터내셔널
    92 [쿠쿠](혜택가 6.1만원) 쿠쿠 본사직영 CR-0675FW 에그 밥솥 6인용 밥솥 69,310원 http://item.gmarket.co.kr/Item?goodscode=1719323841&ver=20230601 쿠쿠전자(주)
    93 [로지텍]로지텍코리아 G102ic 2세대 LIGHTSYNC 마우스 23,900원 http://item.gmarket.co.kr/Item?goodscode=1828364877&ver=20230601 로지텍G공식스토어
    94 [쿠쿠](혜택가 8.0만원) 쿠쿠 본사직영 5.5L 에어프라이어 CAF-G0610TB 90,550원 http://item.gmarket.co.kr/Item?goodscode=2178092544&ver=20230601 쿠쿠전자(주)
    95 루메나 무선 써큘레이터 (애쉬브라운) FAN-STAND3-BR 34,900원 http://item.gmarket.co.kr/Item?goodscode=2970067951&ver=20230601 AKmall
    96 JCP1 Apple 애플 펜슬 2세대 (MU8F2KH/A)  오늘출발 내일도착 195,900원 http://item.gmarket.co.kr/Item?goodscode=2696387467&ver=20230601 제이씨엠컴퍼니
    97 [삼성전자]삼성전자 DDR4 8G PC4-25600 (정품)-Mrc 22,770원 http://item.gmarket.co.kr/Item?goodscode=2741864059&ver=20230601 미라클솔루션
    98 [삼성전자]삼성전자 삼성 870 EVO SATA3 500G MZ-77E500B/KR SSD 52,270원 http://item.gmarket.co.kr/Item?goodscode=2446525915&ver=20230601 하나아이엔티
    99 [신일전자]신일 SIF-14GI 14인치 가정용/스탠드 선풍기 5엽날개 57,400원 http://item.gmarket.co.kr/Item?goodscode=2101739267&ver=20230601 시즌클럽
    100 [미홀]샤오미 MIWHOLE 20L 제습기 / 미홀 제습기 / 무료배송 124,000원 http://item.gmarket.co.kr/Item?goodscode=2544070928&ver=20230601 홍콩리미티드



```python
import requests, openpyxl
from bs4 import BeautifulSoup

excel_file = openpyxl.Workbook()
excel_sheet = excel_file.active
excel_sheet.append(['랭킹', '상품명', '판매가격', '상품상세링크', '판매업체'])

res = requests.get('https://www.gmarket.co.kr/n/best?viewType=G&groupCode=G06')
soup = BeautifulSoup(res.content, 'html.parser')

# 웹사이트 코드가 수시로 변경되면서, best-list class 를 가진 태그가 하나이기 때문에 해당 태그만 선택하도록 수정
bestitems = soup.select_one('div.best-list') # select_one() 은 해당 조건에 맞는 태그 하나만 선택하는 함수
products = bestitems.select('ul > li')

for index, product in enumerate(products):
    title = product.select_one('a.itemname')
    price = product.select_one('div.s-price > strong')
    if link_re.match(title['href']):            
        res_info = requests.get(title['href'])
        soup_info = BeautifulSoup(res_info.content, 'html.parser')

        # 웹사이트가 변경되어, 다음과 같이 작성해야 정상 크롤링이 되는 것으로 보입니다. 
        # 기존 코드: provider_info = soup_info.select_one('div.item-topinfo > div.item-topinfo_headline > p > a > strong')
        provider_info = soup_info.select_one("div.item-topinfo_headline span.text__seller > a")

        print(index + 1, title.get_text(), price.get_text(), title['href'], provider_info.get_text())
        excel_sheet.append([index + 1, title.get_text(), price.get_text(), title['href'], provider_info.get_text()])

excel_file.save('BESTPRODUCT_COM_GMARKET.xlsx')
excel_file.close()
```

    1 [ASUS]긴급추가수량확보 ASUS ROG ALLY RC71L-NH001W 휴대용 게임기 Z1 Extreme/16GB/512GB/500Nits/Touch/UMPC 999,000원 http://item.gmarket.co.kr/Item?goodscode=2961459160&ver=20230531 ASUS공식총판
    2 [대우](혜택가3.5만원) 2023년형 대우 3단 높이조절 리모컨 34cm형 써큘레이터 선풍기 / DEF-KC3040 서큘레이터 39,900원 http://item.gmarket.co.kr/Item?goodscode=2865607786&ver=20230531 스마일배송
    3 [유파]내일도착유파 선풍기리모컨초미풍발터치 TSK-3528CR 32,180원 http://item.gmarket.co.kr/Item?goodscode=2177829053&ver=20230531 CJ온스타일
    4 [대우](혜택가 2.9만원) 2023년형 대우 에어 써큘레이터 DEF-KC1020 스탠드 선풍기 공기순환 서큘레이터 33,300원 http://item.gmarket.co.kr/Item?goodscode=1833787752&ver=20230531 스마일배송
    5 [LG전자]혜택가 45만원대 LG전자 휘센 제습기 20L 블루 DQ202PBBC 489,000원 http://item.gmarket.co.kr/Item?goodscode=2428196089&ver=20230531 LG공식인증점삼정
    6 [다이슨]다이슨 에어랩 멀티스타일러 롱 (니켈/코퍼) (10% 쿠폰할인) 749,000원 http://item.gmarket.co.kr/Item?goodscode=2841728540&ver=20230531 다이슨코리아유한회사
    7 [솔러스에어]욕실 화장실 천장형 캠핑용 선풍기 타프 실링팬 AIR701CF 28,900원 http://item.gmarket.co.kr/Item?goodscode=2892450550&ver=20230531 에어로코리아
    8 [위닉스]공식인증점) 위닉스 제습기 12L {DXTE120-MPK} (~5/31 카드가 21.9만) 235,480원 http://item.gmarket.co.kr/Item?goodscode=2861939687&ver=20230531 위닉스공식파트너두림
    9 [위닉스]위닉스 뽀송 제습기 17L DN3E170-LWK /RT 에너지소비효율1등급 제조사정품 422,800원 http://item.gmarket.co.kr/Item?goodscode=2514306486&ver=20230531 로켓가전몰
    10 [신일]23년 신상] 신일 BLDC 서큘레이터 air S9 1set(SIF-CS30BG/SIF-CS30DG/SIF-CS30BL) 137,760원 http://item.gmarket.co.kr/Item?goodscode=2947091062&ver=20230531 현대홈쇼핑Hmall
    11 [위니아]전국지역/기본설치포함)인버터 벽걸이에어컨 ERV06GHP + 471,000원 http://item.gmarket.co.kr/Item?goodscode=2401170088&ver=20230531 딤채SHOP
    12 [한경희생활과학]한경희 저소음 13L 제습기 HE-D707 / 예약판매 6월 12일 발송 189,000원 http://item.gmarket.co.kr/Item?goodscode=2098489923&ver=20230531 인프라맥스
    13 [윈드피아](최종2.6만)가정용 업소용 사무실 스탠드 키높이 선풍기 WA-170 28,600원 http://item.gmarket.co.kr/Item?goodscode=1298066886&ver=20230531 윈드피아
    14 [LG전자]LG전자 2023 울트라PC 15UD40Q-GX5DK 혜택가 49만 가성비 라이젠 노트북 669,000원 http://item.gmarket.co.kr/Item?goodscode=2918818023&ver=20230531 LG총판(주)이좋은세상
    15 [쿠쿠](혜택가 22.8만원) 쿠쿠 본사직영 CRP-CHP1010FD 10인용 IH전기압력 밥솥 260,110원 http://item.gmarket.co.kr/Item?goodscode=1689086718&ver=20230531 쿠쿠전자(주)
    16 [까미또]갤럭시S23 S22 S21 S20 플러스 5G 노트20울트라 A53 A23 A52 A32 아이폰14 13 12 프로 맥스 핸드폰 케이스 6,900원 http://item.gmarket.co.kr/Item?goodscode=225532316&ver=20230531 까미또
    17 [마이크론]마이크론 2300 M.2 nvMe Gen3 512GB 38,000원 http://item.gmarket.co.kr/Item?goodscode=2945769074&ver=20230531 휴비딕공식
    18 [르젠]내일도착르젠 앱연동 BLDC 선풍기 LZEF-DC180 화이트 76,800원 http://item.gmarket.co.kr/Item?goodscode=1859564938&ver=20230531 CJ온스타일
    19 [대웅]대웅 가정용 스탠드선풍기  키높이선풍기 29,540원 http://item.gmarket.co.kr/Item?goodscode=1783779831&ver=20230531 lainzone
    20 [윈드피아](최종3만)가정용 업소용 스탠드 리모컨 선풍기 인기상품 1700R 39,900원 http://item.gmarket.co.kr/Item?goodscode=730730680&ver=20230531 윈드피아
    21 [이노크아든]이노크 저소음 스탠드선풍기 14형 특가 IA-FBI7942 28,700원 http://item.gmarket.co.kr/Item?goodscode=1428376659&ver=20230531 스마일배송
    22 [신일전자]2023년 NEW  BLDC 서큘레이터 air S9 (베이지/딥그린/블루) 142,800원 http://item.gmarket.co.kr/Item?goodscode=2948035919&ver=20230531 롯데  홈쇼핑
    23 [닌텐도]젤다의 전설 티어스 오브 더 킹덤 (한글판) I 81,500원 http://item.gmarket.co.kr/Item?goodscode=2935472887&ver=20230531 아이게임몰
    24 [대우](혜택가 2.9만원) 2023년형 대우 3단 높이조절 34cm형 써큘레이터 선풍기 / DEF-KC2030 서큘레이터 33,300원 http://item.gmarket.co.kr/Item?goodscode=2868268543&ver=20230531 스마일배송
    25 [테팔]테팔 믹서기 블렌더 블렌드포스 플러스 BL4258KR BL4258 블랙 46,410원 http://item.gmarket.co.kr/Item?goodscode=2310724665&ver=20230531 테팔공식판매
    26 [르젠]르젠 스탠드형 리모컨 써큘레이터 선풍기 LZEF-AR03 51,900원 http://item.gmarket.co.kr/Item?goodscode=2554812512&ver=20230531 주이앤에스인터내셔널
    27 [쿠쿠]쿠쿠 국내정품 6인용 IH트윈프레셔 밥솥 CRP-LHTR0610FW 309,860원 http://item.gmarket.co.kr/Item?goodscode=2810660898&ver=20230531 쿠쿠국내정품스토어
    28 [신일전자]신일 스탠드 선풍기 저소음 12인치 SIF-12PNX 59,900원 http://item.gmarket.co.kr/Item?goodscode=1324935106&ver=20230531 SHINIL
    29 [코슬리]코슬리 핸디 스팀다리미 유니크 아메리카노 기프티콘 증정 36,700원 http://item.gmarket.co.kr/Item?goodscode=2005499762&ver=20230531 코슬리
    30 [대우](혜택가 3.5만원) 2023년형 대우 2단 높이조절 리모컨 40cm형 써큘레이터 선풍기 / DEF-NS1400 서큘레이터 39,900원 http://item.gmarket.co.kr/Item?goodscode=2950107808&ver=20230531 스마일배송
    31 [쿠쿠]쿠쿠 정품인증 10인용 풀스테인리스 IH트윈프레셔 밥솥 CRP-KHTS1060FD 277,550원 http://item.gmarket.co.kr/Item?goodscode=2861619916&ver=20230531 쿠쿠정품스토어
    32 [솔러스에어]솔러스에어 탁상용 선풍기 타이머 좌우회전 수면풍 자연풍 무선 무소음 미니 탁상형 소형 충전식 AIR604TF 34,800원 http://item.gmarket.co.kr/Item?goodscode=2877456477&ver=20230531 에어로코리아
    33 [유닉스]유닉스 전문가용 매직기 미용실 고데기 볼륨 스트레이트너 세라믹 앞머리 고대기 판고데기 추천 29,700원 http://item.gmarket.co.kr/Item?goodscode=2606062725&ver=20230531 레브커머스
    34 [윈드피아](최종2.3만)가정용선풍기 업소용선풍기 스탠드 선풍기 WA-370 27,900원 http://item.gmarket.co.kr/Item?goodscode=1382089049&ver=20230531 윈드피아
    35 [레드울프]갤럭시 S23 S22 울트라 노트20/노트10/S21/S20/S10/A53/A32/A23/A33/A71/A82 플러스 가죽 지갑형 폰케이스 19,900원 http://item.gmarket.co.kr/Item?goodscode=268988080&ver=20230531 레드울프
    36 [OLLY(가전)]올리 멀티 에어프라이어 그릴 OLAG09B 에어프라이기 스테이크 그릴 89,000원 http://item.gmarket.co.kr/Item?goodscode=2839853819&ver=20230531 에어로코리아
    37 [삼성전자]삼성 무풍에어컨 벽걸이 슬림(AR07C9150HZT) 719,200원 http://item.gmarket.co.kr/Item?goodscode=2958178866&ver=20230531 NS홈쇼핑
    38 [르젠]르젠 앱연동 스마트 BLDC 리모컨 선풍기 LZEF-DC02 78,500원 http://item.gmarket.co.kr/Item?goodscode=2843298236&ver=20230531 주이앤에스인터내셔널
    39 [보만]카드혜택가 3.6만)32만개 판매 국민 스팀다리미 보만 핸디형 가정용 휴대용 여행용 의류스팀기 DB8231 42,700원 http://item.gmarket.co.kr/Item?goodscode=1224898468&ver=20230531 레브커머스
    40 [삼성전자]갤럭시 워치4 44mm / SM-R870N 최종가 169000원 179,000원 http://item.gmarket.co.kr/Item?goodscode=2248339963&ver=20230531 삼성모바일전문몰
    41 [캐리어]1등급 클라윈드 20L 제습기 CDHM-C020LLPL 299,000원 http://item.gmarket.co.kr/Item?goodscode=2413161576&ver=20230531 캐리어공식판매
    42 [신일전자][신일] 2022년형 BLDC air S8 써큘레이터 (베이지/딥그린/라이트핑크) 118,400원 http://item.gmarket.co.kr/Item?goodscode=2451351419&ver=20230531 현대홈쇼핑Hmall
    43 토트넘 훗스퍼 스마트워치 TOT-SW231 (2종 택1) 129,000원 http://item.gmarket.co.kr/Item?goodscode=2962116010&ver=20230531 MEGA_STORE
    44 [캐리어]6평형 인버터 벽걸이 에어컨 ORCD061FAWWSD (전국_기본설치비 포함)(추천) 479,790원 http://item.gmarket.co.kr/Item?goodscode=2424340425&ver=20230531 캐리어공식판매
    45 [피스넷]골전도 블루투스 이어폰 피스넷 프리본 시즌2 / 2023년 5월 신제품 49,900원 http://item.gmarket.co.kr/Item?goodscode=1656073805&ver=20230531 피스넷
    46 [쿠쿠](혜택가 5.9만원) 쿠쿠 본사직영 20L 전자레인지 CMW-A201DW 67,510원 http://item.gmarket.co.kr/Item?goodscode=2839783463&ver=20230531 쿠쿠전자(주)
    47 [쿠쿠](혜택가 20.9만원) 쿠쿠 본사직영 CRP-DHP0610FD 6인용 IH전기압력 밥솥 238,510원 http://item.gmarket.co.kr/Item?goodscode=1692190118&ver=20230531 쿠쿠전자(주)
    48 [ASUS]힐링쉴드 강화유리 필름 ROG ALLY 전용 14,800원 http://item.gmarket.co.kr/Item?goodscode=2971758521&ver=20230531 ASUS공식총판
    49 [테팔]테팔 스팀다리미 무선다리미 프리무브 에어 FV6550 83,610원 http://item.gmarket.co.kr/Item?goodscode=2330377014&ver=20230531 테팔공식판매
    50 [샤오미]샤오미 미지아 무선 선풍기 5세대 에어서큘레이터 BLDC 마그네틱 충전 돼지코 증정 BPLDS05DM 113,300원 http://item.gmarket.co.kr/Item?goodscode=2887042390&ver=20230531 e직구-HK
    51 [다이슨]다이슨 에어랩 멀티 스타일러 컴플리트 롱 (코퍼/니켈) 749,000원 http://item.gmarket.co.kr/Item?goodscode=2935603979&ver=20230531 다이슨코리아유한회사
    52 [위닉스]공식파트너_위닉스 뽀송 제습기 16L (DN2H160-IWK) 집중건조킷 포함 341,490원 http://item.gmarket.co.kr/Item?goodscode=2879470884&ver=20230531 공식파트너위니텍
    53 [위닉스]공식인증점)위닉스 컴팩트 미니건조기 플러스 {HS2E400-MGK} 오가닉그린 (최대 4KG) (~5/31 카드가 29.9만) 331,500원 http://item.gmarket.co.kr/Item?goodscode=2548106026&ver=20230531 위닉스공식파트너두림
    54 갤럭시 S23 S22 울트라 노트20/노트10/S21/S20/S10/A53/A32/A23/A33 플러스 핸드폰 휴대폰 가죽케이스 19,900원 http://item.gmarket.co.kr/Item?goodscode=826268928&ver=20230531 꾹이몰
    55 [신일전자]신일 BLDC 무소음 리모컨 선풍기 SIF-DC826 123,000원 http://item.gmarket.co.kr/Item?goodscode=2841795103&ver=20230531 니코샵
    56 [캐리어]캐리어 10L 제습기 CDHM-C010LMOW 22년형 신제품 199,000원 http://item.gmarket.co.kr/Item?goodscode=2441792558&ver=20230531 이지스21매장
    57 [네고]2매+1 (이벤트참여) 강화유리 지문방지 갤럭시S23 S22 울트라 플러스 노트 플립 폴드 아이폰14 13 프로맥스 14,900원 http://item.gmarket.co.kr/Item?goodscode=637253436&ver=20230531 네고네고
    58 [LG전자]LG전자 DQ132PWXC 지역별운송료상이 (가인) 398,920원 http://item.gmarket.co.kr/Item?goodscode=2494906253&ver=20230531 가인리테일
    59 [필립스]최종13910원)PHILIPS 필립스 코털제거기 NT1620/15 콧털 정리 트리머 13,410원 http://item.gmarket.co.kr/Item?goodscode=1894866019&ver=20230531 (주)필립스코리아
    60 [삼성전자]삼성 비스포크 무풍에어컨 윈도우핏 베이지 AW06C7155TWA 969,000원 http://item.gmarket.co.kr/Item?goodscode=2958178600&ver=20230531 NS홈쇼핑
    61 [신일]신일 BLDC 에어서큘레이터 AIR S9 베이지 2세트(SIF-TS10RA) 255,960원 http://item.gmarket.co.kr/Item?goodscode=2962153901&ver=20230531 NS홈쇼핑
    62 [대우]대우 써큘레이터형 BLDC 에코팬 리모컨 선풍기 73,800원 http://item.gmarket.co.kr/Item?goodscode=1863410114&ver=20230531 스마일배송
    63 [엔보우]모노 탁상용 무선 휴대용 선풍기 2세대 1+1(할인 행사) 29,900원 http://item.gmarket.co.kr/Item?goodscode=1812271382&ver=20230531 엔보우
    64 [신일전자]프리미엄 선풍기 SIF-T14DC - 14인치 / 상하.좌우 3D입체회전 BLDC / 무소음 써큘레이터 159,000원 http://item.gmarket.co.kr/Item?goodscode=2893946602&ver=20230531 SK매직공식스토어
    65 [유닉스]유닉스 미니 웨이브 볼륨 뽕 멀티 고데기 UCI-B2304 7,900원 http://item.gmarket.co.kr/Item?goodscode=2610076235&ver=20230531 엘더스
    66 [아이엔픽]핸드폰 갤럭시S23울트라 S22플러스 S21 S20 S10 노트20 노트10 노트9 노트8 A34 A53 A23 A32 A31 가죽 지갑 9,900원 http://item.gmarket.co.kr/Item?goodscode=1418909600&ver=20230531 엘엘비
    67 [LG전자]DQ202PSUA o클릭o LG전자/제습기 505,490원 http://item.gmarket.co.kr/Item?goodscode=2454689996&ver=20230531 다락방가전쇼핑몰
    68 [닌텐도]닌텐도 스위치 젤다의 전설 티어스 오브 더 킹덤 일반판 +특전 74,800원 http://item.gmarket.co.kr/Item?goodscode=2847648865&ver=20230531 지피클럽
    69 [위닉스]공식인증점) 위닉스 NEW 타워엣지 공기청정기 {AT8E430-MWK} 화이트 224,730원 http://item.gmarket.co.kr/Item?goodscode=2882340090&ver=20230531 위닉스공식파트너두림
    70 [샤클라]샤클라 로봇 물걸레청소기 KGC001 199,000원 http://item.gmarket.co.kr/Item?goodscode=2225642928&ver=20230531 공식인증샵
    71 [삼성전자]{신규런칭} NEW 갤럭시 A34 자급제 128GB SM-A346N/쿠폰(5%)추가할인/무이자(최대12개월)/당일발송 464,440원 http://item.gmarket.co.kr/Item?goodscode=2861319917&ver=20230531 삼성전자공식총판점
    72 [쿠쿠]6인용 쿠쿠 고화력 밥솥 CRP-P0660FD+냉동용기 147,250원 http://item.gmarket.co.kr/Item?goodscode=2710412548&ver=20230531 CJ온스타일플러스
    73 [위니아]인증 위니아 가정용제습기 EDH8DNWB 8L 182,000원 http://item.gmarket.co.kr/Item?goodscode=1817247637&ver=20230531 WINIADIMCHAE
    74 [솔러스에어]탁상용 소형 미니 무소음 무선 휴대용 탁상 선풍기 AIR601TF 29,900원 http://item.gmarket.co.kr/Item?goodscode=2839674262&ver=20230531 에어로코리아
    75 [위닉스]{공식파트너} 위닉스 뽀송 제습기 10리터 DXAE100-JWK 215,690원 http://item.gmarket.co.kr/Item?goodscode=510161514&ver=20230531 공식파트너위니텍
    76 [제이엠더블유]JMW 터보 항공기 MG1800 PLUS 올화이트 55,550원 http://item.gmarket.co.kr/Item?goodscode=1564671373&ver=20230531 스마일배송
    77 [LG전자]LG전자 27ART10AKPL 스탠드형 스탠바이미 TV (H) 978,000원 http://item.gmarket.co.kr/Item?goodscode=2927118819&ver=20230531 호야라인
    78 [샤오미]샤오미 미지아 LED 모니터 조명 1s MJGJD02YL 스크린바 무선리모컨 45,900원 http://item.gmarket.co.kr/Item?goodscode=2866332287&ver=20230531 meetg9
    79 [미홀]샤오미 MIWHOLE 20L 제습기 / 미홀 제습기 / 무료배송 124,000원 http://item.gmarket.co.kr/Item?goodscode=2544070928&ver=20230531 홍콩리미티드
    80 모닝컴 리모컨 초미세풍 발터치스탠드선풍기 23년도 신상품 32,900원 http://item.gmarket.co.kr/Item?goodscode=2923134780&ver=20230531 lainzone
    81 [유닉스]20만개판매 유닉스 전문가용 드라이기 헤어드라이어 미용실 가정용 업소용 냉풍 찬바람 가성비 UN-A1770 41,940원 http://item.gmarket.co.kr/Item?goodscode=978590816&ver=20230531 레브커머스
    82 [쿠쿠](혜택가 17.6만원) 쿠쿠 본사직영 10인용 IH압력 밥솥 CRP-HWF1060FDM 206,110원 http://item.gmarket.co.kr/Item?goodscode=1624106044&ver=20230531 쿠쿠전자(주)
    83 [필립스]카드가 12만원대) PHILIPS 전기면도기 SkinIQ 5000 S5582/36 오션블루 122,550원 http://item.gmarket.co.kr/Item?goodscode=2065305475&ver=20230531 (주)필립스코리아
    84 [삼성전자]삼성전자 정품 마이크로SD EVO Plus 512GB MB-MC512KA 33,960원 http://item.gmarket.co.kr/Item?goodscode=2258073651&ver=20230531 에버랩스
    85 [한경희이지라이프]한경희 제습기 HE-D720 대용량 20L (최종혜택가 21.5만 5월 신제품) /예약판매 249,000원 http://item.gmarket.co.kr/Item?goodscode=2923177563&ver=20230531 인프라맥스
    86 [위닉스]공식인증점)위닉스 컴팩트 미니건조기 플러스 {HS2E400-MEK} 화이트베이지 (최대 4KG) (~5/31 카드 29.9만) 321,500원 http://item.gmarket.co.kr/Item?goodscode=2548097351&ver=20230531 위닉스공식파트너두림
    87 [캐리어]총알배송 전국무료배송 캐리어 고효율 1등급 20리터 제습기 386,130원 http://item.gmarket.co.kr/Item?goodscode=2880038660&ver=20230531 신세계몰
    88 [씽크웨이]씽크에어 DL12 제습기 138,570원 http://item.gmarket.co.kr/Item?goodscode=2151848191&ver=20230531 씽크웨이
    89 오피스 2021 Home  Student (홈앤스튜던트) PKC 106,000원 http://item.gmarket.co.kr/Item?goodscode=2428212761&ver=20230531 스마일배송
    90 핸드폰/갤럭시/S23/S22/노트20/S21/S20/노트10/플러스/노트9/8/A34/키즈/A32/울트라/점프/아이폰14프로맥스 9,800원 http://item.gmarket.co.kr/Item?goodscode=208542015&ver=20230531 컴퍼니제이
    91 [신일전자]신일 에어써큘레이터 화이트 SIF-FA800B 103,600원 http://item.gmarket.co.kr/Item?goodscode=2955811170&ver=20230531 CJ온스타일LIVE
    92 [뷰플]뷰플 갈바닉마사지기 얼굴리프팅 피부홈케어 EMS 딜링클 35,070원 http://item.gmarket.co.kr/Item?goodscode=2654791706&ver=20230531 (주)아워코퍼레이션
    93 [비쎌]비쎌 다용도습식청소기 스팟클린 3698V /본사직영 전용포뮬라포함 리뷰선물증정 179,000원 http://item.gmarket.co.kr/Item?goodscode=1807271089&ver=20230531 비쎌공식몰
    94 [삼성전자]삼성전자 DDR4 16G PC4-25600 (정품) -Mrc 22년 11주 46,280원 http://item.gmarket.co.kr/Item?goodscode=2827306317&ver=20230531 미라클솔루션
    95 아이폰14 13 프로 실리콘 케이스 애플 로고 아이폰12 프로 아이폰XS 커플 파스텔 폰케이스 11,900원 http://item.gmarket.co.kr/Item?goodscode=2498816088&ver=20230531 짜롱이케이스
    96 [로지텍]로지텍코리아 G102ic 2세대 LIGHTSYNC 마우스 23,900원 http://item.gmarket.co.kr/Item?goodscode=1828364877&ver=20230531 로지텍G공식스토어
    97 [신일전자]신일 스탠드형 리모컨 선풍기 SIF-S14RWS 86,000원 http://item.gmarket.co.kr/Item?goodscode=1373779506&ver=20230531 니코샵
    98 JCP1 Apple 애플 펜슬 2세대 (MU8F2KH/A)  오늘출발 내일도착 195,900원 http://item.gmarket.co.kr/Item?goodscode=2696387467&ver=20230531 제이씨엠컴퍼니
    99 [LG전자]수도권충청대구부산 기본설치포함 휘센 멀티 에어컨 FQ17HCKWC2 1,630,870원 http://item.gmarket.co.kr/Item?goodscode=2423169668&ver=20230531 네고네고
    100 [Lenovo]태블릿 p12 샤오신패드 2022 4+64g 10.6인치 WIFI 중국롬 167,000원 http://item.gmarket.co.kr/Item?goodscode=2580451792&ver=20230531 SMARTA



```python

```
