---
layout: single
title: "정규표현식을 활용한 데이터추출"
categories: python
tag: [crawling, regularexpression, jupyternotebook]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---





## 예제: 왜 정규표현식이 필요할까?

### 패턴화되어있는 것을 추출하기에 용이하다. 


```python
from urllib.request import urlopen
from bs4 import BeautifulSoup
import re

res = urlopen('https://davelee-fun.github.io/blog/crawl_test_css.html')
soup = BeautifulSoup(res, "html.parser")

data = soup.select('ul#dev_course_list li.course')
for item in data:
    print (item.get_text())
```

    (초급) - 강사가 실제 사용하는 자동 프로그램 소개 [2]
    (초급) - 필요한 프로그램 설치 시연 [5]
    (초급) - 데이터를 엑셀 파일로 만들기 [9]
    (초급) -     엑셀 파일 이쁘게! 이쁘게! [8]
    (초급) -     나대신 주기적으로 파이썬 프로그램 실행하기 [7]
    (초급) - 파이썬으로 슬랙(slack) 메신저에 글쓰기 [40]
    (초급) - 웹사이트 변경사항 주기적으로 체크해서, 메신저로 알람주기 [12]
    (초급) - 네이버 API 사용해서, 블로그에 글쓰기 [42]
    (중급) - 자동으로 쿠팡파트너스 API 로 가져온 상품 정보, 네이버 블로그/트위터에 홍보하기 [412]



```python
from urllib.request import urlopen
from bs4 import BeautifulSoup
import re

res = urlopen('https://davelee-fun.github.io/blog/crawl_test_css.html')
soup = BeautifulSoup(res, "html.parser")

data = soup.select('ul#dev_course_list li.course')
for item in data:
    print (re.sub('\[[0-9]+\]', '', item.get_text()) )
```

    (초급) - 강사가 실제 사용하는 자동 프로그램 소개 
    (초급) - 필요한 프로그램 설치 시연 
    (초급) - 데이터를 엑셀 파일로 만들기 
    (초급) -     엑셀 파일 이쁘게! 이쁘게! 
    (초급) -     나대신 주기적으로 파이썬 프로그램 실행하기 
    (초급) - 파이썬으로 슬랙(slack) 메신저에 글쓰기 
    (초급) - 웹사이트 변경사항 주기적으로 체크해서, 메신저로 알람주기 
    (초급) - 네이버 API 사용해서, 블로그에 글쓰기 
    (중급) - 자동으로 쿠팡파트너스 API 로 가져온 상품 정보, 네이버 블로그/트위터에 홍보하기 


## 정규표현식

<div class="alert alert-block alert-success">
<strong><font color="blue" size="4em">프로그래밍 연습(생각만 해보기)</font></strong><br>
다음 코드를 실행해보고, 이름 정보를 통해 남자인지, 여자인지, 기타인지(남녀 구별 불가)를 확인할 수 있는 방법 생각해보기
</div>
<pre>
import openpyxl

## 엑셀파일 열기
work_book = openpyxl.load_workbook('train.xlsx')

## 현재 Active Sheet 얻기
work_sheet = work_book.active

## work_sheet.rows는 해당 쉬트의 모든 행을 객체로 가지고 있음
for each_row in work_sheet.rows:
    print (each_row[3].value)
    
work_book.close()
</pre>

### 세 라인의 코드를 추가하면 이름의 특징을 추출할 수 있음
<pre>
import re
regex = re.compile('[A-Za-z]+\.')
print ( regex.findall('찾을 문자열') )
</pre>


```python
import openpyxl
import re

regex = re.compile(' [A-Za-z]+\.')
# 엑셀파일 열기
work_book = openpyxl.load_workbook('train.xlsx')

# 현재 Active Sheet 얻기
work_sheet = work_book.active

# work_sheet.rows는 해당 쉬트의 모든 행을 객체로 가지고 있음
for each_row in work_sheet.rows:
    print (each_row[3].value)
    print (regex.findall(each_row[3].value))    

work_book.close()
```

    Name
    []
    Braund, Mr. Owen Harris
    [' Mr.']
    Cumings, Mrs. John Bradley (Florence Briggs Thayer)
    [' Mrs.']
    Heikkinen, Miss. Laina
    [' Miss.']
    Futrelle, Mrs. Jacques Heath (Lily May Peel)
    [' Mrs.']
    Allen, Mr. William Henry
    [' Mr.']
    Moran, Mr. James
    [' Mr.']
    McCarthy, Mr. Timothy J
    [' Mr.']
    Palsson, Master. Gosta Leonard
    [' Master.']
    Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)
    [' Mrs.']
    Nasser, Mrs. Nicholas (Adele Achem)
    [' Mrs.']
    Sandstrom, Miss. Marguerite Rut
    [' Miss.']
    Bonnell, Miss. Elizabeth
    [' Miss.']
    Saundercock, Mr. William Henry
    [' Mr.']
    Andersson, Mr. Anders Johan
    [' Mr.']
    Vestrom, Miss. Hulda Amanda Adolfina
    [' Miss.']
    Hewlett, Mrs. (Mary D Kingcome) 
    [' Mrs.']
    Rice, Master. Eugene
    [' Master.']
    Williams, Mr. Charles Eugene
    [' Mr.']
    Vander Planke, Mrs. Julius (Emelia Maria Vandemoortele)
    [' Mrs.']
    Masselmani, Mrs. Fatima
    [' Mrs.']
    Fynney, Mr. Joseph J
    [' Mr.']
    Beesley, Mr. Lawrence
    [' Mr.']
    McGowan, Miss. Anna "Annie"
    [' Miss.']
    Sloper, Mr. William Thompson
    [' Mr.']
    Palsson, Miss. Torborg Danira
    [' Miss.']
    Asplund, Mrs. Carl Oscar (Selma Augusta Emilia Johansson)
    [' Mrs.']
    Emir, Mr. Farred Chehab
    [' Mr.']
    Fortune, Mr. Charles Alexander
    [' Mr.']
    O'Dwyer, Miss. Ellen "Nellie"
    [' Miss.']
    Todoroff, Mr. Lalio
    [' Mr.']
    Uruchurtu, Don. Manuel E
    [' Don.']
    Spencer, Mrs. William Augustus (Marie Eugenie)
    [' Mrs.']
    Glynn, Miss. Mary Agatha
    [' Miss.']
    Wheadon, Mr. Edward H
    [' Mr.']
    Meyer, Mr. Edgar Joseph
    [' Mr.']
    Holverson, Mr. Alexander Oskar
    [' Mr.']
    Mamee, Mr. Hanna
    [' Mr.']
    Cann, Mr. Ernest Charles
    [' Mr.']
    Vander Planke, Miss. Augusta Maria
    [' Miss.']
    Nicola-Yarred, Miss. Jamila
    [' Miss.']
    Ahlin, Mrs. Johan (Johanna Persdotter Larsson)
    [' Mrs.']
    Turpin, Mrs. William John Robert (Dorothy Ann Wonnacott)
    [' Mrs.']
    Kraeff, Mr. Theodor
    [' Mr.']
    Laroche, Miss. Simonne Marie Anne Andree
    [' Miss.']
    Devaney, Miss. Margaret Delia
    [' Miss.']
    Rogers, Mr. William John
    [' Mr.']
    Lennon, Mr. Denis
    [' Mr.']
    O'Driscoll, Miss. Bridget
    [' Miss.']
    Samaan, Mr. Youssef
    [' Mr.']
    Arnold-Franchi, Mrs. Josef (Josefine Franchi)
    [' Mrs.']
    Panula, Master. Juha Niilo
    [' Master.']
    Nosworthy, Mr. Richard Cater
    [' Mr.']
    Harper, Mrs. Henry Sleeper (Myna Haxtun)
    [' Mrs.']
    Faunthorpe, Mrs. Lizzie (Elizabeth Anne Wilkinson)
    [' Mrs.']
    Ostby, Mr. Engelhart Cornelius
    [' Mr.']
    Woolner, Mr. Hugh
    [' Mr.']
    Rugg, Miss. Emily
    [' Miss.']
    Novel, Mr. Mansouer
    [' Mr.']
    West, Miss. Constance Mirium
    [' Miss.']
    Goodwin, Master. William Frederick
    [' Master.']
    Sirayanian, Mr. Orsen
    [' Mr.']
    Icard, Miss. Amelie
    [' Miss.']
    Harris, Mr. Henry Birkhardt
    [' Mr.']
    Skoog, Master. Harald
    [' Master.']
    Stewart, Mr. Albert A
    [' Mr.']
    Moubarek, Master. Gerios
    [' Master.']
    Nye, Mrs. (Elizabeth Ramell)
    [' Mrs.']
    Crease, Mr. Ernest James
    [' Mr.']
    Andersson, Miss. Erna Alexandra
    [' Miss.']
    Kink, Mr. Vincenz
    [' Mr.']
    Jenkin, Mr. Stephen Curnow
    [' Mr.']
    Goodwin, Miss. Lillian Amy
    [' Miss.']
    Hood, Mr. Ambrose Jr
    [' Mr.']
    Chronopoulos, Mr. Apostolos
    [' Mr.']
    Bing, Mr. Lee
    [' Mr.']
    Moen, Mr. Sigurd Hansen
    [' Mr.']
    Staneff, Mr. Ivan
    [' Mr.']
    Moutal, Mr. Rahamin Haim
    [' Mr.']
    Caldwell, Master. Alden Gates
    [' Master.']
    Dowdell, Miss. Elizabeth
    [' Miss.']
    Waelens, Mr. Achille
    [' Mr.']
    Sheerlinck, Mr. Jan Baptist
    [' Mr.']
    McDermott, Miss. Brigdet Delia
    [' Miss.']
    Carrau, Mr. Francisco M
    [' Mr.']
    Ilett, Miss. Bertha
    [' Miss.']
    Backstrom, Mrs. Karl Alfred (Maria Mathilda Gustafsson)
    [' Mrs.']
    Ford, Mr. William Neal
    [' Mr.']
    Slocovski, Mr. Selman Francis
    [' Mr.']
    Fortune, Miss. Mabel Helen
    [' Miss.']
    Celotti, Mr. Francesco
    [' Mr.']
    Christmann, Mr. Emil
    [' Mr.']
    Andreasson, Mr. Paul Edvin
    [' Mr.']
    Chaffee, Mr. Herbert Fuller
    [' Mr.']
    Dean, Mr. Bertram Frank
    [' Mr.']
    Coxon, Mr. Daniel
    [' Mr.']
    Shorney, Mr. Charles Joseph
    [' Mr.']
    Goldschmidt, Mr. George B
    [' Mr.']
    Greenfield, Mr. William Bertram
    [' Mr.']
    Doling, Mrs. John T (Ada Julia Bone)
    [' Mrs.']
    Kantor, Mr. Sinai
    [' Mr.']
    Petranec, Miss. Matilda
    [' Miss.']
    Petroff, Mr. Pastcho ("Pentcho")
    [' Mr.']
    White, Mr. Richard Frasar
    [' Mr.']
    Johansson, Mr. Gustaf Joel
    [' Mr.']
    Gustafsson, Mr. Anders Vilhelm
    [' Mr.']
    Mionoff, Mr. Stoytcho
    [' Mr.']
    Salkjelsvik, Miss. Anna Kristine
    [' Miss.']
    Moss, Mr. Albert Johan
    [' Mr.']
    Rekic, Mr. Tido
    [' Mr.']
    Moran, Miss. Bertha
    [' Miss.']
    Porter, Mr. Walter Chamberlain
    [' Mr.']
    Zabour, Miss. Hileni
    [' Miss.']
    Barton, Mr. David John
    [' Mr.']
    Jussila, Miss. Katriina
    [' Miss.']
    Attalah, Miss. Malake
    [' Miss.']
    Pekoniemi, Mr. Edvard
    [' Mr.']
    Connors, Mr. Patrick
    [' Mr.']
    Turpin, Mr. William John Robert
    [' Mr.']
    Baxter, Mr. Quigg Edmond
    [' Mr.']
    Andersson, Miss. Ellis Anna Maria
    [' Miss.']
    Hickman, Mr. Stanley George
    [' Mr.']
    Moore, Mr. Leonard Charles
    [' Mr.']
    Nasser, Mr. Nicholas
    [' Mr.']
    Webber, Miss. Susan
    [' Miss.']
    White, Mr. Percival Wayland
    [' Mr.']
    Nicola-Yarred, Master. Elias
    [' Master.']
    McMahon, Mr. Martin
    [' Mr.']
    Madsen, Mr. Fridtjof Arne
    [' Mr.']
    Peter, Miss. Anna
    [' Miss.']
    Ekstrom, Mr. Johan
    [' Mr.']
    Drazenoic, Mr. Jozef
    [' Mr.']
    Coelho, Mr. Domingos Fernandeo
    [' Mr.']
    Robins, Mrs. Alexander A (Grace Charity Laury)
    [' Mrs.']
    Weisz, Mrs. Leopold (Mathilde Francoise Pede)
    [' Mrs.']
    Sobey, Mr. Samuel James Hayden
    [' Mr.']
    Richard, Mr. Emile
    [' Mr.']
    Newsom, Miss. Helen Monypeny
    [' Miss.']
    Futrelle, Mr. Jacques Heath
    [' Mr.']
    Osen, Mr. Olaf Elon
    [' Mr.']
    Giglio, Mr. Victor
    [' Mr.']
    Boulos, Mrs. Joseph (Sultana)
    [' Mrs.']
    Nysten, Miss. Anna Sofia
    [' Miss.']
    Hakkarainen, Mrs. Pekka Pietari (Elin Matilda Dolck)
    [' Mrs.']
    Burke, Mr. Jeremiah
    [' Mr.']
    Andrew, Mr. Edgardo Samuel
    [' Mr.']
    Nicholls, Mr. Joseph Charles
    [' Mr.']
    Andersson, Mr. August Edvard ("Wennerstrom")
    [' Mr.']
    Ford, Miss. Robina Maggie "Ruby"
    [' Miss.']
    Navratil, Mr. Michel ("Louis M Hoffman")
    [' Mr.']
    Byles, Rev. Thomas Roussel Davids
    [' Rev.']
    Bateman, Rev. Robert James
    [' Rev.']
    Pears, Mrs. Thomas (Edith Wearne)
    [' Mrs.']
    Meo, Mr. Alfonzo
    [' Mr.']
    van Billiard, Mr. Austin Blyler
    [' Mr.']
    Olsen, Mr. Ole Martin
    [' Mr.']
    Williams, Mr. Charles Duane
    [' Mr.']
    Gilnagh, Miss. Katherine "Katie"
    [' Miss.']
    Corn, Mr. Harry
    [' Mr.']
    Smiljanic, Mr. Mile
    [' Mr.']
    Sage, Master. Thomas Henry
    [' Master.']
    Cribb, Mr. John Hatfield
    [' Mr.']
    Watt, Mrs. James (Elizabeth "Bessie" Inglis Milne)
    [' Mrs.']
    Bengtsson, Mr. John Viktor
    [' Mr.']
    Calic, Mr. Jovo
    [' Mr.']
    Panula, Master. Eino Viljami
    [' Master.']
    Goldsmith, Master. Frank John William "Frankie"
    [' Master.']
    Chibnall, Mrs. (Edith Martha Bowerman)
    [' Mrs.']
    Skoog, Mrs. William (Anna Bernhardina Karlsson)
    [' Mrs.']
    Baumann, Mr. John D
    [' Mr.']
    Ling, Mr. Lee
    [' Mr.']
    Van der hoef, Mr. Wyckoff
    [' Mr.']
    Rice, Master. Arthur
    [' Master.']
    Johnson, Miss. Eleanor Ileen
    [' Miss.']
    Sivola, Mr. Antti Wilhelm
    [' Mr.']
    Smith, Mr. James Clinch
    [' Mr.']
    Klasen, Mr. Klas Albin
    [' Mr.']
    Lefebre, Master. Henry Forbes
    [' Master.']
    Isham, Miss. Ann Elizabeth
    [' Miss.']
    Hale, Mr. Reginald
    [' Mr.']
    Leonard, Mr. Lionel
    [' Mr.']
    Sage, Miss. Constance Gladys
    [' Miss.']
    Pernot, Mr. Rene
    [' Mr.']
    Asplund, Master. Clarence Gustaf Hugo
    [' Master.']
    Becker, Master. Richard F
    [' Master.']
    Kink-Heilmann, Miss. Luise Gretchen
    [' Miss.']
    Rood, Mr. Hugh Roscoe
    [' Mr.']
    O'Brien, Mrs. Thomas (Johanna "Hannah" Godfrey)
    [' Mrs.']
    Romaine, Mr. Charles Hallace ("Mr C Rolmane")
    [' Mr.']
    Bourke, Mr. John
    [' Mr.']
    Turcin, Mr. Stjepan
    [' Mr.']
    Pinsky, Mrs. (Rosa)
    [' Mrs.']
    Carbines, Mr. William
    [' Mr.']
    Andersen-Jensen, Miss. Carla Christine Nielsine
    [' Miss.']
    Navratil, Master. Michel M
    [' Master.']
    Brown, Mrs. James Joseph (Margaret Tobin)
    [' Mrs.']
    Lurette, Miss. Elise
    [' Miss.']
    Mernagh, Mr. Robert
    [' Mr.']
    Olsen, Mr. Karl Siegwart Andreas
    [' Mr.']
    Madigan, Miss. Margaret "Maggie"
    [' Miss.']
    Yrois, Miss. Henriette ("Mrs Harbeck")
    [' Miss.']
    Vande Walle, Mr. Nestor Cyriel
    [' Mr.']
    Sage, Mr. Frederick
    [' Mr.']
    Johanson, Mr. Jakob Alfred
    [' Mr.']
    Youseff, Mr. Gerious
    [' Mr.']
    Cohen, Mr. Gurshon "Gus"
    [' Mr.']
    Strom, Miss. Telma Matilda
    [' Miss.']
    Backstrom, Mr. Karl Alfred
    [' Mr.']
    Albimona, Mr. Nassef Cassem
    [' Mr.']
    Carr, Miss. Helen "Ellen"
    [' Miss.']
    Blank, Mr. Henry
    [' Mr.']
    Ali, Mr. Ahmed
    [' Mr.']
    Cameron, Miss. Clear Annie
    [' Miss.']
    Perkin, Mr. John Henry
    [' Mr.']
    Givard, Mr. Hans Kristensen
    [' Mr.']
    Kiernan, Mr. Philip
    [' Mr.']
    Newell, Miss. Madeleine
    [' Miss.']
    Honkanen, Miss. Eliina
    [' Miss.']
    Jacobsohn, Mr. Sidney Samuel
    [' Mr.']
    Bazzani, Miss. Albina
    [' Miss.']
    Harris, Mr. Walter
    [' Mr.']
    Sunderland, Mr. Victor Francis
    [' Mr.']
    Bracken, Mr. James H
    [' Mr.']
    Green, Mr. George Henry
    [' Mr.']
    Nenkoff, Mr. Christo
    [' Mr.']
    Hoyt, Mr. Frederick Maxfield
    [' Mr.']
    Berglund, Mr. Karl Ivar Sven
    [' Mr.']
    Mellors, Mr. William John
    [' Mr.']
    Lovell, Mr. John Hall ("Henry")
    [' Mr.']
    Fahlstrom, Mr. Arne Jonas
    [' Mr.']
    Lefebre, Miss. Mathilde
    [' Miss.']
    Harris, Mrs. Henry Birkhardt (Irene Wallach)
    [' Mrs.']
    Larsson, Mr. Bengt Edvin
    [' Mr.']
    Sjostedt, Mr. Ernst Adolf
    [' Mr.']
    Asplund, Miss. Lillian Gertrud
    [' Miss.']
    Leyson, Mr. Robert William Norman
    [' Mr.']
    Harknett, Miss. Alice Phoebe
    [' Miss.']
    Hold, Mr. Stephen
    [' Mr.']
    Collyer, Miss. Marjorie "Lottie"
    [' Miss.']
    Pengelly, Mr. Frederick William
    [' Mr.']
    Hunt, Mr. George Henry
    [' Mr.']
    Zabour, Miss. Thamine
    [' Miss.']
    Murphy, Miss. Katherine "Kate"
    [' Miss.']
    Coleridge, Mr. Reginald Charles
    [' Mr.']
    Maenpaa, Mr. Matti Alexanteri
    [' Mr.']
    Attalah, Mr. Sleiman
    [' Mr.']
    Minahan, Dr. William Edward
    [' Dr.']
    Lindahl, Miss. Agda Thorilda Viktoria
    [' Miss.']
    Hamalainen, Mrs. William (Anna)
    [' Mrs.']
    Beckwith, Mr. Richard Leonard
    [' Mr.']
    Carter, Rev. Ernest Courtenay
    [' Rev.']
    Reed, Mr. James George
    [' Mr.']
    Strom, Mrs. Wilhelm (Elna Matilda Persson)
    [' Mrs.']
    Stead, Mr. William Thomas
    [' Mr.']
    Lobb, Mr. William Arthur
    [' Mr.']
    Rosblom, Mrs. Viktor (Helena Wilhelmina)
    [' Mrs.']
    Touma, Mrs. Darwis (Hanne Youssef Razi)
    [' Mrs.']
    Thorne, Mrs. Gertrude Maybelle
    [' Mrs.']
    Cherry, Miss. Gladys
    [' Miss.']
    Ward, Miss. Anna
    [' Miss.']
    Parrish, Mrs. (Lutie Davis)
    [' Mrs.']
    Smith, Mr. Thomas
    [' Mr.']
    Asplund, Master. Edvin Rojj Felix
    [' Master.']
    Taussig, Mr. Emil
    [' Mr.']
    Harrison, Mr. William
    [' Mr.']
    Henry, Miss. Delia
    [' Miss.']
    Reeves, Mr. David
    [' Mr.']
    Panula, Mr. Ernesti Arvid
    [' Mr.']
    Persson, Mr. Ernst Ulrik
    [' Mr.']
    Graham, Mrs. William Thompson (Edith Junkins)
    [' Mrs.']
    Bissette, Miss. Amelia
    [' Miss.']
    Cairns, Mr. Alexander
    [' Mr.']
    Tornquist, Mr. William Henry
    [' Mr.']
    Mellinger, Mrs. (Elizabeth Anne Maidment)
    [' Mrs.']
    Natsch, Mr. Charles H
    [' Mr.']
    Healy, Miss. Hanora "Nora"
    [' Miss.']
    Andrews, Miss. Kornelia Theodosia
    [' Miss.']
    Lindblom, Miss. Augusta Charlotta
    [' Miss.']
    Parkes, Mr. Francis "Frank"
    [' Mr.']
    Rice, Master. Eric
    [' Master.']
    Abbott, Mrs. Stanton (Rosa Hunt)
    [' Mrs.']
    Duane, Mr. Frank
    [' Mr.']
    Olsson, Mr. Nils Johan Goransson
    [' Mr.']
    de Pelsmaeker, Mr. Alfons
    [' Mr.']
    Dorking, Mr. Edward Arthur
    [' Mr.']
    Smith, Mr. Richard William
    [' Mr.']
    Stankovic, Mr. Ivan
    [' Mr.']
    de Mulder, Mr. Theodore
    [' Mr.']
    Naidenoff, Mr. Penko
    [' Mr.']
    Hosono, Mr. Masabumi
    [' Mr.']
    Connolly, Miss. Kate
    [' Miss.']
    Barber, Miss. Ellen "Nellie"
    [' Miss.']
    Bishop, Mrs. Dickinson H (Helen Walton)
    [' Mrs.']
    Levy, Mr. Rene Jacques
    [' Mr.']
    Haas, Miss. Aloisia
    [' Miss.']
    Mineff, Mr. Ivan
    [' Mr.']
    Lewy, Mr. Ervin G
    [' Mr.']
    Hanna, Mr. Mansour
    [' Mr.']
    Allison, Miss. Helen Loraine
    [' Miss.']
    Saalfeld, Mr. Adolphe
    [' Mr.']
    Baxter, Mrs. James (Helene DeLaudeniere Chaput)
    [' Mrs.']
    Kelly, Miss. Anna Katherine "Annie Kate"
    [' Miss.']
    McCoy, Mr. Bernard
    [' Mr.']
    Johnson, Mr. William Cahoone Jr
    [' Mr.']
    Keane, Miss. Nora A
    [' Miss.']
    Williams, Mr. Howard Hugh "Harry"
    [' Mr.']
    Allison, Master. Hudson Trevor
    [' Master.']
    Fleming, Miss. Margaret
    [' Miss.']
    Penasco y Castellana, Mrs. Victor de Satode (Maria Josefa Perez de Soto y Vallejo)
    [' Mrs.']
    Abelson, Mr. Samuel
    [' Mr.']
    Francatelli, Miss. Laura Mabel
    [' Miss.']
    Hays, Miss. Margaret Bechstein
    [' Miss.']
    Ryerson, Miss. Emily Borie
    [' Miss.']
    Lahtinen, Mrs. William (Anna Sylfven)
    [' Mrs.']
    Hendekovic, Mr. Ignjac
    [' Mr.']
    Hart, Mr. Benjamin
    [' Mr.']
    Nilsson, Miss. Helmina Josefina
    [' Miss.']
    Kantor, Mrs. Sinai (Miriam Sternin)
    [' Mrs.']
    Moraweck, Dr. Ernest
    [' Dr.']
    Wick, Miss. Mary Natalie
    [' Miss.']
    Spedden, Mrs. Frederic Oakley (Margaretta Corning Stone)
    [' Mrs.']
    Dennis, Mr. Samuel
    [' Mr.']
    Danoff, Mr. Yoto
    [' Mr.']
    Slayter, Miss. Hilda Mary
    [' Miss.']
    Caldwell, Mrs. Albert Francis (Sylvia Mae Harbaugh)
    [' Mrs.']
    Sage, Mr. George John Jr
    [' Mr.']
    Young, Miss. Marie Grice
    [' Miss.']
    Nysveen, Mr. Johan Hansen
    [' Mr.']
    Ball, Mrs. (Ada E Hall)
    [' Mrs.']
    Goldsmith, Mrs. Frank John (Emily Alice Brown)
    [' Mrs.']
    Hippach, Miss. Jean Gertrude
    [' Miss.']
    McCoy, Miss. Agnes
    [' Miss.']
    Partner, Mr. Austen
    [' Mr.']
    Graham, Mr. George Edward
    [' Mr.']
    Vander Planke, Mr. Leo Edmondus
    [' Mr.']
    Frauenthal, Mrs. Henry William (Clara Heinsheimer)
    [' Mrs.']
    Denkoff, Mr. Mitto
    [' Mr.']
    Pears, Mr. Thomas Clinton
    [' Mr.']
    Burns, Miss. Elizabeth Margaret
    [' Miss.']
    Dahl, Mr. Karl Edwart
    [' Mr.']
    Blackwell, Mr. Stephen Weart
    [' Mr.']
    Navratil, Master. Edmond Roger
    [' Master.']
    Fortune, Miss. Alice Elizabeth
    [' Miss.']
    Collander, Mr. Erik Gustaf
    [' Mr.']
    Sedgwick, Mr. Charles Frederick Waddington
    [' Mr.']
    Fox, Mr. Stanley Hubert
    [' Mr.']
    Brown, Miss. Amelia "Mildred"
    [' Miss.']
    Smith, Miss. Marion Elsie
    [' Miss.']
    Davison, Mrs. Thomas Henry (Mary E Finck)
    [' Mrs.']
    Coutts, Master. William Loch "William"
    [' Master.']
    Dimic, Mr. Jovan
    [' Mr.']
    Odahl, Mr. Nils Martin
    [' Mr.']
    Williams-Lambert, Mr. Fletcher Fellows
    [' Mr.']
    Elias, Mr. Tannous
    [' Mr.']
    Arnold-Franchi, Mr. Josef
    [' Mr.']
    Yousif, Mr. Wazli
    [' Mr.']
    Vanden Steen, Mr. Leo Peter
    [' Mr.']
    Bowerman, Miss. Elsie Edith
    [' Miss.']
    Funk, Miss. Annie Clemmer
    [' Miss.']
    McGovern, Miss. Mary
    [' Miss.']
    Mockler, Miss. Helen Mary "Ellie"
    [' Miss.']
    Skoog, Mr. Wilhelm
    [' Mr.']
    del Carlo, Mr. Sebastiano
    [' Mr.']
    Barbara, Mrs. (Catherine David)
    [' Mrs.']
    Asim, Mr. Adola
    [' Mr.']
    O'Brien, Mr. Thomas
    [' Mr.']
    Adahl, Mr. Mauritz Nils Martin
    [' Mr.']
    Warren, Mrs. Frank Manley (Anna Sophia Atkinson)
    [' Mrs.']
    Moussa, Mrs. (Mantoura Boulos)
    [' Mrs.']
    Jermyn, Miss. Annie
    [' Miss.']
    Aubart, Mme. Leontine Pauline
    [' Mme.']
    Harder, Mr. George Achilles
    [' Mr.']
    Wiklund, Mr. Jakob Alfred
    [' Mr.']
    Beavan, Mr. William Thomas
    [' Mr.']
    Ringhini, Mr. Sante
    [' Mr.']
    Palsson, Miss. Stina Viola
    [' Miss.']
    Meyer, Mrs. Edgar Joseph (Leila Saks)
    [' Mrs.']
    Landergren, Miss. Aurora Adelia
    [' Miss.']
    Widener, Mr. Harry Elkins
    [' Mr.']
    Betros, Mr. Tannous
    [' Mr.']
    Gustafsson, Mr. Karl Gideon
    [' Mr.']
    Bidois, Miss. Rosalie
    [' Miss.']
    Nakid, Miss. Maria ("Mary")
    [' Miss.']
    Tikkanen, Mr. Juho
    [' Mr.']
    Holverson, Mrs. Alexander Oskar (Mary Aline Towner)
    [' Mrs.']
    Plotcharsky, Mr. Vasil
    [' Mr.']
    Davies, Mr. Charles Henry
    [' Mr.']
    Goodwin, Master. Sidney Leonard
    [' Master.']
    Buss, Miss. Kate
    [' Miss.']
    Sadlier, Mr. Matthew
    [' Mr.']
    Lehmann, Miss. Bertha
    [' Miss.']
    Carter, Mr. William Ernest
    [' Mr.']
    Jansson, Mr. Carl Olof
    [' Mr.']
    Gustafsson, Mr. Johan Birger
    [' Mr.']
    Newell, Miss. Marjorie
    [' Miss.']
    Sandstrom, Mrs. Hjalmar (Agnes Charlotta Bengtsson)
    [' Mrs.']
    Johansson, Mr. Erik
    [' Mr.']
    Olsson, Miss. Elina
    [' Miss.']
    McKane, Mr. Peter David
    [' Mr.']
    Pain, Dr. Alfred
    [' Dr.']
    Trout, Mrs. William H (Jessie L)
    [' Mrs.']
    Niskanen, Mr. Juha
    [' Mr.']
    Adams, Mr. John
    [' Mr.']
    Jussila, Miss. Mari Aina
    [' Miss.']
    Hakkarainen, Mr. Pekka Pietari
    [' Mr.']
    Oreskovic, Miss. Marija
    [' Miss.']
    Gale, Mr. Shadrach
    [' Mr.']
    Widegren, Mr. Carl/Charles Peter
    [' Mr.']
    Richards, Master. William Rowe
    [' Master.']
    Birkeland, Mr. Hans Martin Monsen
    [' Mr.']
    Lefebre, Miss. Ida
    [' Miss.']
    Sdycoff, Mr. Todor
    [' Mr.']
    Hart, Mr. Henry
    [' Mr.']
    Minahan, Miss. Daisy E
    [' Miss.']
    Cunningham, Mr. Alfred Fleming
    [' Mr.']
    Sundman, Mr. Johan Julian
    [' Mr.']
    Meek, Mrs. Thomas (Annie Louise Rowley)
    [' Mrs.']
    Drew, Mrs. James Vivian (Lulu Thorne Christian)
    [' Mrs.']
    Silven, Miss. Lyyli Karoliina
    [' Miss.']
    Matthews, Mr. William John
    [' Mr.']
    Van Impe, Miss. Catharina
    [' Miss.']
    Gheorgheff, Mr. Stanio
    [' Mr.']
    Charters, Mr. David
    [' Mr.']
    Zimmerman, Mr. Leo
    [' Mr.']
    Danbom, Mrs. Ernst Gilbert (Anna Sigrid Maria Brogren)
    [' Mrs.']
    Rosblom, Mr. Viktor Richard
    [' Mr.']
    Wiseman, Mr. Phillippe
    [' Mr.']
    Clarke, Mrs. Charles V (Ada Maria Winfield)
    [' Mrs.']
    Phillips, Miss. Kate Florence ("Mrs Kate Louise Phillips Marshall")
    [' Miss.']
    Flynn, Mr. James
    [' Mr.']
    Pickard, Mr. Berk (Berk Trembisky)
    [' Mr.']
    Bjornstrom-Steffansson, Mr. Mauritz Hakan
    [' Mr.']
    Thorneycroft, Mrs. Percival (Florence Kate White)
    [' Mrs.']
    Louch, Mrs. Charles Alexander (Alice Adelaide Slow)
    [' Mrs.']
    Kallio, Mr. Nikolai Erland
    [' Mr.']
    Silvey, Mr. William Baird
    [' Mr.']
    Carter, Miss. Lucile Polk
    [' Miss.']
    Ford, Miss. Doolina Margaret "Daisy"
    [' Miss.']
    Richards, Mrs. Sidney (Emily Hocking)
    [' Mrs.']
    Fortune, Mr. Mark
    [' Mr.']
    Kvillner, Mr. Johan Henrik Johannesson
    [' Mr.']
    Hart, Mrs. Benjamin (Esther Ada Bloomfield)
    [' Mrs.']
    Hampe, Mr. Leon
    [' Mr.']
    Petterson, Mr. Johan Emil
    [' Mr.']
    Reynaldo, Ms. Encarnacion
    [' Ms.']
    Johannesen-Bratthammer, Mr. Bernt
    [' Mr.']
    Dodge, Master. Washington
    [' Master.']
    Mellinger, Miss. Madeleine Violet
    [' Miss.']
    Seward, Mr. Frederic Kimber
    [' Mr.']
    Baclini, Miss. Marie Catherine
    [' Miss.']
    Peuchen, Major. Arthur Godfrey
    [' Major.']
    West, Mr. Edwy Arthur
    [' Mr.']
    Hagland, Mr. Ingvald Olai Olsen
    [' Mr.']
    Foreman, Mr. Benjamin Laventall
    [' Mr.']
    Goldenberg, Mr. Samuel L
    [' Mr.']
    Peduzzi, Mr. Joseph
    [' Mr.']
    Jalsevac, Mr. Ivan
    [' Mr.']
    Millet, Mr. Francis Davis
    [' Mr.']
    Kenyon, Mrs. Frederick R (Marion)
    [' Mrs.']
    Toomey, Miss. Ellen
    [' Miss.']
    O'Connor, Mr. Maurice
    [' Mr.']
    Anderson, Mr. Harry
    [' Mr.']
    Morley, Mr. William
    [' Mr.']
    Gee, Mr. Arthur H
    [' Mr.']
    Milling, Mr. Jacob Christian
    [' Mr.']
    Maisner, Mr. Simon
    [' Mr.']
    Goncalves, Mr. Manuel Estanslas
    [' Mr.']
    Campbell, Mr. William
    [' Mr.']
    Smart, Mr. John Montgomery
    [' Mr.']
    Scanlan, Mr. James
    [' Mr.']
    Baclini, Miss. Helene Barbara
    [' Miss.']
    Keefe, Mr. Arthur
    [' Mr.']
    Cacic, Mr. Luka
    [' Mr.']
    West, Mrs. Edwy Arthur (Ada Mary Worth)
    [' Mrs.']
    Jerwan, Mrs. Amin S (Marie Marthe Thuillard)
    [' Mrs.']
    Strandberg, Miss. Ida Sofia
    [' Miss.']
    Clifford, Mr. George Quincy
    [' Mr.']
    Renouf, Mr. Peter Henry
    [' Mr.']
    Braund, Mr. Lewis Richard
    [' Mr.']
    Karlsson, Mr. Nils August
    [' Mr.']
    Hirvonen, Miss. Hildur E
    [' Miss.']
    Goodwin, Master. Harold Victor
    [' Master.']
    Frost, Mr. Anthony Wood "Archie"
    [' Mr.']
    Rouse, Mr. Richard Henry
    [' Mr.']
    Turkula, Mrs. (Hedwig)
    [' Mrs.']
    Bishop, Mr. Dickinson H
    [' Mr.']
    Lefebre, Miss. Jeannie
    [' Miss.']
    Hoyt, Mrs. Frederick Maxfield (Jane Anne Forby)
    [' Mrs.']
    Kent, Mr. Edward Austin
    [' Mr.']
    Somerton, Mr. Francis William
    [' Mr.']
    Coutts, Master. Eden Leslie "Neville"
    [' Master.']
    Hagland, Mr. Konrad Mathias Reiersen
    [' Mr.']
    Windelov, Mr. Einar
    [' Mr.']
    Molson, Mr. Harry Markland
    [' Mr.']
    Artagaveytia, Mr. Ramon
    [' Mr.']
    Stanley, Mr. Edward Roland
    [' Mr.']
    Yousseff, Mr. Gerious
    [' Mr.']
    Eustis, Miss. Elizabeth Mussey
    [' Miss.']
    Shellard, Mr. Frederick William
    [' Mr.']
    Allison, Mrs. Hudson J C (Bessie Waldo Daniels)
    [' Mrs.']
    Svensson, Mr. Olof
    [' Mr.']
    Calic, Mr. Petar
    [' Mr.']
    Canavan, Miss. Mary
    [' Miss.']
    O'Sullivan, Miss. Bridget Mary
    [' Miss.']
    Laitinen, Miss. Kristina Sofia
    [' Miss.']
    Maioni, Miss. Roberta
    [' Miss.']
    Penasco y Castellana, Mr. Victor de Satode
    [' Mr.']
    Quick, Mrs. Frederick Charles (Jane Richards)
    [' Mrs.']
    Bradley, Mr. George ("George Arthur Brayton")
    [' Mr.']
    Olsen, Mr. Henry Margido
    [' Mr.']
    Lang, Mr. Fang
    [' Mr.']
    Daly, Mr. Eugene Patrick
    [' Mr.']
    Webber, Mr. James
    [' Mr.']
    McGough, Mr. James Robert
    [' Mr.']
    Rothschild, Mrs. Martin (Elizabeth L. Barrett)
    [' Mrs.', ' L.']
    Coleff, Mr. Satio
    [' Mr.']
    Walker, Mr. William Anderson
    [' Mr.']
    Lemore, Mrs. (Amelia Milley)
    [' Mrs.']
    Ryan, Mr. Patrick
    [' Mr.']
    Angle, Mrs. William A (Florence "Mary" Agnes Hughes)
    [' Mrs.']
    Pavlovic, Mr. Stefo
    [' Mr.']
    Perreault, Miss. Anne
    [' Miss.']
    Vovk, Mr. Janko
    [' Mr.']
    Lahoud, Mr. Sarkis
    [' Mr.']
    Hippach, Mrs. Louis Albert (Ida Sophia Fischer)
    [' Mrs.']
    Kassem, Mr. Fared
    [' Mr.']
    Farrell, Mr. James
    [' Mr.']
    Ridsdale, Miss. Lucy
    [' Miss.']
    Farthing, Mr. John
    [' Mr.']
    Salonen, Mr. Johan Werner
    [' Mr.']
    Hocking, Mr. Richard George
    [' Mr.']
    Quick, Miss. Phyllis May
    [' Miss.']
    Toufik, Mr. Nakli
    [' Mr.']
    Elias, Mr. Joseph Jr
    [' Mr.']
    Peter, Mrs. Catherine (Catherine Rizk)
    [' Mrs.']
    Cacic, Miss. Marija
    [' Miss.']
    Hart, Miss. Eva Miriam
    [' Miss.']
    Butt, Major. Archibald Willingham
    [' Major.']
    LeRoy, Miss. Bertha
    [' Miss.']
    Risien, Mr. Samuel Beard
    [' Mr.']
    Frolicher, Miss. Hedwig Margaritha
    [' Miss.']
    Crosby, Miss. Harriet R
    [' Miss.']
    Andersson, Miss. Ingeborg Constanzia
    [' Miss.']
    Andersson, Miss. Sigrid Elisabeth
    [' Miss.']
    Beane, Mr. Edward
    [' Mr.']
    Douglas, Mr. Walter Donald
    [' Mr.']
    Nicholson, Mr. Arthur Ernest
    [' Mr.']
    Beane, Mrs. Edward (Ethel Clarke)
    [' Mrs.']
    Padro y Manent, Mr. Julian
    [' Mr.']
    Goldsmith, Mr. Frank John
    [' Mr.']
    Davies, Master. John Morgan Jr
    [' Master.']
    Thayer, Mr. John Borland Jr
    [' Mr.']
    Sharp, Mr. Percival James R
    [' Mr.']
    O'Brien, Mr. Timothy
    [' Mr.']
    Leeni, Mr. Fahim ("Philip Zenni")
    [' Mr.']
    Ohman, Miss. Velin
    [' Miss.']
    Wright, Mr. George
    [' Mr.']
    Duff Gordon, Lady. (Lucille Christiana Sutherland) ("Mrs Morgan")
    [' Lady.']
    Robbins, Mr. Victor
    [' Mr.']
    Taussig, Mrs. Emil (Tillie Mandelbaum)
    [' Mrs.']
    de Messemaeker, Mrs. Guillaume Joseph (Emma)
    [' Mrs.']
    Morrow, Mr. Thomas Rowan
    [' Mr.']
    Sivic, Mr. Husein
    [' Mr.']
    Norman, Mr. Robert Douglas
    [' Mr.']
    Simmons, Mr. John
    [' Mr.']
    Meanwell, Miss. (Marion Ogden)
    [' Miss.']
    Davies, Mr. Alfred J
    [' Mr.']
    Stoytcheff, Mr. Ilia
    [' Mr.']
    Palsson, Mrs. Nils (Alma Cornelia Berglund)
    [' Mrs.']
    Doharr, Mr. Tannous
    [' Mr.']
    Jonsson, Mr. Carl
    [' Mr.']
    Harris, Mr. George
    [' Mr.']
    Appleton, Mrs. Edward Dale (Charlotte Lamson)
    [' Mrs.']
    Flynn, Mr. John Irwin ("Irving")
    [' Mr.']
    Kelly, Miss. Mary
    [' Miss.']
    Rush, Mr. Alfred George John
    [' Mr.']
    Patchett, Mr. George
    [' Mr.']
    Garside, Miss. Ethel
    [' Miss.']
    Silvey, Mrs. William Baird (Alice Munger)
    [' Mrs.']
    Caram, Mrs. Joseph (Maria Elias)
    [' Mrs.']
    Jussila, Mr. Eiriik
    [' Mr.']
    Christy, Miss. Julie Rachel
    [' Miss.']
    Thayer, Mrs. John Borland (Marian Longstreth Morris)
    [' Mrs.']
    Downton, Mr. William James
    [' Mr.']
    Ross, Mr. John Hugo
    [' Mr.']
    Paulner, Mr. Uscher
    [' Mr.']
    Taussig, Miss. Ruth
    [' Miss.']
    Jarvis, Mr. John Denzil
    [' Mr.']
    Frolicher-Stehli, Mr. Maxmillian
    [' Mr.']
    Gilinski, Mr. Eliezer
    [' Mr.']
    Murdlin, Mr. Joseph
    [' Mr.']
    Rintamaki, Mr. Matti
    [' Mr.']
    Stephenson, Mrs. Walter Bertram (Martha Eustis)
    [' Mrs.']
    Elsbury, Mr. William James
    [' Mr.']
    Bourke, Miss. Mary
    [' Miss.']
    Chapman, Mr. John Henry
    [' Mr.']
    Van Impe, Mr. Jean Baptiste
    [' Mr.']
    Leitch, Miss. Jessie Wills
    [' Miss.']
    Johnson, Mr. Alfred
    [' Mr.']
    Boulos, Mr. Hanna
    [' Mr.']
    Duff Gordon, Sir. Cosmo Edmund ("Mr Morgan")
    [' Sir.']
    Jacobsohn, Mrs. Sidney Samuel (Amy Frances Christy)
    [' Mrs.']
    Slabenoff, Mr. Petco
    [' Mr.']
    Harrington, Mr. Charles H
    [' Mr.']
    Torber, Mr. Ernst William
    [' Mr.']
    Homer, Mr. Harry ("Mr E Haven")
    [' Mr.']
    Lindell, Mr. Edvard Bengtsson
    [' Mr.']
    Karaic, Mr. Milan
    [' Mr.']
    Daniel, Mr. Robert Williams
    [' Mr.']
    Laroche, Mrs. Joseph (Juliette Marie Louise Lafargue)
    [' Mrs.']
    Shutes, Miss. Elizabeth W
    [' Miss.']
    Andersson, Mrs. Anders Johan (Alfrida Konstantia Brogren)
    [' Mrs.']
    Jardin, Mr. Jose Neto
    [' Mr.']
    Murphy, Miss. Margaret Jane
    [' Miss.']
    Horgan, Mr. John
    [' Mr.']
    Brocklebank, Mr. William Alfred
    [' Mr.']
    Herman, Miss. Alice
    [' Miss.']
    Danbom, Mr. Ernst Gilbert
    [' Mr.']
    Lobb, Mrs. William Arthur (Cordelia K Stanlick)
    [' Mrs.']
    Becker, Miss. Marion Louise
    [' Miss.']
    Gavey, Mr. Lawrence
    [' Mr.']
    Yasbeck, Mr. Antoni
    [' Mr.']
    Kimball, Mr. Edwin Nelson Jr
    [' Mr.']
    Nakid, Mr. Sahid
    [' Mr.']
    Hansen, Mr. Henry Damsgaard
    [' Mr.']
    Bowen, Mr. David John "Dai"
    [' Mr.']
    Sutton, Mr. Frederick
    [' Mr.']
    Kirkland, Rev. Charles Leonard
    [' Rev.']
    Longley, Miss. Gretchen Fiske
    [' Miss.']
    Bostandyeff, Mr. Guentcho
    [' Mr.']
    O'Connell, Mr. Patrick D
    [' Mr.']
    Barkworth, Mr. Algernon Henry Wilson
    [' Mr.']
    Lundahl, Mr. Johan Svensson
    [' Mr.']
    Stahelin-Maeglin, Dr. Max
    [' Dr.']
    Parr, Mr. William Henry Marsh
    [' Mr.']
    Skoog, Miss. Mabel
    [' Miss.']
    Davis, Miss. Mary
    [' Miss.']
    Leinonen, Mr. Antti Gustaf
    [' Mr.']
    Collyer, Mr. Harvey
    [' Mr.']
    Panula, Mrs. Juha (Maria Emilia Ojala)
    [' Mrs.']
    Thorneycroft, Mr. Percival
    [' Mr.']
    Jensen, Mr. Hans Peder
    [' Mr.']
    Sagesser, Mlle. Emma
    [' Mlle.']
    Skoog, Miss. Margit Elizabeth
    [' Miss.']
    Foo, Mr. Choong
    [' Mr.']
    Baclini, Miss. Eugenie
    [' Miss.']
    Harper, Mr. Henry Sleeper
    [' Mr.']
    Cor, Mr. Liudevit
    [' Mr.']
    Simonius-Blumer, Col. Oberst Alfons
    [' Col.']
    Willey, Mr. Edward
    [' Mr.']
    Stanley, Miss. Amy Zillah Elsie
    [' Miss.']
    Mitkoff, Mr. Mito
    [' Mr.']
    Doling, Miss. Elsie
    [' Miss.']
    Kalvik, Mr. Johannes Halvorsen
    [' Mr.']
    O'Leary, Miss. Hanora "Norah"
    [' Miss.']
    Hegarty, Miss. Hanora "Nora"
    [' Miss.']
    Hickman, Mr. Leonard Mark
    [' Mr.']
    Radeff, Mr. Alexander
    [' Mr.']
    Bourke, Mrs. John (Catherine)
    [' Mrs.']
    Eitemiller, Mr. George Floyd
    [' Mr.']
    Newell, Mr. Arthur Webster
    [' Mr.']
    Frauenthal, Dr. Henry William
    [' Dr.']
    Badt, Mr. Mohamed
    [' Mr.']
    Colley, Mr. Edward Pomeroy
    [' Mr.']
    Coleff, Mr. Peju
    [' Mr.']
    Lindqvist, Mr. Eino William
    [' Mr.']
    Hickman, Mr. Lewis
    [' Mr.']
    Butler, Mr. Reginald Fenton
    [' Mr.']
    Rommetvedt, Mr. Knud Paust
    [' Mr.']
    Cook, Mr. Jacob
    [' Mr.']
    Taylor, Mrs. Elmer Zebley (Juliet Cummins Wright)
    [' Mrs.']
    Brown, Mrs. Thomas William Solomon (Elizabeth Catherine Ford)
    [' Mrs.']
    Davidson, Mr. Thornton
    [' Mr.']
    Mitchell, Mr. Henry Michael
    [' Mr.']
    Wilhelms, Mr. Charles
    [' Mr.']
    Watson, Mr. Ennis Hastings
    [' Mr.']
    Edvardsson, Mr. Gustaf Hjalmar
    [' Mr.']
    Sawyer, Mr. Frederick Charles
    [' Mr.']
    Turja, Miss. Anna Sofia
    [' Miss.']
    Goodwin, Mrs. Frederick (Augusta Tyler)
    [' Mrs.']
    Cardeza, Mr. Thomas Drake Martinez
    [' Mr.']
    Peters, Miss. Katie
    [' Miss.']
    Hassab, Mr. Hammad
    [' Mr.']
    Olsvigen, Mr. Thor Anderson
    [' Mr.']
    Goodwin, Mr. Charles Edward
    [' Mr.']
    Brown, Mr. Thomas William Solomon
    [' Mr.']
    Laroche, Mr. Joseph Philippe Lemercier
    [' Mr.']
    Panula, Mr. Jaako Arnold
    [' Mr.']
    Dakic, Mr. Branko
    [' Mr.']
    Fischer, Mr. Eberhard Thelander
    [' Mr.']
    Madill, Miss. Georgette Alexandra
    [' Miss.']
    Dick, Mr. Albert Adrian
    [' Mr.']
    Karun, Miss. Manca
    [' Miss.']
    Lam, Mr. Ali
    [' Mr.']
    Saad, Mr. Khalil
    [' Mr.']
    Weir, Col. John
    [' Col.']
    Chapman, Mr. Charles Henry
    [' Mr.']
    Kelly, Mr. James
    [' Mr.']
    Mullens, Miss. Katherine "Katie"
    [' Miss.']
    Thayer, Mr. John Borland
    [' Mr.']
    Humblen, Mr. Adolf Mathias Nicolai Olsen
    [' Mr.']
    Astor, Mrs. John Jacob (Madeleine Talmadge Force)
    [' Mrs.']
    Silverthorne, Mr. Spencer Victor
    [' Mr.']
    Barbara, Miss. Saiide
    [' Miss.']
    Gallagher, Mr. Martin
    [' Mr.']
    Hansen, Mr. Henrik Juul
    [' Mr.']
    Morley, Mr. Henry Samuel ("Mr Henry Marshall")
    [' Mr.']
    Kelly, Mrs. Florence "Fannie"
    [' Mrs.']
    Calderhead, Mr. Edward Pennington
    [' Mr.']
    Cleaver, Miss. Alice
    [' Miss.']
    Moubarek, Master. Halim Gonios ("William George")
    [' Master.']
    Mayne, Mlle. Berthe Antonine ("Mrs de Villiers")
    [' Mlle.']
    Klaber, Mr. Herman
    [' Mr.']
    Taylor, Mr. Elmer Zebley
    [' Mr.']
    Larsson, Mr. August Viktor
    [' Mr.']
    Greenberg, Mr. Samuel
    [' Mr.']
    Soholt, Mr. Peter Andreas Lauritz Andersen
    [' Mr.']
    Endres, Miss. Caroline Louise
    [' Miss.']
    Troutt, Miss. Edwina Celia "Winnie"
    [' Miss.']
    McEvoy, Mr. Michael
    [' Mr.']
    Johnson, Mr. Malkolm Joackim
    [' Mr.']
    Harper, Miss. Annie Jessie "Nina"
    [' Miss.']
    Jensen, Mr. Svend Lauritz
    [' Mr.']
    Gillespie, Mr. William Henry
    [' Mr.']
    Hodges, Mr. Henry Price
    [' Mr.']
    Chambers, Mr. Norman Campbell
    [' Mr.']
    Oreskovic, Mr. Luka
    [' Mr.']
    Renouf, Mrs. Peter Henry (Lillian Jefferys)
    [' Mrs.']
    Mannion, Miss. Margareth
    [' Miss.']
    Bryhl, Mr. Kurt Arnold Gottfrid
    [' Mr.']
    Ilmakangas, Miss. Pieta Sofia
    [' Miss.']
    Allen, Miss. Elisabeth Walton
    [' Miss.']
    Hassan, Mr. Houssein G N
    [' Mr.']
    Knight, Mr. Robert J
    [' Mr.']
    Berriman, Mr. William John
    [' Mr.']
    Troupiansky, Mr. Moses Aaron
    [' Mr.']
    Williams, Mr. Leslie
    [' Mr.']
    Ford, Mrs. Edward (Margaret Ann Watson)
    [' Mrs.']
    Lesurer, Mr. Gustave J
    [' Mr.']
    Ivanoff, Mr. Kanio
    [' Mr.']
    Nankoff, Mr. Minko
    [' Mr.']
    Hawksford, Mr. Walter James
    [' Mr.']
    Cavendish, Mr. Tyrell William
    [' Mr.']
    Ryerson, Miss. Susan Parker "Suzette"
    [' Miss.']
    McNamee, Mr. Neal
    [' Mr.']
    Stranden, Mr. Juho
    [' Mr.']
    Crosby, Capt. Edward Gifford
    [' Capt.']
    Abbott, Mr. Rossmore Edward
    [' Mr.']
    Sinkkonen, Miss. Anna
    [' Miss.']
    Marvin, Mr. Daniel Warner
    [' Mr.']
    Connaghton, Mr. Michael
    [' Mr.']
    Wells, Miss. Joan
    [' Miss.']
    Moor, Master. Meier
    [' Master.']
    Vande Velde, Mr. Johannes Joseph
    [' Mr.']
    Jonkoff, Mr. Lalio
    [' Mr.']
    Herman, Mrs. Samuel (Jane Laver)
    [' Mrs.']
    Hamalainen, Master. Viljo
    [' Master.']
    Carlsson, Mr. August Sigfrid
    [' Mr.']
    Bailey, Mr. Percy Andrew
    [' Mr.']
    Theobald, Mr. Thomas Leonard
    [' Mr.']
    Rothes, the Countess. of (Lucy Noel Martha Dyer-Edwards)
    [' Countess.']
    Garfirth, Mr. John
    [' Mr.']
    Nirva, Mr. Iisakki Antino Aijo
    [' Mr.']
    Barah, Mr. Hanna Assi
    [' Mr.']
    Carter, Mrs. William Ernest (Lucile Polk)
    [' Mrs.']
    Eklund, Mr. Hans Linus
    [' Mr.']
    Hogeboom, Mrs. John C (Anna Andrews)
    [' Mrs.']
    Brewe, Dr. Arthur Jackson
    [' Dr.']
    Mangan, Miss. Mary
    [' Miss.']
    Moran, Mr. Daniel J
    [' Mr.']
    Gronnestad, Mr. Daniel Danielsen
    [' Mr.']
    Lievens, Mr. Rene Aime
    [' Mr.']
    Jensen, Mr. Niels Peder
    [' Mr.']
    Mack, Mrs. (Mary)
    [' Mrs.']
    Elias, Mr. Dibo
    [' Mr.']
    Hocking, Mrs. Elizabeth (Eliza Needs)
    [' Mrs.']
    Myhrman, Mr. Pehr Fabian Oliver Malkolm
    [' Mr.']
    Tobin, Mr. Roger
    [' Mr.']
    Emanuel, Miss. Virginia Ethel
    [' Miss.']
    Kilgannon, Mr. Thomas J
    [' Mr.']
    Robert, Mrs. Edward Scott (Elisabeth Walton McMillan)
    [' Mrs.']
    Ayoub, Miss. Banoura
    [' Miss.']
    Dick, Mrs. Albert Adrian (Vera Gillespie)
    [' Mrs.']
    Long, Mr. Milton Clyde
    [' Mr.']
    Johnston, Mr. Andrew G
    [' Mr.']
    Ali, Mr. William
    [' Mr.']
    Harmer, Mr. Abraham (David Lishin)
    [' Mr.']
    Sjoblom, Miss. Anna Sofia
    [' Miss.']
    Rice, Master. George Hugh
    [' Master.']
    Dean, Master. Bertram Vere
    [' Master.']
    Guggenheim, Mr. Benjamin
    [' Mr.']
    Keane, Mr. Andrew "Andy"
    [' Mr.']
    Gaskell, Mr. Alfred
    [' Mr.']
    Sage, Miss. Stella Anna
    [' Miss.']
    Hoyt, Mr. William Fisher
    [' Mr.']
    Dantcheff, Mr. Ristiu
    [' Mr.']
    Otter, Mr. Richard
    [' Mr.']
    Leader, Dr. Alice (Farnham)
    [' Dr.']
    Osman, Mrs. Mara
    [' Mrs.']
    Ibrahim Shawah, Mr. Yousseff
    [' Mr.']
    Van Impe, Mrs. Jean Baptiste (Rosalie Paula Govaert)
    [' Mrs.']
    Ponesell, Mr. Martin
    [' Mr.']
    Collyer, Mrs. Harvey (Charlotte Annie Tate)
    [' Mrs.']
    Carter, Master. William Thornton II
    [' Master.']
    Thomas, Master. Assad Alexander
    [' Master.']
    Hedman, Mr. Oskar Arvid
    [' Mr.']
    Johansson, Mr. Karl Johan
    [' Mr.']
    Andrews, Mr. Thomas Jr
    [' Mr.']
    Pettersson, Miss. Ellen Natalia
    [' Miss.']
    Meyer, Mr. August
    [' Mr.']
    Chambers, Mrs. Norman Campbell (Bertha Griggs)
    [' Mrs.']
    Alexander, Mr. William
    [' Mr.']
    Lester, Mr. James
    [' Mr.']
    Slemen, Mr. Richard James
    [' Mr.']
    Andersson, Miss. Ebba Iris Alfrida
    [' Miss.']
    Tomlin, Mr. Ernest Portage
    [' Mr.']
    Fry, Mr. Richard
    [' Mr.']
    Heininen, Miss. Wendla Maria
    [' Miss.']
    Mallet, Mr. Albert
    [' Mr.']
    Holm, Mr. John Fredrik Alexander
    [' Mr.']
    Skoog, Master. Karl Thorsten
    [' Master.']
    Hays, Mrs. Charles Melville (Clara Jennings Gregg)
    [' Mrs.']
    Lulic, Mr. Nikola
    [' Mr.']
    Reuchlin, Jonkheer. John George
    [' Jonkheer.']
    Moor, Mrs. (Beila)
    [' Mrs.']
    Panula, Master. Urho Abraham
    [' Master.']
    Flynn, Mr. John
    [' Mr.']
    Lam, Mr. Len
    [' Mr.']
    Mallet, Master. Andre
    [' Master.']
    McCormack, Mr. Thomas Joseph
    [' Mr.']
    Stone, Mrs. George Nelson (Martha Evelyn)
    [' Mrs.']
    Yasbeck, Mrs. Antoni (Selini Alexander)
    [' Mrs.']
    Richards, Master. George Sibley
    [' Master.']
    Saad, Mr. Amin
    [' Mr.']
    Augustsson, Mr. Albert
    [' Mr.']
    Allum, Mr. Owen George
    [' Mr.']
    Compton, Miss. Sara Rebecca
    [' Miss.']
    Pasic, Mr. Jakob
    [' Mr.']
    Sirota, Mr. Maurice
    [' Mr.']
    Chip, Mr. Chang
    [' Mr.']
    Marechal, Mr. Pierre
    [' Mr.']
    Alhomaki, Mr. Ilmari Rudolf
    [' Mr.']
    Mudd, Mr. Thomas Charles
    [' Mr.']
    Serepeca, Miss. Augusta
    [' Miss.']
    Lemberopolous, Mr. Peter L
    [' Mr.']
    Culumovic, Mr. Jeso
    [' Mr.']
    Abbing, Mr. Anthony
    [' Mr.']
    Sage, Mr. Douglas Bullen
    [' Mr.']
    Markoff, Mr. Marin
    [' Mr.']
    Harper, Rev. John
    [' Rev.']
    Goldenberg, Mrs. Samuel L (Edwiga Grabowska)
    [' Mrs.']
    Andersson, Master. Sigvard Harald Elias
    [' Master.']
    Svensson, Mr. Johan
    [' Mr.']
    Boulos, Miss. Nourelain
    [' Miss.']
    Lines, Miss. Mary Conover
    [' Miss.']
    Carter, Mrs. Ernest Courtenay (Lilian Hughes)
    [' Mrs.']
    Aks, Mrs. Sam (Leah Rosen)
    [' Mrs.']
    Wick, Mrs. George Dennick (Mary Hitchcock)
    [' Mrs.']
    Daly, Mr. Peter Denis 
    [' Mr.']
    Baclini, Mrs. Solomon (Latifa Qurban)
    [' Mrs.']
    Razi, Mr. Raihed
    [' Mr.']
    Hansen, Mr. Claus Peter
    [' Mr.']
    Giles, Mr. Frederick Edward
    [' Mr.']
    Swift, Mrs. Frederick Joel (Margaret Welles Barron)
    [' Mrs.']
    Sage, Miss. Dorothy Edith "Dolly"
    [' Miss.']
    Gill, Mr. John William
    [' Mr.']
    Bystrom, Mrs. (Karolina)
    [' Mrs.']
    Duran y More, Miss. Asuncion
    [' Miss.']
    Roebling, Mr. Washington Augustus II
    [' Mr.']
    van Melkebeke, Mr. Philemon
    [' Mr.']
    Johnson, Master. Harold Theodor
    [' Master.']
    Balkic, Mr. Cerin
    [' Mr.']
    Beckwith, Mrs. Richard Leonard (Sallie Monypeny)
    [' Mrs.']
    Carlsson, Mr. Frans Olof
    [' Mr.']
    Vander Cruyssen, Mr. Victor
    [' Mr.']
    Abelson, Mrs. Samuel (Hannah Wizosky)
    [' Mrs.']
    Najib, Miss. Adele Kiamie "Jane"
    [' Miss.']
    Gustafsson, Mr. Alfred Ossian
    [' Mr.']
    Petroff, Mr. Nedelio
    [' Mr.']
    Laleff, Mr. Kristo
    [' Mr.']
    Potter, Mrs. Thomas Jr (Lily Alexenia Wilson)
    [' Mrs.']
    Shelley, Mrs. William (Imanita Parrish Hall)
    [' Mrs.']
    Markun, Mr. Johann
    [' Mr.']
    Dahlberg, Miss. Gerda Ulrika
    [' Miss.']
    Banfield, Mr. Frederick James
    [' Mr.']
    Sutehall, Mr. Henry Jr
    [' Mr.']
    Rice, Mrs. William (Margaret Norton)
    [' Mrs.']
    Montvila, Rev. Juozas
    [' Rev.']
    Graham, Miss. Margaret Edith
    [' Miss.']
    Johnston, Miss. Catherine Helen "Carrie"
    [' Miss.']
    Behr, Mr. Karl Howell
    [' Mr.']
    Dooley, Mr. Patrick
    [' Mr.']


### 이름 데이터만 보고 성별관련 정보 알아내기


```python
import openpyxl
import re

regex = re.compile(' [A-Za-z]+\.')
# 엑셀파일 열기
work_book = openpyxl.load_workbook('01_data/train.xlsx')

# 현재 Active Sheet 얻기
work_sheet = work_book.active

# work_sheet.rows는 해당 쉬트의 모든 행을 객체로 가지고 있음
for each_row in work_sheet.rows:
    print (each_row[3].value)
    print (regex.findall(each_row[3].value))
    if len(regex.findall(each_row[3].value)) > 0:
        if regex.findall(each_row[3].value)[0].strip() == "Mr.":
            print ("남성")

work_book.close()
```

    Name
    []
    Braund, Mr. Owen Harris
    [' Mr.']
    남성
    Cumings, Mrs. John Bradley (Florence Briggs Thayer)
    [' Mrs.']
    Heikkinen, Miss. Laina
    [' Miss.']
    Futrelle, Mrs. Jacques Heath (Lily May Peel)
    [' Mrs.']
    Allen, Mr. William Henry
    [' Mr.']
    남성
    Moran, Mr. James
    [' Mr.']
    남성
    McCarthy, Mr. Timothy J
    [' Mr.']
    남성
    Palsson, Master. Gosta Leonard
    [' Master.']
    Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)
    [' Mrs.']
    Nasser, Mrs. Nicholas (Adele Achem)
    [' Mrs.']
    Sandstrom, Miss. Marguerite Rut
    [' Miss.']
    Bonnell, Miss. Elizabeth
    [' Miss.']
    Saundercock, Mr. William Henry
    [' Mr.']
    남성
    Andersson, Mr. Anders Johan
    [' Mr.']
    남성
    Vestrom, Miss. Hulda Amanda Adolfina
    [' Miss.']
    Hewlett, Mrs. (Mary D Kingcome) 
    [' Mrs.']
    Rice, Master. Eugene
    [' Master.']
    Williams, Mr. Charles Eugene
    [' Mr.']
    남성
    Vander Planke, Mrs. Julius (Emelia Maria Vandemoortele)
    [' Mrs.']
    Masselmani, Mrs. Fatima
    [' Mrs.']
    Fynney, Mr. Joseph J
    [' Mr.']
    남성
    Beesley, Mr. Lawrence
    [' Mr.']
    남성
    McGowan, Miss. Anna "Annie"
    [' Miss.']
    Sloper, Mr. William Thompson
    [' Mr.']
    남성
    Palsson, Miss. Torborg Danira
    [' Miss.']
    Asplund, Mrs. Carl Oscar (Selma Augusta Emilia Johansson)
    [' Mrs.']
    Emir, Mr. Farred Chehab
    [' Mr.']
    남성
    Fortune, Mr. Charles Alexander
    [' Mr.']
    남성
    O'Dwyer, Miss. Ellen "Nellie"
    [' Miss.']
    Todoroff, Mr. Lalio
    [' Mr.']
    남성
    Uruchurtu, Don. Manuel E
    [' Don.']
    Spencer, Mrs. William Augustus (Marie Eugenie)
    [' Mrs.']
    Glynn, Miss. Mary Agatha
    [' Miss.']
    Wheadon, Mr. Edward H
    [' Mr.']
    남성
    Meyer, Mr. Edgar Joseph
    [' Mr.']
    남성
    Holverson, Mr. Alexander Oskar
    [' Mr.']
    남성
    Mamee, Mr. Hanna
    [' Mr.']
    남성
    Cann, Mr. Ernest Charles
    [' Mr.']
    남성
    Vander Planke, Miss. Augusta Maria
    [' Miss.']
    Nicola-Yarred, Miss. Jamila
    [' Miss.']
    Ahlin, Mrs. Johan (Johanna Persdotter Larsson)
    [' Mrs.']
    Turpin, Mrs. William John Robert (Dorothy Ann Wonnacott)
    [' Mrs.']
    Kraeff, Mr. Theodor
    [' Mr.']
    남성
    Laroche, Miss. Simonne Marie Anne Andree
    [' Miss.']
    Devaney, Miss. Margaret Delia
    [' Miss.']
    Rogers, Mr. William John
    [' Mr.']
    남성
    Lennon, Mr. Denis
    [' Mr.']
    남성
    O'Driscoll, Miss. Bridget
    [' Miss.']
    Samaan, Mr. Youssef
    [' Mr.']
    남성
    Arnold-Franchi, Mrs. Josef (Josefine Franchi)
    [' Mrs.']
    Panula, Master. Juha Niilo
    [' Master.']
    Nosworthy, Mr. Richard Cater
    [' Mr.']
    남성
    Harper, Mrs. Henry Sleeper (Myna Haxtun)
    [' Mrs.']
    Faunthorpe, Mrs. Lizzie (Elizabeth Anne Wilkinson)
    [' Mrs.']
    Ostby, Mr. Engelhart Cornelius
    [' Mr.']
    남성
    Woolner, Mr. Hugh
    [' Mr.']
    남성
    Rugg, Miss. Emily
    [' Miss.']
    Novel, Mr. Mansouer
    [' Mr.']
    남성
    West, Miss. Constance Mirium
    [' Miss.']
    Goodwin, Master. William Frederick
    [' Master.']
    Sirayanian, Mr. Orsen
    [' Mr.']
    남성
    Icard, Miss. Amelie
    [' Miss.']
    Harris, Mr. Henry Birkhardt
    [' Mr.']
    남성
    Skoog, Master. Harald
    [' Master.']
    Stewart, Mr. Albert A
    [' Mr.']
    남성
    Moubarek, Master. Gerios
    [' Master.']
    Nye, Mrs. (Elizabeth Ramell)
    [' Mrs.']
    Crease, Mr. Ernest James
    [' Mr.']
    남성
    Andersson, Miss. Erna Alexandra
    [' Miss.']
    Kink, Mr. Vincenz
    [' Mr.']
    남성
    Jenkin, Mr. Stephen Curnow
    [' Mr.']
    남성
    Goodwin, Miss. Lillian Amy
    [' Miss.']
    Hood, Mr. Ambrose Jr
    [' Mr.']
    남성
    Chronopoulos, Mr. Apostolos
    [' Mr.']
    남성
    Bing, Mr. Lee
    [' Mr.']
    남성
    Moen, Mr. Sigurd Hansen
    [' Mr.']
    남성
    Staneff, Mr. Ivan
    [' Mr.']
    남성
    Moutal, Mr. Rahamin Haim
    [' Mr.']
    남성
    Caldwell, Master. Alden Gates
    [' Master.']
    Dowdell, Miss. Elizabeth
    [' Miss.']
    Waelens, Mr. Achille
    [' Mr.']
    남성
    Sheerlinck, Mr. Jan Baptist
    [' Mr.']
    남성
    McDermott, Miss. Brigdet Delia
    [' Miss.']
    Carrau, Mr. Francisco M
    [' Mr.']
    남성
    Ilett, Miss. Bertha
    [' Miss.']
    Backstrom, Mrs. Karl Alfred (Maria Mathilda Gustafsson)
    [' Mrs.']
    Ford, Mr. William Neal
    [' Mr.']
    남성
    Slocovski, Mr. Selman Francis
    [' Mr.']
    남성
    Fortune, Miss. Mabel Helen
    [' Miss.']
    Celotti, Mr. Francesco
    [' Mr.']
    남성
    Christmann, Mr. Emil
    [' Mr.']
    남성
    Andreasson, Mr. Paul Edvin
    [' Mr.']
    남성
    Chaffee, Mr. Herbert Fuller
    [' Mr.']
    남성
    Dean, Mr. Bertram Frank
    [' Mr.']
    남성
    Coxon, Mr. Daniel
    [' Mr.']
    남성
    Shorney, Mr. Charles Joseph
    [' Mr.']
    남성
    Goldschmidt, Mr. George B
    [' Mr.']
    남성
    Greenfield, Mr. William Bertram
    [' Mr.']
    남성
    Doling, Mrs. John T (Ada Julia Bone)
    [' Mrs.']
    Kantor, Mr. Sinai
    [' Mr.']
    남성
    Petranec, Miss. Matilda
    [' Miss.']
    Petroff, Mr. Pastcho ("Pentcho")
    [' Mr.']
    남성
    White, Mr. Richard Frasar
    [' Mr.']
    남성
    Johansson, Mr. Gustaf Joel
    [' Mr.']
    남성
    Gustafsson, Mr. Anders Vilhelm
    [' Mr.']
    남성
    Mionoff, Mr. Stoytcho
    [' Mr.']
    남성
    Salkjelsvik, Miss. Anna Kristine
    [' Miss.']
    Moss, Mr. Albert Johan
    [' Mr.']
    남성
    Rekic, Mr. Tido
    [' Mr.']
    남성
    Moran, Miss. Bertha
    [' Miss.']
    Porter, Mr. Walter Chamberlain
    [' Mr.']
    남성
    Zabour, Miss. Hileni
    [' Miss.']
    Barton, Mr. David John
    [' Mr.']
    남성
    Jussila, Miss. Katriina
    [' Miss.']
    Attalah, Miss. Malake
    [' Miss.']
    Pekoniemi, Mr. Edvard
    [' Mr.']
    남성
    Connors, Mr. Patrick
    [' Mr.']
    남성
    Turpin, Mr. William John Robert
    [' Mr.']
    남성
    Baxter, Mr. Quigg Edmond
    [' Mr.']
    남성
    Andersson, Miss. Ellis Anna Maria
    [' Miss.']
    Hickman, Mr. Stanley George
    [' Mr.']
    남성
    Moore, Mr. Leonard Charles
    [' Mr.']
    남성
    Nasser, Mr. Nicholas
    [' Mr.']
    남성
    Webber, Miss. Susan
    [' Miss.']
    White, Mr. Percival Wayland
    [' Mr.']
    남성
    Nicola-Yarred, Master. Elias
    [' Master.']
    McMahon, Mr. Martin
    [' Mr.']
    남성
    Madsen, Mr. Fridtjof Arne
    [' Mr.']
    남성
    Peter, Miss. Anna
    [' Miss.']
    Ekstrom, Mr. Johan
    [' Mr.']
    남성
    Drazenoic, Mr. Jozef
    [' Mr.']
    남성
    Coelho, Mr. Domingos Fernandeo
    [' Mr.']
    남성
    Robins, Mrs. Alexander A (Grace Charity Laury)
    [' Mrs.']
    Weisz, Mrs. Leopold (Mathilde Francoise Pede)
    [' Mrs.']
    Sobey, Mr. Samuel James Hayden
    [' Mr.']
    남성
    Richard, Mr. Emile
    [' Mr.']
    남성
    Newsom, Miss. Helen Monypeny
    [' Miss.']
    Futrelle, Mr. Jacques Heath
    [' Mr.']
    남성
    Osen, Mr. Olaf Elon
    [' Mr.']
    남성
    Giglio, Mr. Victor
    [' Mr.']
    남성
    Boulos, Mrs. Joseph (Sultana)
    [' Mrs.']
    Nysten, Miss. Anna Sofia
    [' Miss.']
    Hakkarainen, Mrs. Pekka Pietari (Elin Matilda Dolck)
    [' Mrs.']
    Burke, Mr. Jeremiah
    [' Mr.']
    남성
    Andrew, Mr. Edgardo Samuel
    [' Mr.']
    남성
    Nicholls, Mr. Joseph Charles
    [' Mr.']
    남성
    Andersson, Mr. August Edvard ("Wennerstrom")
    [' Mr.']
    남성
    Ford, Miss. Robina Maggie "Ruby"
    [' Miss.']
    Navratil, Mr. Michel ("Louis M Hoffman")
    [' Mr.']
    남성
    Byles, Rev. Thomas Roussel Davids
    [' Rev.']
    Bateman, Rev. Robert James
    [' Rev.']
    Pears, Mrs. Thomas (Edith Wearne)
    [' Mrs.']
    Meo, Mr. Alfonzo
    [' Mr.']
    남성
    van Billiard, Mr. Austin Blyler
    [' Mr.']
    남성
    Olsen, Mr. Ole Martin
    [' Mr.']
    남성
    Williams, Mr. Charles Duane
    [' Mr.']
    남성
    Gilnagh, Miss. Katherine "Katie"
    [' Miss.']
    Corn, Mr. Harry
    [' Mr.']
    남성
    Smiljanic, Mr. Mile
    [' Mr.']
    남성
    Sage, Master. Thomas Henry
    [' Master.']
    Cribb, Mr. John Hatfield
    [' Mr.']
    남성
    Watt, Mrs. James (Elizabeth "Bessie" Inglis Milne)
    [' Mrs.']
    Bengtsson, Mr. John Viktor
    [' Mr.']
    남성
    Calic, Mr. Jovo
    [' Mr.']
    남성
    Panula, Master. Eino Viljami
    [' Master.']
    Goldsmith, Master. Frank John William "Frankie"
    [' Master.']
    Chibnall, Mrs. (Edith Martha Bowerman)
    [' Mrs.']
    Skoog, Mrs. William (Anna Bernhardina Karlsson)
    [' Mrs.']
    Baumann, Mr. John D
    [' Mr.']
    남성
    Ling, Mr. Lee
    [' Mr.']
    남성
    Van der hoef, Mr. Wyckoff
    [' Mr.']
    남성
    Rice, Master. Arthur
    [' Master.']
    Johnson, Miss. Eleanor Ileen
    [' Miss.']
    Sivola, Mr. Antti Wilhelm
    [' Mr.']
    남성
    Smith, Mr. James Clinch
    [' Mr.']
    남성
    Klasen, Mr. Klas Albin
    [' Mr.']
    남성
    Lefebre, Master. Henry Forbes
    [' Master.']
    Isham, Miss. Ann Elizabeth
    [' Miss.']
    Hale, Mr. Reginald
    [' Mr.']
    남성
    Leonard, Mr. Lionel
    [' Mr.']
    남성
    Sage, Miss. Constance Gladys
    [' Miss.']
    Pernot, Mr. Rene
    [' Mr.']
    남성
    Asplund, Master. Clarence Gustaf Hugo
    [' Master.']
    Becker, Master. Richard F
    [' Master.']
    Kink-Heilmann, Miss. Luise Gretchen
    [' Miss.']
    Rood, Mr. Hugh Roscoe
    [' Mr.']
    남성
    O'Brien, Mrs. Thomas (Johanna "Hannah" Godfrey)
    [' Mrs.']
    Romaine, Mr. Charles Hallace ("Mr C Rolmane")
    [' Mr.']
    남성
    Bourke, Mr. John
    [' Mr.']
    남성
    Turcin, Mr. Stjepan
    [' Mr.']
    남성
    Pinsky, Mrs. (Rosa)
    [' Mrs.']
    Carbines, Mr. William
    [' Mr.']
    남성
    Andersen-Jensen, Miss. Carla Christine Nielsine
    [' Miss.']
    Navratil, Master. Michel M
    [' Master.']
    Brown, Mrs. James Joseph (Margaret Tobin)
    [' Mrs.']
    Lurette, Miss. Elise
    [' Miss.']
    Mernagh, Mr. Robert
    [' Mr.']
    남성
    Olsen, Mr. Karl Siegwart Andreas
    [' Mr.']
    남성
    Madigan, Miss. Margaret "Maggie"
    [' Miss.']
    Yrois, Miss. Henriette ("Mrs Harbeck")
    [' Miss.']
    Vande Walle, Mr. Nestor Cyriel
    [' Mr.']
    남성
    Sage, Mr. Frederick
    [' Mr.']
    남성
    Johanson, Mr. Jakob Alfred
    [' Mr.']
    남성
    Youseff, Mr. Gerious
    [' Mr.']
    남성
    Cohen, Mr. Gurshon "Gus"
    [' Mr.']
    남성
    Strom, Miss. Telma Matilda
    [' Miss.']
    Backstrom, Mr. Karl Alfred
    [' Mr.']
    남성
    Albimona, Mr. Nassef Cassem
    [' Mr.']
    남성
    Carr, Miss. Helen "Ellen"
    [' Miss.']
    Blank, Mr. Henry
    [' Mr.']
    남성
    Ali, Mr. Ahmed
    [' Mr.']
    남성
    Cameron, Miss. Clear Annie
    [' Miss.']
    Perkin, Mr. John Henry
    [' Mr.']
    남성
    Givard, Mr. Hans Kristensen
    [' Mr.']
    남성
    Kiernan, Mr. Philip
    [' Mr.']
    남성
    Newell, Miss. Madeleine
    [' Miss.']
    Honkanen, Miss. Eliina
    [' Miss.']
    Jacobsohn, Mr. Sidney Samuel
    [' Mr.']
    남성
    Bazzani, Miss. Albina
    [' Miss.']
    Harris, Mr. Walter
    [' Mr.']
    남성
    Sunderland, Mr. Victor Francis
    [' Mr.']
    남성
    Bracken, Mr. James H
    [' Mr.']
    남성
    Green, Mr. George Henry
    [' Mr.']
    남성
    Nenkoff, Mr. Christo
    [' Mr.']
    남성
    Hoyt, Mr. Frederick Maxfield
    [' Mr.']
    남성
    Berglund, Mr. Karl Ivar Sven
    [' Mr.']
    남성
    Mellors, Mr. William John
    [' Mr.']
    남성
    Lovell, Mr. John Hall ("Henry")
    [' Mr.']
    남성
    Fahlstrom, Mr. Arne Jonas
    [' Mr.']
    남성
    Lefebre, Miss. Mathilde
    [' Miss.']
    Harris, Mrs. Henry Birkhardt (Irene Wallach)
    [' Mrs.']
    Larsson, Mr. Bengt Edvin
    [' Mr.']
    남성
    Sjostedt, Mr. Ernst Adolf
    [' Mr.']
    남성
    Asplund, Miss. Lillian Gertrud
    [' Miss.']
    Leyson, Mr. Robert William Norman
    [' Mr.']
    남성
    Harknett, Miss. Alice Phoebe
    [' Miss.']
    Hold, Mr. Stephen
    [' Mr.']
    남성
    Collyer, Miss. Marjorie "Lottie"
    [' Miss.']
    Pengelly, Mr. Frederick William
    [' Mr.']
    남성
    Hunt, Mr. George Henry
    [' Mr.']
    남성
    Zabour, Miss. Thamine
    [' Miss.']
    Murphy, Miss. Katherine "Kate"
    [' Miss.']
    Coleridge, Mr. Reginald Charles
    [' Mr.']
    남성
    Maenpaa, Mr. Matti Alexanteri
    [' Mr.']
    남성
    Attalah, Mr. Sleiman
    [' Mr.']
    남성
    Minahan, Dr. William Edward
    [' Dr.']
    Lindahl, Miss. Agda Thorilda Viktoria
    [' Miss.']
    Hamalainen, Mrs. William (Anna)
    [' Mrs.']
    Beckwith, Mr. Richard Leonard
    [' Mr.']
    남성
    Carter, Rev. Ernest Courtenay
    [' Rev.']
    Reed, Mr. James George
    [' Mr.']
    남성
    Strom, Mrs. Wilhelm (Elna Matilda Persson)
    [' Mrs.']
    Stead, Mr. William Thomas
    [' Mr.']
    남성
    Lobb, Mr. William Arthur
    [' Mr.']
    남성
    Rosblom, Mrs. Viktor (Helena Wilhelmina)
    [' Mrs.']
    Touma, Mrs. Darwis (Hanne Youssef Razi)
    [' Mrs.']
    Thorne, Mrs. Gertrude Maybelle
    [' Mrs.']
    Cherry, Miss. Gladys
    [' Miss.']
    Ward, Miss. Anna
    [' Miss.']
    Parrish, Mrs. (Lutie Davis)
    [' Mrs.']
    Smith, Mr. Thomas
    [' Mr.']
    남성
    Asplund, Master. Edvin Rojj Felix
    [' Master.']
    Taussig, Mr. Emil
    [' Mr.']
    남성
    Harrison, Mr. William
    [' Mr.']
    남성
    Henry, Miss. Delia
    [' Miss.']
    Reeves, Mr. David
    [' Mr.']
    남성
    Panula, Mr. Ernesti Arvid
    [' Mr.']
    남성
    Persson, Mr. Ernst Ulrik
    [' Mr.']
    남성
    Graham, Mrs. William Thompson (Edith Junkins)
    [' Mrs.']
    Bissette, Miss. Amelia
    [' Miss.']
    Cairns, Mr. Alexander
    [' Mr.']
    남성
    Tornquist, Mr. William Henry
    [' Mr.']
    남성
    Mellinger, Mrs. (Elizabeth Anne Maidment)
    [' Mrs.']
    Natsch, Mr. Charles H
    [' Mr.']
    남성
    Healy, Miss. Hanora "Nora"
    [' Miss.']
    Andrews, Miss. Kornelia Theodosia
    [' Miss.']
    Lindblom, Miss. Augusta Charlotta
    [' Miss.']
    Parkes, Mr. Francis "Frank"
    [' Mr.']
    남성
    Rice, Master. Eric
    [' Master.']
    Abbott, Mrs. Stanton (Rosa Hunt)
    [' Mrs.']
    Duane, Mr. Frank
    [' Mr.']
    남성
    Olsson, Mr. Nils Johan Goransson
    [' Mr.']
    남성
    de Pelsmaeker, Mr. Alfons
    [' Mr.']
    남성
    Dorking, Mr. Edward Arthur
    [' Mr.']
    남성
    Smith, Mr. Richard William
    [' Mr.']
    남성
    Stankovic, Mr. Ivan
    [' Mr.']
    남성
    de Mulder, Mr. Theodore
    [' Mr.']
    남성
    Naidenoff, Mr. Penko
    [' Mr.']
    남성
    Hosono, Mr. Masabumi
    [' Mr.']
    남성
    Connolly, Miss. Kate
    [' Miss.']
    Barber, Miss. Ellen "Nellie"
    [' Miss.']
    Bishop, Mrs. Dickinson H (Helen Walton)
    [' Mrs.']
    Levy, Mr. Rene Jacques
    [' Mr.']
    남성
    Haas, Miss. Aloisia
    [' Miss.']
    Mineff, Mr. Ivan
    [' Mr.']
    남성
    Lewy, Mr. Ervin G
    [' Mr.']
    남성
    Hanna, Mr. Mansour
    [' Mr.']
    남성
    Allison, Miss. Helen Loraine
    [' Miss.']
    Saalfeld, Mr. Adolphe
    [' Mr.']
    남성
    Baxter, Mrs. James (Helene DeLaudeniere Chaput)
    [' Mrs.']
    Kelly, Miss. Anna Katherine "Annie Kate"
    [' Miss.']
    McCoy, Mr. Bernard
    [' Mr.']
    남성
    Johnson, Mr. William Cahoone Jr
    [' Mr.']
    남성
    Keane, Miss. Nora A
    [' Miss.']
    Williams, Mr. Howard Hugh "Harry"
    [' Mr.']
    남성
    Allison, Master. Hudson Trevor
    [' Master.']
    Fleming, Miss. Margaret
    [' Miss.']
    Penasco y Castellana, Mrs. Victor de Satode (Maria Josefa Perez de Soto y Vallejo)
    [' Mrs.']
    Abelson, Mr. Samuel
    [' Mr.']
    남성
    Francatelli, Miss. Laura Mabel
    [' Miss.']
    Hays, Miss. Margaret Bechstein
    [' Miss.']
    Ryerson, Miss. Emily Borie
    [' Miss.']
    Lahtinen, Mrs. William (Anna Sylfven)
    [' Mrs.']
    Hendekovic, Mr. Ignjac
    [' Mr.']
    남성
    Hart, Mr. Benjamin
    [' Mr.']
    남성
    Nilsson, Miss. Helmina Josefina
    [' Miss.']
    Kantor, Mrs. Sinai (Miriam Sternin)
    [' Mrs.']
    Moraweck, Dr. Ernest
    [' Dr.']
    Wick, Miss. Mary Natalie
    [' Miss.']
    Spedden, Mrs. Frederic Oakley (Margaretta Corning Stone)
    [' Mrs.']
    Dennis, Mr. Samuel
    [' Mr.']
    남성
    Danoff, Mr. Yoto
    [' Mr.']
    남성
    Slayter, Miss. Hilda Mary
    [' Miss.']
    Caldwell, Mrs. Albert Francis (Sylvia Mae Harbaugh)
    [' Mrs.']
    Sage, Mr. George John Jr
    [' Mr.']
    남성
    Young, Miss. Marie Grice
    [' Miss.']
    Nysveen, Mr. Johan Hansen
    [' Mr.']
    남성
    Ball, Mrs. (Ada E Hall)
    [' Mrs.']
    Goldsmith, Mrs. Frank John (Emily Alice Brown)
    [' Mrs.']
    Hippach, Miss. Jean Gertrude
    [' Miss.']
    McCoy, Miss. Agnes
    [' Miss.']
    Partner, Mr. Austen
    [' Mr.']
    남성
    Graham, Mr. George Edward
    [' Mr.']
    남성
    Vander Planke, Mr. Leo Edmondus
    [' Mr.']
    남성
    Frauenthal, Mrs. Henry William (Clara Heinsheimer)
    [' Mrs.']
    Denkoff, Mr. Mitto
    [' Mr.']
    남성
    Pears, Mr. Thomas Clinton
    [' Mr.']
    남성
    Burns, Miss. Elizabeth Margaret
    [' Miss.']
    Dahl, Mr. Karl Edwart
    [' Mr.']
    남성
    Blackwell, Mr. Stephen Weart
    [' Mr.']
    남성
    Navratil, Master. Edmond Roger
    [' Master.']
    Fortune, Miss. Alice Elizabeth
    [' Miss.']
    Collander, Mr. Erik Gustaf
    [' Mr.']
    남성
    Sedgwick, Mr. Charles Frederick Waddington
    [' Mr.']
    남성
    Fox, Mr. Stanley Hubert
    [' Mr.']
    남성
    Brown, Miss. Amelia "Mildred"
    [' Miss.']
    Smith, Miss. Marion Elsie
    [' Miss.']
    Davison, Mrs. Thomas Henry (Mary E Finck)
    [' Mrs.']
    Coutts, Master. William Loch "William"
    [' Master.']
    Dimic, Mr. Jovan
    [' Mr.']
    남성
    Odahl, Mr. Nils Martin
    [' Mr.']
    남성
    Williams-Lambert, Mr. Fletcher Fellows
    [' Mr.']
    남성
    Elias, Mr. Tannous
    [' Mr.']
    남성
    Arnold-Franchi, Mr. Josef
    [' Mr.']
    남성
    Yousif, Mr. Wazli
    [' Mr.']
    남성
    Vanden Steen, Mr. Leo Peter
    [' Mr.']
    남성
    Bowerman, Miss. Elsie Edith
    [' Miss.']
    Funk, Miss. Annie Clemmer
    [' Miss.']
    McGovern, Miss. Mary
    [' Miss.']
    Mockler, Miss. Helen Mary "Ellie"
    [' Miss.']
    Skoog, Mr. Wilhelm
    [' Mr.']
    남성
    del Carlo, Mr. Sebastiano
    [' Mr.']
    남성
    Barbara, Mrs. (Catherine David)
    [' Mrs.']
    Asim, Mr. Adola
    [' Mr.']
    남성
    O'Brien, Mr. Thomas
    [' Mr.']
    남성
    Adahl, Mr. Mauritz Nils Martin
    [' Mr.']
    남성
    Warren, Mrs. Frank Manley (Anna Sophia Atkinson)
    [' Mrs.']
    Moussa, Mrs. (Mantoura Boulos)
    [' Mrs.']
    Jermyn, Miss. Annie
    [' Miss.']
    Aubart, Mme. Leontine Pauline
    [' Mme.']
    Harder, Mr. George Achilles
    [' Mr.']
    남성
    Wiklund, Mr. Jakob Alfred
    [' Mr.']
    남성
    Beavan, Mr. William Thomas
    [' Mr.']
    남성
    Ringhini, Mr. Sante
    [' Mr.']
    남성
    Palsson, Miss. Stina Viola
    [' Miss.']
    Meyer, Mrs. Edgar Joseph (Leila Saks)
    [' Mrs.']
    Landergren, Miss. Aurora Adelia
    [' Miss.']
    Widener, Mr. Harry Elkins
    [' Mr.']
    남성
    Betros, Mr. Tannous
    [' Mr.']
    남성
    Gustafsson, Mr. Karl Gideon
    [' Mr.']
    남성
    Bidois, Miss. Rosalie
    [' Miss.']
    Nakid, Miss. Maria ("Mary")
    [' Miss.']
    Tikkanen, Mr. Juho
    [' Mr.']
    남성
    Holverson, Mrs. Alexander Oskar (Mary Aline Towner)
    [' Mrs.']
    Plotcharsky, Mr. Vasil
    [' Mr.']
    남성
    Davies, Mr. Charles Henry
    [' Mr.']
    남성
    Goodwin, Master. Sidney Leonard
    [' Master.']
    Buss, Miss. Kate
    [' Miss.']
    Sadlier, Mr. Matthew
    [' Mr.']
    남성
    Lehmann, Miss. Bertha
    [' Miss.']
    Carter, Mr. William Ernest
    [' Mr.']
    남성
    Jansson, Mr. Carl Olof
    [' Mr.']
    남성
    Gustafsson, Mr. Johan Birger
    [' Mr.']
    남성
    Newell, Miss. Marjorie
    [' Miss.']
    Sandstrom, Mrs. Hjalmar (Agnes Charlotta Bengtsson)
    [' Mrs.']
    Johansson, Mr. Erik
    [' Mr.']
    남성
    Olsson, Miss. Elina
    [' Miss.']
    McKane, Mr. Peter David
    [' Mr.']
    남성
    Pain, Dr. Alfred
    [' Dr.']
    Trout, Mrs. William H (Jessie L)
    [' Mrs.']
    Niskanen, Mr. Juha
    [' Mr.']
    남성
    Adams, Mr. John
    [' Mr.']
    남성
    Jussila, Miss. Mari Aina
    [' Miss.']
    Hakkarainen, Mr. Pekka Pietari
    [' Mr.']
    남성
    Oreskovic, Miss. Marija
    [' Miss.']
    Gale, Mr. Shadrach
    [' Mr.']
    남성
    Widegren, Mr. Carl/Charles Peter
    [' Mr.']
    남성
    Richards, Master. William Rowe
    [' Master.']
    Birkeland, Mr. Hans Martin Monsen
    [' Mr.']
    남성
    Lefebre, Miss. Ida
    [' Miss.']
    Sdycoff, Mr. Todor
    [' Mr.']
    남성
    Hart, Mr. Henry
    [' Mr.']
    남성
    Minahan, Miss. Daisy E
    [' Miss.']
    Cunningham, Mr. Alfred Fleming
    [' Mr.']
    남성
    Sundman, Mr. Johan Julian
    [' Mr.']
    남성
    Meek, Mrs. Thomas (Annie Louise Rowley)
    [' Mrs.']
    Drew, Mrs. James Vivian (Lulu Thorne Christian)
    [' Mrs.']
    Silven, Miss. Lyyli Karoliina
    [' Miss.']
    Matthews, Mr. William John
    [' Mr.']
    남성
    Van Impe, Miss. Catharina
    [' Miss.']
    Gheorgheff, Mr. Stanio
    [' Mr.']
    남성
    Charters, Mr. David
    [' Mr.']
    남성
    Zimmerman, Mr. Leo
    [' Mr.']
    남성
    Danbom, Mrs. Ernst Gilbert (Anna Sigrid Maria Brogren)
    [' Mrs.']
    Rosblom, Mr. Viktor Richard
    [' Mr.']
    남성
    Wiseman, Mr. Phillippe
    [' Mr.']
    남성
    Clarke, Mrs. Charles V (Ada Maria Winfield)
    [' Mrs.']
    Phillips, Miss. Kate Florence ("Mrs Kate Louise Phillips Marshall")
    [' Miss.']
    Flynn, Mr. James
    [' Mr.']
    남성
    Pickard, Mr. Berk (Berk Trembisky)
    [' Mr.']
    남성
    Bjornstrom-Steffansson, Mr. Mauritz Hakan
    [' Mr.']
    남성
    Thorneycroft, Mrs. Percival (Florence Kate White)
    [' Mrs.']
    Louch, Mrs. Charles Alexander (Alice Adelaide Slow)
    [' Mrs.']
    Kallio, Mr. Nikolai Erland
    [' Mr.']
    남성
    Silvey, Mr. William Baird
    [' Mr.']
    남성
    Carter, Miss. Lucile Polk
    [' Miss.']
    Ford, Miss. Doolina Margaret "Daisy"
    [' Miss.']
    Richards, Mrs. Sidney (Emily Hocking)
    [' Mrs.']
    Fortune, Mr. Mark
    [' Mr.']
    남성
    Kvillner, Mr. Johan Henrik Johannesson
    [' Mr.']
    남성
    Hart, Mrs. Benjamin (Esther Ada Bloomfield)
    [' Mrs.']
    Hampe, Mr. Leon
    [' Mr.']
    남성
    Petterson, Mr. Johan Emil
    [' Mr.']
    남성
    Reynaldo, Ms. Encarnacion
    [' Ms.']
    Johannesen-Bratthammer, Mr. Bernt
    [' Mr.']
    남성
    Dodge, Master. Washington
    [' Master.']
    Mellinger, Miss. Madeleine Violet
    [' Miss.']
    Seward, Mr. Frederic Kimber
    [' Mr.']
    남성
    Baclini, Miss. Marie Catherine
    [' Miss.']
    Peuchen, Major. Arthur Godfrey
    [' Major.']
    West, Mr. Edwy Arthur
    [' Mr.']
    남성
    Hagland, Mr. Ingvald Olai Olsen
    [' Mr.']
    남성
    Foreman, Mr. Benjamin Laventall
    [' Mr.']
    남성
    Goldenberg, Mr. Samuel L
    [' Mr.']
    남성
    Peduzzi, Mr. Joseph
    [' Mr.']
    남성
    Jalsevac, Mr. Ivan
    [' Mr.']
    남성
    Millet, Mr. Francis Davis
    [' Mr.']
    남성
    Kenyon, Mrs. Frederick R (Marion)
    [' Mrs.']
    Toomey, Miss. Ellen
    [' Miss.']
    O'Connor, Mr. Maurice
    [' Mr.']
    남성
    Anderson, Mr. Harry
    [' Mr.']
    남성
    Morley, Mr. William
    [' Mr.']
    남성
    Gee, Mr. Arthur H
    [' Mr.']
    남성
    Milling, Mr. Jacob Christian
    [' Mr.']
    남성
    Maisner, Mr. Simon
    [' Mr.']
    남성
    Goncalves, Mr. Manuel Estanslas
    [' Mr.']
    남성
    Campbell, Mr. William
    [' Mr.']
    남성
    Smart, Mr. John Montgomery
    [' Mr.']
    남성
    Scanlan, Mr. James
    [' Mr.']
    남성
    Baclini, Miss. Helene Barbara
    [' Miss.']
    Keefe, Mr. Arthur
    [' Mr.']
    남성
    Cacic, Mr. Luka
    [' Mr.']
    남성
    West, Mrs. Edwy Arthur (Ada Mary Worth)
    [' Mrs.']
    Jerwan, Mrs. Amin S (Marie Marthe Thuillard)
    [' Mrs.']
    Strandberg, Miss. Ida Sofia
    [' Miss.']
    Clifford, Mr. George Quincy
    [' Mr.']
    남성
    Renouf, Mr. Peter Henry
    [' Mr.']
    남성
    Braund, Mr. Lewis Richard
    [' Mr.']
    남성
    Karlsson, Mr. Nils August
    [' Mr.']
    남성
    Hirvonen, Miss. Hildur E
    [' Miss.']
    Goodwin, Master. Harold Victor
    [' Master.']
    Frost, Mr. Anthony Wood "Archie"
    [' Mr.']
    남성
    Rouse, Mr. Richard Henry
    [' Mr.']
    남성
    Turkula, Mrs. (Hedwig)
    [' Mrs.']
    Bishop, Mr. Dickinson H
    [' Mr.']
    남성
    Lefebre, Miss. Jeannie
    [' Miss.']
    Hoyt, Mrs. Frederick Maxfield (Jane Anne Forby)
    [' Mrs.']
    Kent, Mr. Edward Austin
    [' Mr.']
    남성
    Somerton, Mr. Francis William
    [' Mr.']
    남성
    Coutts, Master. Eden Leslie "Neville"
    [' Master.']
    Hagland, Mr. Konrad Mathias Reiersen
    [' Mr.']
    남성
    Windelov, Mr. Einar
    [' Mr.']
    남성
    Molson, Mr. Harry Markland
    [' Mr.']
    남성
    Artagaveytia, Mr. Ramon
    [' Mr.']
    남성
    Stanley, Mr. Edward Roland
    [' Mr.']
    남성
    Yousseff, Mr. Gerious
    [' Mr.']
    남성
    Eustis, Miss. Elizabeth Mussey
    [' Miss.']
    Shellard, Mr. Frederick William
    [' Mr.']
    남성
    Allison, Mrs. Hudson J C (Bessie Waldo Daniels)
    [' Mrs.']
    Svensson, Mr. Olof
    [' Mr.']
    남성
    Calic, Mr. Petar
    [' Mr.']
    남성
    Canavan, Miss. Mary
    [' Miss.']
    O'Sullivan, Miss. Bridget Mary
    [' Miss.']
    Laitinen, Miss. Kristina Sofia
    [' Miss.']
    Maioni, Miss. Roberta
    [' Miss.']
    Penasco y Castellana, Mr. Victor de Satode
    [' Mr.']
    남성
    Quick, Mrs. Frederick Charles (Jane Richards)
    [' Mrs.']
    Bradley, Mr. George ("George Arthur Brayton")
    [' Mr.']
    남성
    Olsen, Mr. Henry Margido
    [' Mr.']
    남성
    Lang, Mr. Fang
    [' Mr.']
    남성
    Daly, Mr. Eugene Patrick
    [' Mr.']
    남성
    Webber, Mr. James
    [' Mr.']
    남성
    McGough, Mr. James Robert
    [' Mr.']
    남성
    Rothschild, Mrs. Martin (Elizabeth L. Barrett)
    [' Mrs.', ' L.']
    Coleff, Mr. Satio
    [' Mr.']
    남성
    Walker, Mr. William Anderson
    [' Mr.']
    남성
    Lemore, Mrs. (Amelia Milley)
    [' Mrs.']
    Ryan, Mr. Patrick
    [' Mr.']
    남성
    Angle, Mrs. William A (Florence "Mary" Agnes Hughes)
    [' Mrs.']
    Pavlovic, Mr. Stefo
    [' Mr.']
    남성
    Perreault, Miss. Anne
    [' Miss.']
    Vovk, Mr. Janko
    [' Mr.']
    남성
    Lahoud, Mr. Sarkis
    [' Mr.']
    남성
    Hippach, Mrs. Louis Albert (Ida Sophia Fischer)
    [' Mrs.']
    Kassem, Mr. Fared
    [' Mr.']
    남성
    Farrell, Mr. James
    [' Mr.']
    남성
    Ridsdale, Miss. Lucy
    [' Miss.']
    Farthing, Mr. John
    [' Mr.']
    남성
    Salonen, Mr. Johan Werner
    [' Mr.']
    남성
    Hocking, Mr. Richard George
    [' Mr.']
    남성
    Quick, Miss. Phyllis May
    [' Miss.']
    Toufik, Mr. Nakli
    [' Mr.']
    남성
    Elias, Mr. Joseph Jr
    [' Mr.']
    남성
    Peter, Mrs. Catherine (Catherine Rizk)
    [' Mrs.']
    Cacic, Miss. Marija
    [' Miss.']
    Hart, Miss. Eva Miriam
    [' Miss.']
    Butt, Major. Archibald Willingham
    [' Major.']
    LeRoy, Miss. Bertha
    [' Miss.']
    Risien, Mr. Samuel Beard
    [' Mr.']
    남성
    Frolicher, Miss. Hedwig Margaritha
    [' Miss.']
    Crosby, Miss. Harriet R
    [' Miss.']
    Andersson, Miss. Ingeborg Constanzia
    [' Miss.']
    Andersson, Miss. Sigrid Elisabeth
    [' Miss.']
    Beane, Mr. Edward
    [' Mr.']
    남성
    Douglas, Mr. Walter Donald
    [' Mr.']
    남성
    Nicholson, Mr. Arthur Ernest
    [' Mr.']
    남성
    Beane, Mrs. Edward (Ethel Clarke)
    [' Mrs.']
    Padro y Manent, Mr. Julian
    [' Mr.']
    남성
    Goldsmith, Mr. Frank John
    [' Mr.']
    남성
    Davies, Master. John Morgan Jr
    [' Master.']
    Thayer, Mr. John Borland Jr
    [' Mr.']
    남성
    Sharp, Mr. Percival James R
    [' Mr.']
    남성
    O'Brien, Mr. Timothy
    [' Mr.']
    남성
    Leeni, Mr. Fahim ("Philip Zenni")
    [' Mr.']
    남성
    Ohman, Miss. Velin
    [' Miss.']
    Wright, Mr. George
    [' Mr.']
    남성
    Duff Gordon, Lady. (Lucille Christiana Sutherland) ("Mrs Morgan")
    [' Lady.']
    Robbins, Mr. Victor
    [' Mr.']
    남성
    Taussig, Mrs. Emil (Tillie Mandelbaum)
    [' Mrs.']
    de Messemaeker, Mrs. Guillaume Joseph (Emma)
    [' Mrs.']
    Morrow, Mr. Thomas Rowan
    [' Mr.']
    남성
    Sivic, Mr. Husein
    [' Mr.']
    남성
    Norman, Mr. Robert Douglas
    [' Mr.']
    남성
    Simmons, Mr. John
    [' Mr.']
    남성
    Meanwell, Miss. (Marion Ogden)
    [' Miss.']
    Davies, Mr. Alfred J
    [' Mr.']
    남성
    Stoytcheff, Mr. Ilia
    [' Mr.']
    남성
    Palsson, Mrs. Nils (Alma Cornelia Berglund)
    [' Mrs.']
    Doharr, Mr. Tannous
    [' Mr.']
    남성
    Jonsson, Mr. Carl
    [' Mr.']
    남성
    Harris, Mr. George
    [' Mr.']
    남성
    Appleton, Mrs. Edward Dale (Charlotte Lamson)
    [' Mrs.']
    Flynn, Mr. John Irwin ("Irving")
    [' Mr.']
    남성
    Kelly, Miss. Mary
    [' Miss.']
    Rush, Mr. Alfred George John
    [' Mr.']
    남성
    Patchett, Mr. George
    [' Mr.']
    남성
    Garside, Miss. Ethel
    [' Miss.']
    Silvey, Mrs. William Baird (Alice Munger)
    [' Mrs.']
    Caram, Mrs. Joseph (Maria Elias)
    [' Mrs.']
    Jussila, Mr. Eiriik
    [' Mr.']
    남성
    Christy, Miss. Julie Rachel
    [' Miss.']
    Thayer, Mrs. John Borland (Marian Longstreth Morris)
    [' Mrs.']
    Downton, Mr. William James
    [' Mr.']
    남성
    Ross, Mr. John Hugo
    [' Mr.']
    남성
    Paulner, Mr. Uscher
    [' Mr.']
    남성
    Taussig, Miss. Ruth
    [' Miss.']
    Jarvis, Mr. John Denzil
    [' Mr.']
    남성
    Frolicher-Stehli, Mr. Maxmillian
    [' Mr.']
    남성
    Gilinski, Mr. Eliezer
    [' Mr.']
    남성
    Murdlin, Mr. Joseph
    [' Mr.']
    남성
    Rintamaki, Mr. Matti
    [' Mr.']
    남성
    Stephenson, Mrs. Walter Bertram (Martha Eustis)
    [' Mrs.']
    Elsbury, Mr. William James
    [' Mr.']
    남성
    Bourke, Miss. Mary
    [' Miss.']
    Chapman, Mr. John Henry
    [' Mr.']
    남성
    Van Impe, Mr. Jean Baptiste
    [' Mr.']
    남성
    Leitch, Miss. Jessie Wills
    [' Miss.']
    Johnson, Mr. Alfred
    [' Mr.']
    남성
    Boulos, Mr. Hanna
    [' Mr.']
    남성
    Duff Gordon, Sir. Cosmo Edmund ("Mr Morgan")
    [' Sir.']
    Jacobsohn, Mrs. Sidney Samuel (Amy Frances Christy)
    [' Mrs.']
    Slabenoff, Mr. Petco
    [' Mr.']
    남성
    Harrington, Mr. Charles H
    [' Mr.']
    남성
    Torber, Mr. Ernst William
    [' Mr.']
    남성
    Homer, Mr. Harry ("Mr E Haven")
    [' Mr.']
    남성
    Lindell, Mr. Edvard Bengtsson
    [' Mr.']
    남성
    Karaic, Mr. Milan
    [' Mr.']
    남성
    Daniel, Mr. Robert Williams
    [' Mr.']
    남성
    Laroche, Mrs. Joseph (Juliette Marie Louise Lafargue)
    [' Mrs.']
    Shutes, Miss. Elizabeth W
    [' Miss.']
    Andersson, Mrs. Anders Johan (Alfrida Konstantia Brogren)
    [' Mrs.']
    Jardin, Mr. Jose Neto
    [' Mr.']
    남성
    Murphy, Miss. Margaret Jane
    [' Miss.']
    Horgan, Mr. John
    [' Mr.']
    남성
    Brocklebank, Mr. William Alfred
    [' Mr.']
    남성
    Herman, Miss. Alice
    [' Miss.']
    Danbom, Mr. Ernst Gilbert
    [' Mr.']
    남성
    Lobb, Mrs. William Arthur (Cordelia K Stanlick)
    [' Mrs.']
    Becker, Miss. Marion Louise
    [' Miss.']
    Gavey, Mr. Lawrence
    [' Mr.']
    남성
    Yasbeck, Mr. Antoni
    [' Mr.']
    남성
    Kimball, Mr. Edwin Nelson Jr
    [' Mr.']
    남성
    Nakid, Mr. Sahid
    [' Mr.']
    남성
    Hansen, Mr. Henry Damsgaard
    [' Mr.']
    남성
    Bowen, Mr. David John "Dai"
    [' Mr.']
    남성
    Sutton, Mr. Frederick
    [' Mr.']
    남성
    Kirkland, Rev. Charles Leonard
    [' Rev.']
    Longley, Miss. Gretchen Fiske
    [' Miss.']
    Bostandyeff, Mr. Guentcho
    [' Mr.']
    남성
    O'Connell, Mr. Patrick D
    [' Mr.']
    남성
    Barkworth, Mr. Algernon Henry Wilson
    [' Mr.']
    남성
    Lundahl, Mr. Johan Svensson
    [' Mr.']
    남성
    Stahelin-Maeglin, Dr. Max
    [' Dr.']
    Parr, Mr. William Henry Marsh
    [' Mr.']
    남성
    Skoog, Miss. Mabel
    [' Miss.']
    Davis, Miss. Mary
    [' Miss.']
    Leinonen, Mr. Antti Gustaf
    [' Mr.']
    남성
    Collyer, Mr. Harvey
    [' Mr.']
    남성
    Panula, Mrs. Juha (Maria Emilia Ojala)
    [' Mrs.']
    Thorneycroft, Mr. Percival
    [' Mr.']
    남성
    Jensen, Mr. Hans Peder
    [' Mr.']
    남성
    Sagesser, Mlle. Emma
    [' Mlle.']
    Skoog, Miss. Margit Elizabeth
    [' Miss.']
    Foo, Mr. Choong
    [' Mr.']
    남성
    Baclini, Miss. Eugenie
    [' Miss.']
    Harper, Mr. Henry Sleeper
    [' Mr.']
    남성
    Cor, Mr. Liudevit
    [' Mr.']
    남성
    Simonius-Blumer, Col. Oberst Alfons
    [' Col.']
    Willey, Mr. Edward
    [' Mr.']
    남성
    Stanley, Miss. Amy Zillah Elsie
    [' Miss.']
    Mitkoff, Mr. Mito
    [' Mr.']
    남성
    Doling, Miss. Elsie
    [' Miss.']
    Kalvik, Mr. Johannes Halvorsen
    [' Mr.']
    남성
    O'Leary, Miss. Hanora "Norah"
    [' Miss.']
    Hegarty, Miss. Hanora "Nora"
    [' Miss.']
    Hickman, Mr. Leonard Mark
    [' Mr.']
    남성
    Radeff, Mr. Alexander
    [' Mr.']
    남성
    Bourke, Mrs. John (Catherine)
    [' Mrs.']
    Eitemiller, Mr. George Floyd
    [' Mr.']
    남성
    Newell, Mr. Arthur Webster
    [' Mr.']
    남성
    Frauenthal, Dr. Henry William
    [' Dr.']
    Badt, Mr. Mohamed
    [' Mr.']
    남성
    Colley, Mr. Edward Pomeroy
    [' Mr.']
    남성
    Coleff, Mr. Peju
    [' Mr.']
    남성
    Lindqvist, Mr. Eino William
    [' Mr.']
    남성
    Hickman, Mr. Lewis
    [' Mr.']
    남성
    Butler, Mr. Reginald Fenton
    [' Mr.']
    남성
    Rommetvedt, Mr. Knud Paust
    [' Mr.']
    남성
    Cook, Mr. Jacob
    [' Mr.']
    남성
    Taylor, Mrs. Elmer Zebley (Juliet Cummins Wright)
    [' Mrs.']
    Brown, Mrs. Thomas William Solomon (Elizabeth Catherine Ford)
    [' Mrs.']
    Davidson, Mr. Thornton
    [' Mr.']
    남성
    Mitchell, Mr. Henry Michael
    [' Mr.']
    남성
    Wilhelms, Mr. Charles
    [' Mr.']
    남성
    Watson, Mr. Ennis Hastings
    [' Mr.']
    남성
    Edvardsson, Mr. Gustaf Hjalmar
    [' Mr.']
    남성
    Sawyer, Mr. Frederick Charles
    [' Mr.']
    남성
    Turja, Miss. Anna Sofia
    [' Miss.']
    Goodwin, Mrs. Frederick (Augusta Tyler)
    [' Mrs.']
    Cardeza, Mr. Thomas Drake Martinez
    [' Mr.']
    남성
    Peters, Miss. Katie
    [' Miss.']
    Hassab, Mr. Hammad
    [' Mr.']
    남성
    Olsvigen, Mr. Thor Anderson
    [' Mr.']
    남성
    Goodwin, Mr. Charles Edward
    [' Mr.']
    남성
    Brown, Mr. Thomas William Solomon
    [' Mr.']
    남성
    Laroche, Mr. Joseph Philippe Lemercier
    [' Mr.']
    남성
    Panula, Mr. Jaako Arnold
    [' Mr.']
    남성
    Dakic, Mr. Branko
    [' Mr.']
    남성
    Fischer, Mr. Eberhard Thelander
    [' Mr.']
    남성
    Madill, Miss. Georgette Alexandra
    [' Miss.']
    Dick, Mr. Albert Adrian
    [' Mr.']
    남성
    Karun, Miss. Manca
    [' Miss.']
    Lam, Mr. Ali
    [' Mr.']
    남성
    Saad, Mr. Khalil
    [' Mr.']
    남성
    Weir, Col. John
    [' Col.']
    Chapman, Mr. Charles Henry
    [' Mr.']
    남성
    Kelly, Mr. James
    [' Mr.']
    남성
    Mullens, Miss. Katherine "Katie"
    [' Miss.']
    Thayer, Mr. John Borland
    [' Mr.']
    남성
    Humblen, Mr. Adolf Mathias Nicolai Olsen
    [' Mr.']
    남성
    Astor, Mrs. John Jacob (Madeleine Talmadge Force)
    [' Mrs.']
    Silverthorne, Mr. Spencer Victor
    [' Mr.']
    남성
    Barbara, Miss. Saiide
    [' Miss.']
    Gallagher, Mr. Martin
    [' Mr.']
    남성
    Hansen, Mr. Henrik Juul
    [' Mr.']
    남성
    Morley, Mr. Henry Samuel ("Mr Henry Marshall")
    [' Mr.']
    남성
    Kelly, Mrs. Florence "Fannie"
    [' Mrs.']
    Calderhead, Mr. Edward Pennington
    [' Mr.']
    남성
    Cleaver, Miss. Alice
    [' Miss.']
    Moubarek, Master. Halim Gonios ("William George")
    [' Master.']
    Mayne, Mlle. Berthe Antonine ("Mrs de Villiers")
    [' Mlle.']
    Klaber, Mr. Herman
    [' Mr.']
    남성
    Taylor, Mr. Elmer Zebley
    [' Mr.']
    남성
    Larsson, Mr. August Viktor
    [' Mr.']
    남성
    Greenberg, Mr. Samuel
    [' Mr.']
    남성
    Soholt, Mr. Peter Andreas Lauritz Andersen
    [' Mr.']
    남성
    Endres, Miss. Caroline Louise
    [' Miss.']
    Troutt, Miss. Edwina Celia "Winnie"
    [' Miss.']
    McEvoy, Mr. Michael
    [' Mr.']
    남성
    Johnson, Mr. Malkolm Joackim
    [' Mr.']
    남성
    Harper, Miss. Annie Jessie "Nina"
    [' Miss.']
    Jensen, Mr. Svend Lauritz
    [' Mr.']
    남성
    Gillespie, Mr. William Henry
    [' Mr.']
    남성
    Hodges, Mr. Henry Price
    [' Mr.']
    남성
    Chambers, Mr. Norman Campbell
    [' Mr.']
    남성
    Oreskovic, Mr. Luka
    [' Mr.']
    남성
    Renouf, Mrs. Peter Henry (Lillian Jefferys)
    [' Mrs.']
    Mannion, Miss. Margareth
    [' Miss.']
    Bryhl, Mr. Kurt Arnold Gottfrid
    [' Mr.']
    남성
    Ilmakangas, Miss. Pieta Sofia
    [' Miss.']
    Allen, Miss. Elisabeth Walton
    [' Miss.']
    Hassan, Mr. Houssein G N
    [' Mr.']
    남성
    Knight, Mr. Robert J
    [' Mr.']
    남성
    Berriman, Mr. William John
    [' Mr.']
    남성
    Troupiansky, Mr. Moses Aaron
    [' Mr.']
    남성
    Williams, Mr. Leslie
    [' Mr.']
    남성
    Ford, Mrs. Edward (Margaret Ann Watson)
    [' Mrs.']
    Lesurer, Mr. Gustave J
    [' Mr.']
    남성
    Ivanoff, Mr. Kanio
    [' Mr.']
    남성
    Nankoff, Mr. Minko
    [' Mr.']
    남성
    Hawksford, Mr. Walter James
    [' Mr.']
    남성
    Cavendish, Mr. Tyrell William
    [' Mr.']
    남성
    Ryerson, Miss. Susan Parker "Suzette"
    [' Miss.']
    McNamee, Mr. Neal
    [' Mr.']
    남성
    Stranden, Mr. Juho
    [' Mr.']
    남성
    Crosby, Capt. Edward Gifford
    [' Capt.']
    Abbott, Mr. Rossmore Edward
    [' Mr.']
    남성
    Sinkkonen, Miss. Anna
    [' Miss.']
    Marvin, Mr. Daniel Warner
    [' Mr.']
    남성
    Connaghton, Mr. Michael
    [' Mr.']
    남성
    Wells, Miss. Joan
    [' Miss.']
    Moor, Master. Meier
    [' Master.']
    Vande Velde, Mr. Johannes Joseph
    [' Mr.']
    남성
    Jonkoff, Mr. Lalio
    [' Mr.']
    남성
    Herman, Mrs. Samuel (Jane Laver)
    [' Mrs.']
    Hamalainen, Master. Viljo
    [' Master.']
    Carlsson, Mr. August Sigfrid
    [' Mr.']
    남성
    Bailey, Mr. Percy Andrew
    [' Mr.']
    남성
    Theobald, Mr. Thomas Leonard
    [' Mr.']
    남성
    Rothes, the Countess. of (Lucy Noel Martha Dyer-Edwards)
    [' Countess.']
    Garfirth, Mr. John
    [' Mr.']
    남성
    Nirva, Mr. Iisakki Antino Aijo
    [' Mr.']
    남성
    Barah, Mr. Hanna Assi
    [' Mr.']
    남성
    Carter, Mrs. William Ernest (Lucile Polk)
    [' Mrs.']
    Eklund, Mr. Hans Linus
    [' Mr.']
    남성
    Hogeboom, Mrs. John C (Anna Andrews)
    [' Mrs.']
    Brewe, Dr. Arthur Jackson
    [' Dr.']
    Mangan, Miss. Mary
    [' Miss.']
    Moran, Mr. Daniel J
    [' Mr.']
    남성
    Gronnestad, Mr. Daniel Danielsen
    [' Mr.']
    남성
    Lievens, Mr. Rene Aime
    [' Mr.']
    남성
    Jensen, Mr. Niels Peder
    [' Mr.']
    남성
    Mack, Mrs. (Mary)
    [' Mrs.']
    Elias, Mr. Dibo
    [' Mr.']
    남성
    Hocking, Mrs. Elizabeth (Eliza Needs)
    [' Mrs.']
    Myhrman, Mr. Pehr Fabian Oliver Malkolm
    [' Mr.']
    남성
    Tobin, Mr. Roger
    [' Mr.']
    남성
    Emanuel, Miss. Virginia Ethel
    [' Miss.']
    Kilgannon, Mr. Thomas J
    [' Mr.']
    남성
    Robert, Mrs. Edward Scott (Elisabeth Walton McMillan)
    [' Mrs.']
    Ayoub, Miss. Banoura
    [' Miss.']
    Dick, Mrs. Albert Adrian (Vera Gillespie)
    [' Mrs.']
    Long, Mr. Milton Clyde
    [' Mr.']
    남성
    Johnston, Mr. Andrew G
    [' Mr.']
    남성
    Ali, Mr. William
    [' Mr.']
    남성
    Harmer, Mr. Abraham (David Lishin)
    [' Mr.']
    남성
    Sjoblom, Miss. Anna Sofia
    [' Miss.']
    Rice, Master. George Hugh
    [' Master.']
    Dean, Master. Bertram Vere
    [' Master.']
    Guggenheim, Mr. Benjamin
    [' Mr.']
    남성
    Keane, Mr. Andrew "Andy"
    [' Mr.']
    남성
    Gaskell, Mr. Alfred
    [' Mr.']
    남성
    Sage, Miss. Stella Anna
    [' Miss.']
    Hoyt, Mr. William Fisher
    [' Mr.']
    남성
    Dantcheff, Mr. Ristiu
    [' Mr.']
    남성
    Otter, Mr. Richard
    [' Mr.']
    남성
    Leader, Dr. Alice (Farnham)
    [' Dr.']
    Osman, Mrs. Mara
    [' Mrs.']
    Ibrahim Shawah, Mr. Yousseff
    [' Mr.']
    남성
    Van Impe, Mrs. Jean Baptiste (Rosalie Paula Govaert)
    [' Mrs.']
    Ponesell, Mr. Martin
    [' Mr.']
    남성
    Collyer, Mrs. Harvey (Charlotte Annie Tate)
    [' Mrs.']
    Carter, Master. William Thornton II
    [' Master.']
    Thomas, Master. Assad Alexander
    [' Master.']
    Hedman, Mr. Oskar Arvid
    [' Mr.']
    남성
    Johansson, Mr. Karl Johan
    [' Mr.']
    남성
    Andrews, Mr. Thomas Jr
    [' Mr.']
    남성
    Pettersson, Miss. Ellen Natalia
    [' Miss.']
    Meyer, Mr. August
    [' Mr.']
    남성
    Chambers, Mrs. Norman Campbell (Bertha Griggs)
    [' Mrs.']
    Alexander, Mr. William
    [' Mr.']
    남성
    Lester, Mr. James
    [' Mr.']
    남성
    Slemen, Mr. Richard James
    [' Mr.']
    남성
    Andersson, Miss. Ebba Iris Alfrida
    [' Miss.']
    Tomlin, Mr. Ernest Portage
    [' Mr.']
    남성
    Fry, Mr. Richard
    [' Mr.']
    남성
    Heininen, Miss. Wendla Maria
    [' Miss.']
    Mallet, Mr. Albert
    [' Mr.']
    남성
    Holm, Mr. John Fredrik Alexander
    [' Mr.']
    남성
    Skoog, Master. Karl Thorsten
    [' Master.']
    Hays, Mrs. Charles Melville (Clara Jennings Gregg)
    [' Mrs.']
    Lulic, Mr. Nikola
    [' Mr.']
    남성
    Reuchlin, Jonkheer. John George
    [' Jonkheer.']
    Moor, Mrs. (Beila)
    [' Mrs.']
    Panula, Master. Urho Abraham
    [' Master.']
    Flynn, Mr. John
    [' Mr.']
    남성
    Lam, Mr. Len
    [' Mr.']
    남성
    Mallet, Master. Andre
    [' Master.']
    McCormack, Mr. Thomas Joseph
    [' Mr.']
    남성
    Stone, Mrs. George Nelson (Martha Evelyn)
    [' Mrs.']
    Yasbeck, Mrs. Antoni (Selini Alexander)
    [' Mrs.']
    Richards, Master. George Sibley
    [' Master.']
    Saad, Mr. Amin
    [' Mr.']
    남성
    Augustsson, Mr. Albert
    [' Mr.']
    남성
    Allum, Mr. Owen George
    [' Mr.']
    남성
    Compton, Miss. Sara Rebecca
    [' Miss.']
    Pasic, Mr. Jakob
    [' Mr.']
    남성
    Sirota, Mr. Maurice
    [' Mr.']
    남성
    Chip, Mr. Chang
    [' Mr.']
    남성
    Marechal, Mr. Pierre
    [' Mr.']
    남성
    Alhomaki, Mr. Ilmari Rudolf
    [' Mr.']
    남성
    Mudd, Mr. Thomas Charles
    [' Mr.']
    남성
    Serepeca, Miss. Augusta
    [' Miss.']
    Lemberopolous, Mr. Peter L
    [' Mr.']
    남성
    Culumovic, Mr. Jeso
    [' Mr.']
    남성
    Abbing, Mr. Anthony
    [' Mr.']
    남성
    Sage, Mr. Douglas Bullen
    [' Mr.']
    남성
    Markoff, Mr. Marin
    [' Mr.']
    남성
    Harper, Rev. John
    [' Rev.']
    Goldenberg, Mrs. Samuel L (Edwiga Grabowska)
    [' Mrs.']
    Andersson, Master. Sigvard Harald Elias
    [' Master.']
    Svensson, Mr. Johan
    [' Mr.']
    남성
    Boulos, Miss. Nourelain
    [' Miss.']
    Lines, Miss. Mary Conover
    [' Miss.']
    Carter, Mrs. Ernest Courtenay (Lilian Hughes)
    [' Mrs.']
    Aks, Mrs. Sam (Leah Rosen)
    [' Mrs.']
    Wick, Mrs. George Dennick (Mary Hitchcock)
    [' Mrs.']
    Daly, Mr. Peter Denis 
    [' Mr.']
    남성
    Baclini, Mrs. Solomon (Latifa Qurban)
    [' Mrs.']
    Razi, Mr. Raihed
    [' Mr.']
    남성
    Hansen, Mr. Claus Peter
    [' Mr.']
    남성
    Giles, Mr. Frederick Edward
    [' Mr.']
    남성
    Swift, Mrs. Frederick Joel (Margaret Welles Barron)
    [' Mrs.']
    Sage, Miss. Dorothy Edith "Dolly"
    [' Miss.']
    Gill, Mr. John William
    [' Mr.']
    남성
    Bystrom, Mrs. (Karolina)
    [' Mrs.']
    Duran y More, Miss. Asuncion
    [' Miss.']
    Roebling, Mr. Washington Augustus II
    [' Mr.']
    남성
    van Melkebeke, Mr. Philemon
    [' Mr.']
    남성
    Johnson, Master. Harold Theodor
    [' Master.']
    Balkic, Mr. Cerin
    [' Mr.']
    남성
    Beckwith, Mrs. Richard Leonard (Sallie Monypeny)
    [' Mrs.']
    Carlsson, Mr. Frans Olof
    [' Mr.']
    남성
    Vander Cruyssen, Mr. Victor
    [' Mr.']
    남성
    Abelson, Mrs. Samuel (Hannah Wizosky)
    [' Mrs.']
    Najib, Miss. Adele Kiamie "Jane"
    [' Miss.']
    Gustafsson, Mr. Alfred Ossian
    [' Mr.']
    남성
    Petroff, Mr. Nedelio
    [' Mr.']
    남성
    Laleff, Mr. Kristo
    [' Mr.']
    남성
    Potter, Mrs. Thomas Jr (Lily Alexenia Wilson)
    [' Mrs.']
    Shelley, Mrs. William (Imanita Parrish Hall)
    [' Mrs.']
    Markun, Mr. Johann
    [' Mr.']
    남성
    Dahlberg, Miss. Gerda Ulrika
    [' Miss.']
    Banfield, Mr. Frederick James
    [' Mr.']
    남성
    Sutehall, Mr. Henry Jr
    [' Mr.']
    남성
    Rice, Mrs. William (Margaret Norton)
    [' Mrs.']
    Montvila, Rev. Juozas
    [' Rev.']
    Graham, Miss. Margaret Edith
    [' Miss.']
    Johnston, Miss. Catherine Helen "Carrie"
    [' Miss.']
    Behr, Mr. Karl Howell
    [' Mr.']
    남성
    Dooley, Mr. Patrick
    [' Mr.']
    남성


### ' [A-Za-z]+\.' --> regular expression 이라고 함
* 특정한 규칙을 가진 문자열의 집합을 표현하는 데 사용하는 형식

<table>
    <thead>
        <tr style="font-size:1.2em">
            <th style="text-align:center">정규 표현식</th>
            <th style="text-align:center">축약 표현</th>
            <th style="text-align:left">사용 예</th>
        </tr>
    </thead>
    <tbody>
        <tr style="font-size:1.2em">
            <td style="text-align:center">[0-9]</td>
            <td style="text-align:center">\d</td>
            <td style="text-align:left">숫자를 찾음</td>
        </tr>
        <tr style="font-size:1.2em">
            <td style="text-align:center">[^0-9]</td>
            <td style="text-align:center">\D</td>
            <td style="text-align:left">숫자가 아닌 것을 찾음(텍스트, 특수 문자, white space(스페이스, 탭, 엔터 등등)를 찾을 때)</td>
        </tr>
        <tr style="font-size:1.2em">
            <td style="text-align:center">[ \t\n\r\f\v]</td>
            <td style="text-align:center">\s</td>
            <td style="text-align:left">white space(스페이스, 탭, 엔터 등등) 문자인 것을 찾음</td>
        </tr>
        <tr style="font-size:1.2em">
            <td style="text-align:center">[^ \t\n\r\f\v]</td>
            <td style="text-align:center">\S</td>
            <td style="text-align:left">white space(스페이스, 탭, 엔터 등등) 문자가 아닌 것을 찾음(텍스트, 특수 문자, 숫자를 찾을 때)</td>
        </tr>
        <tr style="font-size:1.2em">
            <td style="text-align:center">[A-Za-z0-9]</td>
            <td style="text-align:center">\w</td>
            <td style="text-align:left">문자, 숫자를 찾음</td>
        </tr>
        <tr style="font-size:1.2em">
            <td style="text-align:center">[^A-Za-z0-9]</td>
            <td style="text-align:center">\W</td>
            <td style="text-align:left">문자, 숫자가 아닌 것을 찾음</td>
        </tr>
    </tbody>
</table>

### 예: 문자, 숫자가 아닌 데이터를 찾아서, '' 로 대체해라(삭제해라)


```python
import re
string = "(Dave)"
re.sub('[^A-Za-z0-9]', '', string)    # 문자, 숫자가 아닌 데이터를 찾아서, '' 로 대체해라(삭제해라)
```




    'Dave'




```python
string.replace("(", '').replace(")", '')
```




    'Dave'



## 정규표현식 확인: https://regexr.com/


```python

```

### 위 정규 표현식은 일정한 규칙을 가지고 작성됨, 필요한 패턴은 직접 만들 수도 있음

### 1. Dot \.
* Dot \. 메타 문자는 줄바꿈 문자인 \n를 제외한 모든 문자(한 개)를 의미함
* 예: D.A 는 D + 모든 문자(한 개) + A 를 의미
  - DAA, DvA, D1A

### 정규 표현식 라이브러리 임포트하기


```python
import re
```

### 정규 표현식 패턴 만들기


```python
pattern = re.compile('D.A')
```

### 패턴에 매칭되는지 여부 확인하기 (실습)


```python
pattern.search("DAA")
```




    <re.Match object; span=(0, 3), match='DAA'>




```python
pattern.search("D1A")
```




    <re.Match object; span=(0, 3), match='D1A'>




```python
pattern.search("D00A")
```


```python
pattern.search("DA")
```


```python
pattern.search("d0A")
```


```python
pattern.search("d0A D1A 0111")
```




    <re.Match object; span=(4, 7), match='D1A'>



### 정말 Dot . 이 들어간 패턴을 찾으려면?
<pre>
\. 으로 표시하거나, [.] 으로 표시하면 됨
</pre>


```python
pattern = re.compile('D\.A')
```


```python
pattern.search("D.A")
```




    <re.Match object; span=(0, 3), match='D.A'>




```python
pattern.search("DDA")
```


```python
pattern = re.compile('D[.]A')
```

### 찾고 바꾸기 (특정 패턴이 매칭되는 것을 찾아서, 다른 문자열로 바꾸기)


```python
string = "DDA D1A DDA DA"
```


```python
# re.sub(패턴, 바꿀데이터, 원본데이터)
re.sub('D.A', 'Dave', string)    # 문자, 숫자가 아닌 데이터를 찾아서, '' 로 대체해라(삭제해라)
```




    'Dave Dave Dave DA'



### 2. 반복 ? , \* , +
* ? 는 앞 문자가 0번 또는 1번 표시되는 패턴 (없어도 되고, 한번 있어도 되는 패턴)
* \* 는 앞 문자가 0번 또는 그 이상 반복되는 패턴
* \+ 는 앞 문자가 1번 또는 그 이상 반복되는 패턴

### ? 사용 예


```python
pattern = re.compile('D?A')     # 앞에 문자 D가 없거나, 여러번 반복되고 마지막이 A 인 문자열
```


```python
pattern.search("A")
```




    <re.Match object; span=(0, 1), match='A'>




```python
pattern.search("DA")
```




    <re.Match object; span=(0, 2), match='DA'>




```python
pattern.search("DDDDDDA")
```




    <re.Match object; span=(5, 7), match='DA'>




```python
pattern.search('DDA')
```




    <re.Match object; span=(1, 3), match='DA'>




```python
pattern.search('DAAAA')
```




    <re.Match object; span=(0, 2), match='DA'>



### \* 사용 예


```python
pattern = re.compile('D*A')     # 앞에 문자 D가 없거나, 여러번 반복되고 마지막이 A 인 문자열
```


```python
pattern.search("A")
```




    <re.Match object; span=(0, 1), match='A'>




```python
pattern.search("DA")
```




    <re.Match object; span=(0, 2), match='DA'>




```python
pattern.search("DDDDDDDDDDDDDDDDDDDDDDDDDDDDA")
```




    <re.Match object; span=(0, 29), match='DDDDDDDDDDDDDDDDDDDDDDDDDDDDA'>




```python
pattern = re.compile('AD*A')     # 앞에 문자 D가 없거나, 여러번 반복되고 마지막이 A 인 문자열
```


```python
pattern.search("ADA")
```




    <re.Match object; span=(0, 3), match='ADA'>




```python
pattern.search("ADDDDDDDDDDDDDDDDDDA")
```




    <re.Match object; span=(0, 20), match='ADDDDDDDDDDDDDDDDDDA'>




```python

```

* 참고: https://regexr.com/

### + 사용 예


```python
pattern = re.compile('D+A')
```


```python
pattern.search("A")
```


```python
pattern.search("DA")
```




    <re.Match object; span=(0, 2), match='DA'>




```python
pattern.search("DDDDDDDDDDDDDDDDDDDDDDDDDDDDA")
```




    <re.Match object; span=(0, 29), match='DDDDDDDDDDDDDDDDDDDDDDDDDDDDA'>



### 또다른 반복 표현: {n}, {m,n}
* {n} : 앞 문자가 n 번 반복되는 패턴
* {m, n} : 앞 문자가 m 번 반복되는 패턴부터 n 번 반복되는 패턴까지

### {n} 사용 예


```python
pattern = re.compile('AD{2}A')
```


```python
pattern.search("ADA")
```


```python
pattern.search("ADDA")
```




    <re.Match object; span=(0, 4), match='ADDA'>




```python
pattern.search("ADDDA")
```

### {m,n} 사용 예


```python
pattern = re.compile('AD{2,6}A')    # {m,n} 은 붙여 써야 함 {m, n} 으로 쓰면 안됨(특이함)
```


```python
pattern.search("ADDA")
```




    <re.Match object; span=(0, 4), match='ADDA'>




```python
pattern.search("ADDDA")
```




    <re.Match object; span=(0, 5), match='ADDDA'>




```python
pattern.search("ADDDDDDA")
```




    <re.Match object; span=(0, 8), match='ADDDDDDA'>



### 3. [ ] 괄호 : 괄호 안에 들어가는 문자가 들어 있는 패턴
* 예:  [abc] 는 a, b, c 중 하나가 들어 있는 패턴을 말함


```python
pattern = re.compile('[abcdefgABCDEFG]')    
```


```python
pattern.search("a1234")
```




    <re.Match object; span=(0, 1), match='a'>




```python
pattern.search("z1234")  
```

### 하이픈(-)을 이용하면 알파벳 전체를 나타낼 수 있음
* 예:  [a-c] 는 a, b, c 중 하나가 들어 있는 패턴을 말함


```python
pattern = re.compile('[a-z]')    
```


```python
pattern.search("k1234")  
```




    <re.Match object; span=(0, 1), match='k'>




```python
pattern.search("Z1234")  
```

### [a-zA-Z] 으로 표기하면 대소문자를 모두 포함해서 알파벳 전체를 나타낼 수 있음


```python
pattern = re.compile('[a-zA-Z]') 
```


```python
pattern.search("Z1234")  
```




    <re.Match object; span=(0, 1), match='Z'>



### [a-zA-Z0-9] 로 표기하면 대소문자를 모두 포함해서 알파벳 전체와 함께 숫자 전체도 나타낼 수 있음


```python
pattern = re.compile('[a-zA-Z0-9]') 
```


```python
pattern.search("1234---") 
```




    <re.Match object; span=(0, 1), match='1'>




```python
pattern.search("---------------!@#!@$!$%#%%%#%%@$!$!---") 
```

### [ ] 괄호 안에서 [ 바로 뒤에 ^ 을 쓰면 그 뒤에 오는 문자가 아닌 패턴을 찾음
* 문자를 결국 알파벳, 숫자, 특수문자, whitespace(스페이스, 탭, 엔터등) 로 분류할 수 있으므로
* [^ \t\n\r\f\v] 는 이중에서 whitespace 가 아닌 알파벳, 숫자, 특수문자를 지칭함


```python
pattern = re.compile('[^a-zA-Z0-9]') 
```


```python
pattern.search("---------------!@#!@$!$%#%%%#%%@$!$!---") 
```




    <re.Match object; span=(0, 1), match='-'>




```python

```


```python
pattern = re.compile('[^ \t\n\r\f\v]') 
```


```python
pattern.search("-") 
```




    <re.Match object; span=(0, 1), match='-'>




```python
pattern.search(" ")
```

### 그러면 한글만? --> [가-힣]


```python
pattern = re.compile('[가-힣]') 
```


```python
pattern.search("안") 
```




    <re.Match object; span=(0, 1), match='안'>



### 4. 조합해서 써보자


```python
import re
pattern = re.compile('[a-zA-Z]+')
matched = pattern.search("Dave")
print(matched)
```

    <re.Match object; span=(0, 4), match='Dave'>


### 5. 정규 표현식 라이브러리 함수 사용법

### match 와 search 함수
* match : 문자열 처음부터 정규식과 매칭되는 패턴을 찾아서 리턴
* search : 문자열 전체를 검색해서 정규식과 매칭되는 패턴을 찾아서 리턴


```python
import re
pattern = re.compile('[a-z]+')
```


```python
matched = pattern.match('Dave')
```


```python
searched = pattern.search("Dave")
```


```python
print (matched)
```

    None



```python
print (searched)
```

    <re.Match object; span=(1, 4), match='ave'>


### findall 함수: 정규표현식과 매칭되는 모든 문자열을 리스트 객체로 리턴함


```python
import re
pattern = re.compile('[a-z]+')
findalled = pattern.findall('Game of Life in Python')
```


```python
print (findalled)
```

    ['ame', 'of', 'ife', 'in', 'ython']



```python
pattern2 = re.compile('[A-Za-z]+')
```


```python
findalled2 = pattern2.findall('Game of Life in Python')
```


```python
print (findalled2)
```

    ['Game', 'of', 'Life', 'in', 'Python']


### findall 함수를 사용해서 정규표현식에 해당되는 문자열이 있는지 없는지 확인하기


```python
import re
pattern = re.compile('[a-z]+')
findalled = pattern.findall('GAME')
if len(findalled) > 0:
    print ("정규표현식에 맞는 문자열이 존재함")
else:
    print ("정규표현식에 맞는 문자열이 존재하지 않음")
```

    정규표현식에 맞는 문자열이 존재하지 않음



```python

```

### split 함수: 찾은 정규표현식 패턴 문자열을 기준으로 문자열을 분리


```python
import re
pattern2 = re.compile(':')
splited = pattern2.split('python:java')
```


```python
print (splited)
```

    ['python', 'java']


<div class="alert alert-block alert-success">
<strong><font color="blue" size="4em">프로그래밍 연습</font></strong><br>
' VS ' 로 문자열 앞뒤를 분리해보기 
</div>


```python
import re
pattern2 = re.compile(' [A-Z]{2} ')
splited = pattern2.split('python VS java')
print (splited)
```

    ['python', 'java']


### sub 함수: 찾은 정규표현식 패턴 문자열을 다른 문자열로 변경


```python
import re
pattern2 = re.compile('-')
subed = pattern2.sub('*', '801210-1011323')  # sub(바꿀문자열, 본래문자열)
```


```python
print (subed)
```

    801210*1011323



```python
subed = re.sub('-', '*', '801210-1011323')  # sub(정규표현식, 바꿀문자열, 본래문자열)
```


```python
print (subed)
```

    801210*1011323



```python

```


```python
801210-*******
```

<div class="alert alert-block alert-success">
<strong><font color="blue" size="4em">도전 과제</font></strong><br>
주민번호 뒷자리를 * 로 바꿔서 가려보기<br>
re.sub('-------', '------', each_row[1].value)   <--- 정규표현식, 바꿀문자열 을 넣어봅니다.
</div>
<pre>
import openpyxl
work_book = openpyxl.load_workbook('data_kr.xlsx')
work_sheet = work_book.active
for each_row in work_sheet.rows:
    print(re.sub('-------', '------', each_row[1].value))

work_book.close()
</pre>


```python
import openpyxl
work_book = openpyxl.load_workbook('data_kr.xlsx')
work_sheet = work_book.active
for each_row in work_sheet.rows:
    print(re.sub('-[0-9]{7}', '-*******', each_row[1].value))

work_book.close()
```

    주민등록번호
    800215-*******
    821030-*******
    841230-*******
    790903-*******
    800125-*******
    820612-*******

