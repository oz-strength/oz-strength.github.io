---
layout: single
title: "Text PreProcessing"
categories: python
tag: [colab, ai]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# 표제어 추출 (Lemmatization)

말뭉치(코퍼스)의 단어의 개수를 줄일 수 있는 기법 

be 동사 : be, am, are, is, ...

공부하다 : 공부하고, 공부때문에, 공부해서, 공부덕분에, ...

- 분석시에 단어 빈도수 기반으로 진행 => 자연어 처리 단계에서 상당히 자주 사용 

- 형태소로부터 단어를 만들어가는 : 형태학
  - 어간(stem) : 의미가 있는 단어의 핵심부분
  - 접사(affix) : 단어에 추가적인 의미가 부여되는 부분 

  형태학적 파싱 : 코퍼스에서 어간과 접사를 분리하는 것 

  ex) students => student + s

```python
import nltk # 자연어 처리를 위한 패키지
nltk.download('wordnet')
```

```
[nltk_data] Downloading package wordnet to /root/nltk_data...
True
```

```python
# WordNetLemmatizer : NLTK에서 지원하는 표제어 추출 도구 
from nltk.stem import WordNetLemmatizer
nltk.download('omw-1.4')
```

```
[nltk_data] Downloading package omw-1.4 to /root/nltk_data...
True
```

```python
lemmatizer = WordNetLemmatizer()

words = ['sky', 'computer', 'having', 'lives', 'love', 'mouse', 'dies', 'listened', 'ate', 'has']
print("추출 전: ", words)
print('추출 후: ', [lemmatizer.lemmatize(word) for word in words])
```

```
추출 전:  ['sky', 'computer', 'having', 'lives', 'love', 'mouse', 'dies', 'listened', 'ate', 'has']
추출 후:  ['sky', 'computer', 'having', 'life', 'love', 'mouse', 'dy', 'listened', 'ate', 'ha']
```

```python
lemmatizer.lemmatize('dies', 'v')
```

```
die
```

```python
lemmatizer.lemmatize('listened', 'v')
```

```
listen
```

```python
lemmatizer.lemmatize('better', 'a')

# v : 동사 / a : 형용사 / n : 명사 / r : 부사 
```

```
good
```

# 어간 추출(Stemming)

```python
import nltk
nltk.download('punkt')
```

```python
from nltk.stem import PorterStemmer
from nltk.tokenize import word_tokenize
```

```python
sentence = """The trader, becoming uneasy at this exciting scene, and fearing the
rest of the drove would become dissatisfied with their situations,
permitted sister to leave the yard for a few moments, to keep
mother’s company. He did not watch her, as I thought he would
have done, but permitted her to go about with mother, and even to
accompany us part of our way towards home. He ordered dinner for us,
but not one of us could eat one mouthful. I thought my heart would
break, as the time drew near for our departure. I dreaded the time
when I should bid farewell to my beloved sister, never more to see
her face, never more to meet her in the paternal circle, never more
to hear her fervent prayer to the throne of God.
"""
```

```python
stemmer = PorterStemmer()

words2 = word_tokenize(sentence)
print(words2) 
print([stemmer.stem(w) for w in words2])
```

```
['The', 'trader', ',', 'becoming', 'uneasy', 'at', 'this', 'exciting', 'scene', ',', 'and', 'fearing', 'the', 'rest', 'of', 'the', 'drove', 'would', 'become', 'dissatisfied', 'with', 'their', 'situations', ',', 'permitted', 'sister', 'to', 'leave', 'the', 'yard', 'for', 'a', 'few', 'moments', ',', 'to', 'keep', 'mother', '’', 's', 'company', '.', 'He', 'did', 'not', 'watch', 'her', ',', 'as', 'I', 'thought', 'he', 'would', 'have', 'done', ',', 'but', 'permitted', 'her', 'to', 'go', 'about', 'with', 'mother', ',', 'and', 'even', 'to', 'accompany', 'us', 'part', 'of', 'our', 'way', 'towards', 'home', '.', 'He', 'ordered', 'dinner', 'for', 'us', ',', 'but', 'not', 'one', 'of', 'us', 'could', 'eat', 'one', 'mouthful', '.', 'I', 'thought', 'my', 'heart', 'would', 'break', ',', 'as', 'the', 'time', 'drew', 'near', 'for', 'our', 'departure', '.', 'I', 'dreaded', 'the', 'time', 'when', 'I', 'should', 'bid', 'farewell', 'to', 'my', 'beloved', 'sister', ',', 'never', 'more', 'to', 'see', 'her', 'face', ',', 'never', 'more', 'to', 'meet', 'her', 'in', 'the', 'paternal', 'circle', ',', 'never', 'more', 'to', 'hear', 'her', 'fervent', 'prayer', 'to', 'the', 'throne', 'of', 'God', '.']
['the', 'trader', ',', 'becom', 'uneasi', 'at', 'thi', 'excit', 'scene', ',', 'and', 'fear', 'the', 'rest', 'of', 'the', 'drove', 'would', 'becom', 'dissatisfi', 'with', 'their', 'situat', ',', 'permit', 'sister', 'to', 'leav', 'the', 'yard', 'for', 'a', 'few', 'moment', ',', 'to', 'keep', 'mother', '’', 's', 'compani', '.', 'he', 'did', 'not', 'watch', 'her', ',', 'as', 'i', 'thought', 'he', 'would', 'have', 'done', ',', 'but', 'permit', 'her', 'to', 'go', 'about', 'with', 'mother', ',', 'and', 'even', 'to', 'accompani', 'us', 'part', 'of', 'our', 'way', 'toward', 'home', '.', 'he', 'order', 'dinner', 'for', 'us', ',', 'but', 'not', 'one', 'of', 'us', 'could', 'eat', 'one', 'mouth', '.', 'i', 'thought', 'my', 'heart', 'would', 'break', ',', 'as', 'the', 'time', 'drew', 'near', 'for', 'our', 'departur', '.', 'i', 'dread', 'the', 'time', 'when', 'i', 'should', 'bid', 'farewel', 'to', 'my', 'belov', 'sister', ',', 'never', 'more', 'to', 'see', 'her', 'face', ',', 'never', 'more', 'to', 'meet', 'her', 'in', 'the', 'patern', 'circl', ',', 'never', 'more', 'to', 'hear', 'her', 'fervent', 'prayer', 'to', 'the', 'throne', 'of', 'god', '.']
```

# PorterStemmer : 알고리즘 

규칙 기반으로 접근 => 어림짐작하는 작업 => 섬세한 작업 X => 사전에 없는 단어가 도출될수도 ...! 

- 마틴포터 홈페이지에서 다양한 것들을 살펴볼 수 있다
- 규칙 기반의 접근 
  - ALIZE => AL
  - ANCE => 삭제
  - ICAL => IC

```python
word = ['channelize', 'allowance', 'typical']

print('추출 전 : ', word)
print('추출 후 : ', [stemmer.stem(w) for w in word])
```

```
추출 전 :  ['channelize', 'allowance', 'typical']
추출 후 :  ['channel', 'allow', 'typic']
```

```python
# NLTK에서는 포터 알고리즘 이외에도 랭커스터 스태머(Lancaster Stemmer) 알고리즘을 지원 
from nltk.stem import LancasterStemmer
```

```python
lancaster = LancasterStemmer()
```

```python
print([stemmer.stem(w) for w in words])
print()
print([lancaster.stem(w) for w in words])

# 두 스태머는 다른 결과를 보여줌 
# 두 스태머는 서로 다른 알고리즘을 사용하기 때문!
# 제대로 된 표제어를 뽑아오지 못하고 있음 ...!
```

```
['sky', 'comput', 'have', 'live', 'love', 'mous', 'die', 'listen', 'ate', 'ha']

['sky', 'comput', 'hav', 'liv', 'lov', 'mous', 'die', 'list', 'at', 'has']
```

# 불용어 (Stopword)

단어들 중에서 의미가 없는 단어 

데이터 중에서 의미가 있는 단어 토큰만 취급하기 위해서 의미를 가지지 않는 단어들을 제거하는 작업 

```python
import nltk
nltk.download('stopwords')
nltk.download('punkt')
```

```python
from nltk.corpus import stopwords
```

```python
# NLTK에 있는 불용어
s = stopwords.words('english')
print(len(s))
print(s[:20])
```

```
179
['i', 'me', 'my', 'myself', 'we', 'our', 'ours', 'ourselves', 'you', "you're", "you've", "you'll", "you'd", 'your', 'yours', 'yourself', 'yourselves', 'he', 'him', 'his']
```

```python
sentence = """The provision for each slave, per week, was a peck of corn, two
dozens of herrings, and about four pounds of meat. The children,
under eight years of age, were not allowed anything. The women were
allowed four weeks of leisure at child birth; after which, they were
compelled to leave their infants to provide for themselves, and to
the mercy of Providence, while they were again forced to labor in the
field, sometimes a mile from the house."""

# NLTK에서 지정한 불용어를 가져오기 
sw = set(stopwords.words('english'))
# print(sw)

# 문장을 단어로 쪼개는 작업
word = word_tokenize(sentence)
# print(word)

# 불용어가 아닌 단어들만 list에 담아서 출력 
result =[]
for w in word:
  if w not in sw:
    result.append(w)
print(word)
print(result)
```

```
['The', 'provision', 'for', 'each', 'slave', ',', 'per', 'week', ',', 'was', 'a', 'peck', 'of', 'corn', ',', 'two', 'dozens', 'of', 'herrings', ',', 'and', 'about', 'four', 'pounds', 'of', 'meat', '.', 'The', 'children', ',', 'under', 'eight', 'years', 'of', 'age', ',', 'were', 'not', 'allowed', 'anything', '.', 'The', 'women', 'were', 'allowed', 'four', 'weeks', 'of', 'leisure', 'at', 'child', 'birth', ';', 'after', 'which', ',', 'they', 'were', 'compelled', 'to', 'leave', 'their', 'infants', 'to', 'provide', 'for', 'themselves', ',', 'and', 'to', 'the', 'mercy', 'of', 'Providence', ',', 'while', 'they', 'were', 'again', 'forced', 'to', 'labor', 'in', 'the', 'field', ',', 'sometimes', 'a', 'mile', 'from', 'the', 'house', '.']
['The', 'provision', 'slave', ',', 'per', 'week', ',', 'peck', 'corn', ',', 'two', 'dozens', 'herrings', ',', 'four', 'pounds', 'meat', '.', 'The', 'children', ',', 'eight', 'years', 'age', ',', 'allowed', 'anything', '.', 'The', 'women', 'allowed', 'four', 'weeks', 'leisure', 'child', 'birth', ';', ',', 'compelled', 'leave', 'infants', 'provide', ',', 'mercy', 'Providence', ',', 'forced', 'labor', 'field', ',', 'sometimes', 'mile', 'house', '.']
```

# 한국어 불용어 제거하기 

\- 토큰화 => 조사 or 접속사 같이 명사 or 형용사에서 필요없는 단어들을 제거 

\- 한국어의 경우에는 사용자가 직접 불용어를 지정해서 사용하는 경우가 많음 

```python
!pip install Konlpy
```

```python
from konlpy.tag import Okt
```

```python
okt = Okt()

ex = "안녕하세요 반갑습니다. 여러분의 점심은 어떤 메뉴였나요?"
sw = "의 은 어떤 ? . "

sw = set(sw.split(' '))
token = okt.morphs(ex) # 형태소 분석

result = [w for w in token if w not in sw]

print(token) # 불용어 제거 전
print()
print(result) # 불용어 제거 후
```

```
['안녕하세요', '반갑습니다', '.', '여러분', '의', '점심', '은', '어떤', '메뉴', '였나요', '?']

['안녕하세요', '반갑습니다', '여러분', '점심', '메뉴', '였나요']
```

# 정수 인코딩(Integer Encoding)

컴퓨터의 입장에서는 텍스트보다 숫자를 더 쉽게 처리하려는 경향이 있음 

텍스트에 정수를 부여하는 방법 

1. 단어를 빈도수를 기준으로 정렬 
2. 정렬된 집합을 구성 
3. 빈도가 높은 순 => 낮은 순으로 숫자를 부여 

```python
# 영어 동요
text = """Twinkle, twinkle little star
How I wonder what you are
Up above the world so high
Like a diamond in the sky
Twinkle, twinkle little star
How I wonder what you are
The wheels on the bus go round and round
Round and round, round and round
The wheels on the bus go round and round
All through the town.
The wipers on the bus go "Swish, swish, swish,
Swish, swish, swish, swish, swish, swish"
The wipers on the bus go "Swish, swish, swish"
All through the town.
The door on the bus goes open and shut
Open and shut, open and shut
The door on the bus goes open and shut
All through the town.
The horn on the bus goes "Beep, beep, beep
Beep, beep, beep, bep, beep, beep"
The horn on the bus goes "Beep, beep, beep"
All through the town.
Old MacDonald had a farm, Ee-i-ee-i-o
And on his farm he had a dog, Ee-i-ee-i-o,
With a bow-wow here, And a bow-wow there,
Here a bow, there a wow, Everywhere a bow-wow,
Old MacDonald had a farm, Ee-i-ee-i-o,
Old MacDonald had a farm, Ee-i-ee-i-o,
And on his farm he had a cow, Ee-i-ee-i-o,
With a moo-moo here, And a moo-moo there,
Here a moo, there a moo, Everywhere a moo-moo,
Old MacDonald had a farm, Ee-i-ee-i-o.
Old MacDonald had a farm, Ee-i-ee-i-o,
And on his farm he had a pig, Ee-i-ee-i-o,
With an oink-oink here, And an oink-oink there,
Here an oink, there an oink ,Everywhere an oink-oink,
Old MacDonald had a farm, Ee-i-ee-i-o.
Old MacDonald had a farm, Ee-i-ee-i-o,
And on his farm he had a sheep, Ee-i-ee-i-o,
With a baa-baa here, And a baa-baa there,
Here a baa, there a baa , Everywhere a baa-baa,
Old MacDonald had a farm, Ee-i-ee-i-o."""
```

```python
from nltk.tokenize import sent_tokenize # 영어 문장 토큰화
from nltk.tokenize import word_tokenize # 영어 단어 토큰화
from nltk.corpus import stopwords
```

```python
# 문장 토큰화 
sentence = sent_tokenize(text)
sentence
```

```
['Twinkle, twinkle little star\nHow I wonder what you are\nUp above the world so high\nLike a diamond in the sky\nTwinkle, twinkle little star\nHow I wonder what you are\nThe wheels on the bus go round and round\nRound and round, round and round\nThe wheels on the bus go round and round\nAll through the town.',
 'The wipers on the bus go "Swish, swish, swish,\nSwish, swish, swish, swish, swish, swish"\nThe wipers on the bus go "Swish, swish, swish"\nAll through the town.',
 'The door on the bus goes open and shut\nOpen and shut, open and shut\nThe door on the bus goes open and shut\nAll through the town.',
 'The horn on the bus goes "Beep, beep, beep\nBeep, beep, beep, bep, beep, beep"\nThe horn on the bus goes "Beep, beep, beep"\nAll through the town.',
 'Old MacDonald had a farm, Ee-i-ee-i-o\nAnd on his farm he had a dog, Ee-i-ee-i-o,\nWith a bow-wow here, And a bow-wow there,\nHere a bow, there a wow, Everywhere a bow-wow,\nOld MacDonald had a farm, Ee-i-ee-i-o,\nOld MacDonald had a farm, Ee-i-ee-i-o,\nAnd on his farm he had a cow, Ee-i-ee-i-o,\nWith a moo-moo here, And a moo-moo there,\nHere a moo, there a moo, Everywhere a moo-moo,\nOld MacDonald had a farm, Ee-i-ee-i-o.',
 'Old MacDonald had a farm, Ee-i-ee-i-o,\nAnd on his farm he had a pig, Ee-i-ee-i-o,\nWith an oink-oink here, And an oink-oink there,\nHere an oink, there an oink ,Everywhere an oink-oink,\nOld MacDonald had a farm, Ee-i-ee-i-o.',
 'Old MacDonald had a farm, Ee-i-ee-i-o,\nAnd on his farm he had a sheep, Ee-i-ee-i-o,\nWith a baa-baa here, And a baa-baa there,\nHere a baa, there a baa , Everywhere a baa-baa,\nOld MacDonald had a farm, Ee-i-ee-i-o.']
```

```python
# 단어 토큰화 => 불용어를 뺀 단어 토큰들을 list에 담기 
    
# 단어 등장횟수를 카운팅 // ex) and : 3, on : 2, ...
#     => 단어.lower() # 모든 단어를 소문자화 => 단어 개수를 줄이는데 도움 O
#     => 2글자 이상인 글자 PASS!

sw = set(stopwords.words('english'))
final_sentence = []
aa = {}

for s in sentence:
  # 단어 토큰화 
  word = word_tokenize(s)
  result = []
  for w in word:
    w = w.lower()
    if w not in sw:
      if len(w) > 2: 
        result.append(w)
        if w not in aa: 
          aa[w] = 0
        aa[w] += 1
  final_sentence.append(result)
print(final_sentence)
print(aa)
```

```
[['twinkle', 'twinkle', 'little', 'star', 'wonder', 'world', 'high', 'like', 'diamond', 'sky', 'twinkle', 'twinkle', 'little', 'star', 'wonder', 'wheels', 'bus', 'round', 'round', 'round', 'round', 'round', 'round', 'wheels', 'bus', 'round', 'round', 'town'], ['wipers', 'bus', 'swish', 'swish', 'swish', 'swish', 'swish', 'swish', 'swish', 'swish', 'swish', 'wipers', 'bus', 'swish', 'swish', 'swish', 'town'], ['door', 'bus', 'goes', 'open', 'shut', 'open', 'shut', 'open', 'shut', 'door', 'bus', 'goes', 'open', 'shut', 'town'], ['horn', 'bus', 'goes', 'beep', 'beep', 'beep', 'beep', 'beep', 'beep', 'bep', 'beep', 'beep', 'horn', 'bus', 'goes', 'beep', 'beep', 'beep', 'town'], ['old', 'macdonald', 'farm', 'ee-i-ee-i-o', 'farm', 'dog', 'ee-i-ee-i-o', 'bow-wow', 'bow-wow', 'bow', 'wow', 'everywhere', 'bow-wow', 'old', 'macdonald', 'farm', 'ee-i-ee-i-o', 'old', 'macdonald', 'farm', 'ee-i-ee-i-o', 'farm', 'cow', 'ee-i-ee-i-o', 'moo-moo', 'moo-moo', 'moo', 'moo', 'everywhere', 'moo-moo', 'old', 'macdonald', 'farm', 'ee-i-ee-i-o'], ['old', 'macdonald', 'farm', 'ee-i-ee-i-o', 'farm', 'pig', 'ee-i-ee-i-o', 'oink-oink', 'oink-oink', 'oink', 'oink', 'everywhere', 'oink-oink', 'old', 'macdonald', 'farm', 'ee-i-ee-i-o'], ['old', 'macdonald', 'farm', 'ee-i-ee-i-o', 'farm', 'sheep', 'ee-i-ee-i-o', 'baa-baa', 'baa-baa', 'baa', 'baa', 'everywhere', 'baa-baa', 'old', 'macdonald', 'farm', 'ee-i-ee-i-o']]
{'twinkle': 4, 'little': 2, 'star': 2, 'wonder': 2, 'world': 1, 'high': 1, 'like': 1, 'diamond': 1, 'sky': 1, 'wheels': 2, 'bus': 8, 'round': 8, 'town': 4, 'wipers': 2, 'swish': 12, 'door': 2, 'goes': 4, 'open': 4, 'shut': 4, 'horn': 2, 'beep': 11, 'bep': 1, 'old': 8, 'macdonald': 8, 'farm': 12, 'ee-i-ee-i-o': 12, 'dog': 1, 'bow-wow': 3, 'bow': 1, 'wow': 1, 'everywhere': 4, 'cow': 1, 'moo-moo': 3, 'moo': 2, 'pig': 1, 'oink-oink': 3, 'oink': 2, 'sheep': 1, 'baa-baa': 3, 'baa': 2}
```

```python
# 'star' 단어의 빈도수
print(aa['star'])
```

```
2
```

```python
# sorted() 함수 : 빈도수대로 정렬 
# sorted(정렬할데이터, key옵션, reverse옵션)
#   key옵션 : key Parameter
#             어떤 것을 기준으로 정렬할지 (key에 준 값으로 정렬)
#   reverse옵션 : False(default) >> 오름차순 

# sort() vs sorted() : 
#   sort()는 리스트 자체를 정렬해서 바꾸는 형태
#   sorted()는 원래 리스트는 그대로 두고, 정렬한 것을 새로운 리스트에 넣는 형태

# key=lambda x: x[1] => x[1]의 값이 정렬의 기준 => 빈도수를 기준으로 정렬 

aaSort = sorted(aa.items(), key=lambda x: x[1], reverse=True)
print(aaSort)
```

```
[('swish', 12), ('farm', 12), ('ee-i-ee-i-o', 12), ('beep', 11), ('bus', 8), ('round', 8), ('old', 8), ('macdonald', 8), ('twinkle', 4), ('town', 4), ('goes', 4), ('open', 4), ('shut', 4), ('everywhere', 4), ('bow-wow', 3), ('moo-moo', 3), ('oink-oink', 3), ('baa-baa', 3), ('little', 2), ('star', 2), ('wonder', 2), ('wheels', 2), ('wipers', 2), ('door', 2), ('horn', 2), ('moo', 2), ('oink', 2), ('baa', 2), ('world', 1), ('high', 1), ('like', 1), ('diamond', 1), ('sky', 1), ('bep', 1), ('dog', 1), ('bow', 1), ('wow', 1), ('cow', 1), ('pig', 1), ('sheep', 1)]
```

```python
# [높은 빈도수]를 가지고 있는 단어일수록 [낮은 정수값]을 부여(정수는 1부터 부여)
# 빈도수가 1 이하인것들은 삭제(결과에 안나오게)
# {'twinkle' : 1, 'little' : 2, 'put' : 3, ...}
aa_index = {}
i = 0

for(word, frequency) in aaSort:
  if frequency > 1:
    i = i + 1
    aa_index[word] = i
print(aa_index)
```

```
{'little': 1, 'star': 2, 'wonder': 3, 'wheels': 4, 'wipers': 5, 'door': 6, 'horn': 7, 'moo': 8, 'oink': 9, 'baa': 10, 'bow-wow': 11, 'moo-moo': 12, 'oink-oink': 13, 'baa-baa': 14, 'twinkle': 15, 'town': 16, 'goes': 17, 'open': 18, 'shut': 19, 'everywhere': 20, 'bus': 21, 'round': 22, 'old': 23, 'macdonald': 24, 'beep': 25, 'swish': 26, 'farm': 27, 'ee-i-ee-i-o': 28}
```

```python
# 단어 빈도수가 가장 높은 상위 5개 출력 
freSize = 5

# 인덱스가 5초과(6이상)인 단어들을 aa_final이라는 변수에 담기 
aa_final = [w for (w, index) in aa_index.items() if index >= freSize + 1]

for w in aa_final:
  del aa_index[w]

print(aa_index)
```

```
{'little': 1, 'star': 2, 'wonder': 3, 'wheels': 4, 'wipers': 5}
```

```python
# ['little', 'star', 'wonder', 'wheels', 'wipers'] >> aa_index에 더 이상 존재하지 않는 단어 추가??
# [1,2,3,4,5, ?]
# Out-Of-Vocabulary : 단어 집합에 없는 단어 >> OOV 
# aa_index에 'OOV'라는 단어가 있는 자리를 하나 만들고, 그 단어집합에 존재하지 않는 단어를 
#   OOV의 인덱스로 인코딩 
```

```python
aa_index['OOV'] = len(aa_index) 
print(aa_index)
```

```
{'little': 1, 'star': 2, 'wonder': 3, 'wheels': 4, 'wipers': 5, 'OOV': 6}
```

```python
# 문장마다 텍스트 대신 그 자리에 해당하는 인덱스로 변환 
# 문장마다 단어로 토큰화 : final_sentence 

encoding_sentences = []
for fs in final_sentence:
  encoding_sentence = []
  for w in fs:
    try: 
      # 단어집합에 있는 단어면 해당 단어의 정수를 넣어줌
      encoding_sentence.append(aa_index[w])
    except KeyError:
      # 단어 집합에 없는 단어면 OOV의 정수를 넣어줌
      encoding_sentence.append(aa_index['OOV'])
  encoding_sentences.append(encoding_sentence)
print(encoding_sentences)
```

```
[[6, 6, 1, 2, 3, 6, 6, 6, 6, 6, 6, 6, 1, 2, 3, 4, 6, 6, 6, 6, 6, 6, 6, 4, 6, 6, 6, 6], [5, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 5, 6, 6, 6, 6, 6], [6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6], [6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6], [6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6], [6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6], [6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6, 6]]
```

# 케라스 (Keras)

text preprocessing(텍스트 전처리)를 위한 다양한 도구를 제공

```python
from tensorflow.keras.preprocessing.text import Tokenizer
```

```python
final_sentence = [['twinkle', 'twinkle', 'little', 'star', 'wonder', 'world', 'high', 'like', 'diamond', 'sky', 'twinkle', 'twinkle', 'little', 'star', 'wonder', 'wheels', 'bus', 'round', 'round', 'round', 'round', 'round', 'round', 'wheels', 'bus', 'round', 'round', 'town'], ['wipers', 'bus', 'swish', 'swish', 'swish', 'swish', 'swish', 'swish', 'swish', 'swish', 'swish', 'wipers', 'bus', 'swish', 'swish', 'swish', 'town'], ['door', 'bus', 'goes', 'open', 'shut', 'open', 'shut', 'open', 'shut', 'door', 'bus', 'goes', 'open', 'shut', 'town'], ['horn', 'bus', 'goes', 'beep', 'beep', 'beep', 'beep', 'beep', 'beep', 'bep', 'beep', 'beep', 'horn', 'bus', 'goes', 'beep', 'beep', 'beep', 'town'], ['old', 'macdonald', 'farm', 'ee-i-ee-i-o', 'farm', 'dog', 'ee-i-ee-i-o', 'bow-wow', 'bow-wow', 'bow', 'wow', 'everywhere', 'bow-wow', 'old', 'macdonald', 'farm', 'ee-i-ee-i-o', 'old', 'macdonald', 'farm', 'ee-i-ee-i-o', 'farm', 'cow', 'ee-i-ee-i-o', 'moo-moo', 'moo-moo', 'moo', 'moo', 'everywhere', 'moo-moo', 'old', 'macdonald', 'farm', 'ee-i-ee-i-o'], ['old', 'macdonald', 'farm', 'ee-i-ee-i-o', 'farm', 'pig', 'ee-i-ee-i-o', 'oink-oink', 'oink-oink', 'oink', 'oink', 'everywhere', 'oink-oink', 'old', 'macdonald', 'farm', 'ee-i-ee-i-o'], ['old', 'macdonald', 'farm', 'ee-i-ee-i-o', 'farm', 'sheep', 'ee-i-ee-i-o', 'baa-baa', 'baa-baa', 'baa', 'baa', 'everywhere', 'baa-baa', 'old', 'macdonald', 'farm', 'ee-i-ee-i-o']]
print(final_sentence)
```

```python
tokenizer = Tokenizer()

# fit_on_texts(코퍼스) >> 빈도수를 기준으로 단어 집합을 만들어 줌 
# 높은 빈도수의 단어부터 낮은 숫자를 부여 >> 정수 인코딩 
tokenizer.fit_on_texts(final_sentence)
```

```python
# word_index : 각각 어떤 인덱스가 부여되었는지 확인 
print(tokenizer.word_index)
```

```
{'swish': 1, 'farm': 2, 'ee-i-ee-i-o': 3, 'beep': 4, 'bus': 5, 'round': 6, 'old': 7, 'macdonald': 8, 'twinkle': 9, 'town': 10, 'goes': 11, 'open': 12, 'shut': 13, 'everywhere': 14, 'bow-wow': 15, 'moo-moo': 16, 'oink-oink': 17, 'baa-baa': 18, 'little': 19, 'star': 20, 'wonder': 21, 'wheels': 22, 'wipers': 23, 'door': 24, 'horn': 25, 'moo': 26, 'oink': 27, 'baa': 28, 'world': 29, 'high': 30, 'like': 31, 'diamond': 32, 'sky': 33, 'bep': 34, 'dog': 35, 'bow': 36, 'wow': 37, 'cow': 38, 'pig': 39, 'sheep': 40}
```

```python
# word_counts : 단어 빈도수가 궁금하다면 ...
print(tokenizer.word_counts)
```

```
OrderedDict([('twinkle', 4), ('little', 2), ('star', 2), ('wonder', 2), ('world', 1), ('high', 1), ('like', 1), ('diamond', 1), ('sky', 1), ('wheels', 2), ('bus', 8), ('round', 8), ('town', 4), ('wipers', 2), ('swish', 12), ('door', 2), ('goes', 4), ('open', 4), ('shut', 4), ('horn', 2), ('beep', 11), ('bep', 1), ('old', 8), ('macdonald', 8), ('farm', 12), ('ee-i-ee-i-o', 12), ('dog', 1), ('bow-wow', 3), ('bow', 1), ('wow', 1), ('everywhere', 4), ('cow', 1), ('moo-moo', 3), ('moo', 2), ('pig', 1), ('oink-oink', 3), ('oink', 2), ('sheep', 1), ('baa-baa', 3), ('baa', 2)])
```

```python
# texts_to_sequences(코퍼스) : text를 부여받은 인덱스로 반환하기 
print(tokenizer.texts_to_sequences(final_sentence))
```

```
[[9, 9, 19, 20, 21, 29, 30, 31, 32, 33, 9, 9, 19, 20, 21, 22, 5, 6, 6, 6, 6, 6, 6, 22, 5, 6, 6, 10], [23, 5, 1, 1, 1, 1, 1, 1, 1, 1, 1, 23, 5, 1, 1, 1, 10], [24, 5, 11, 12, 13, 12, 13, 12, 13, 24, 5, 11, 12, 13, 10], [25, 5, 11, 4, 4, 4, 4, 4, 4, 34, 4, 4, 25, 5, 11, 4, 4, 4, 10], [7, 8, 2, 3, 2, 35, 3, 15, 15, 36, 37, 14, 15, 7, 8, 2, 3, 7, 8, 2, 3, 2, 38, 3, 16, 16, 26, 26, 14, 16, 7, 8, 2, 3], [7, 8, 2, 3, 2, 39, 3, 17, 17, 27, 27, 14, 17, 7, 8, 2, 3], [7, 8, 2, 3, 2, 40, 3, 18, 18, 28, 28, 14, 18, 7, 8, 2, 3]]
```

```python
# 빈도수가 높은 상위 5개만 사용하겠다 ~ 라고 지정하는 방법 
# tokenizer = Tokenizer(num_words=숫자)
word_size = 5
tokenizer = Tokenizer(num_words= word_size + 1) # num_words는 count를 0부터 세서 + 1
tokenizer.fit_on_texts(final_sentence)
```

```python
# word_index : 각각 어떤 인덱스가 부여되었는지 확인 
print(tokenizer.word_index)
```

```
{'swish': 1, 'farm': 2, 'ee-i-ee-i-o': 3, 'beep': 4, 'bus': 5, 'round': 6, 'old': 7, 'macdonald': 8, 'twinkle': 9, 'town': 10, 'goes': 11, 'open': 12, 'shut': 13, 'everywhere': 14, 'bow-wow': 15, 'moo-moo': 16, 'oink-oink': 17, 'baa-baa': 18, 'little': 19, 'star': 20, 'wonder': 21, 'wheels': 22, 'wipers': 23, 'door': 24, 'horn': 25, 'moo': 26, 'oink': 27, 'baa': 28, 'world': 29, 'high': 30, 'like': 31, 'diamond': 32, 'sky': 33, 'bep': 34, 'dog': 35, 'bow': 36, 'wow': 37, 'cow': 38, 'pig': 39, 'sheep': 40}
```

```python
# word_counts : 단어 빈도수가 궁금하다면 ...
print(tokenizer.word_counts)
```

```
OrderedDict([('twinkle', 4), ('little', 2), ('star', 2), ('wonder', 2), ('world', 1), ('high', 1), ('like', 1), ('diamond', 1), ('sky', 1), ('wheels', 2), ('bus', 8), ('round', 8), ('town', 4), ('wipers', 2), ('swish', 12), ('door', 2), ('goes', 4), ('open', 4), ('shut', 4), ('horn', 2), ('beep', 11), ('bep', 1), ('old', 8), ('macdonald', 8), ('farm', 12), ('ee-i-ee-i-o', 12), ('dog', 1), ('bow-wow', 3), ('bow', 1), ('wow', 1), ('everywhere', 4), ('cow', 1), ('moo-moo', 3), ('moo', 2), ('pig', 1), ('oink-oink', 3), ('oink', 2), ('sheep', 1), ('baa-baa', 3), ('baa', 2)])
```

```python
# texts_to_sequences(코퍼스) : text를 부여받은 인덱스로 반환하기 
print(tokenizer.texts_to_sequences(final_sentence))
```

```
[[5, 5], [5, 1, 1, 1, 1, 1, 1, 1, 1, 1, 5, 1, 1, 1], [5, 5], [5, 4, 4, 4, 4, 4, 4, 4, 4, 5, 4, 4, 4], [2, 3, 2, 3, 2, 3, 2, 3, 2, 3, 2, 3], [2, 3, 2, 3, 2, 3], [2, 3, 2, 3, 2, 3]]
```

```python
tokenizer = Tokenizer()
tokenizer.fit_on_texts(final_sentence)
```

```python
freSize = 5
wf = [w for w, i in tokenizer.word_index.items() if i >= freSize + 1]
for w in wf:
  del tokenizer.word_index[w]
  del tokenizer.word_counts[w]

print(tokenizer.word_index)
print(tokenizer.word_counts)
print(tokenizer.texts_to_sequences(final_sentence))
```

```
{'swish': 1, 'farm': 2, 'ee-i-ee-i-o': 3, 'beep': 4, 'bus': 5}
OrderedDict([('bus', 8), ('swish', 12), ('beep', 11), ('farm', 12), ('ee-i-ee-i-o', 12)])
[[5, 5], [5, 1, 1, 1, 1, 1, 1, 1, 1, 1, 5, 1, 1, 1], [5, 5], [5, 4, 4, 4, 4, 4, 4, 4, 4, 5, 4, 4, 4], [2, 3, 2, 3, 2, 3, 2, 3, 2, 3, 2, 3], [2, 3, 2, 3, 2, 3], [2, 3, 2, 3, 2, 3]]
```

```python
# keras의 tokenizer에 (기본 설정이) 단어 집합에 없는 단어의 경우 
# OOV(Out-Of-Vocabulary)에 대해서는 단어 자체를 제거하려는 특성 

# OOV를 따로 설정해서 가지고 있고 싶다면 'oov_token'이라는 것을 사용 
freSize = 5
tokenizer = Tokenizer(num_words=freSize + 2, oov_token='OOV')
tokenizer.fit_on_texts(final_sentence)
```

```python
tokenizer.word_index['OOV']
```

```
1
```

```python
print(tokenizer.texts_to_sequences(final_sentence))
# oov_token을 사용하면 OOV의 인덱스가 1로 설정 
# 나머지 빈도수 상위 5개(단어 집합) 자동적으로 인덱스가 +1씩 추가됨 
```

```
[[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 6, 1, 1, 1, 1, 1, 1, 1, 6, 1, 1, 1], [1, 6, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 6, 2, 2, 2, 1], [1, 6, 1, 1, 1, 1, 1, 1, 1, 1, 6, 1, 1, 1, 1], [1, 6, 1, 5, 5, 5, 5, 5, 5, 1, 5, 5, 1, 6, 1, 5, 5, 5, 1], [1, 1, 3, 4, 3, 1, 4, 1, 1, 1, 1, 1, 1, 1, 1, 3, 4, 1, 1, 3, 4, 3, 1, 4, 1, 1, 1, 1, 1, 1, 1, 1, 3, 4], [1, 1, 3, 4, 3, 1, 4, 1, 1, 1, 1, 1, 1, 1, 1, 3, 4], [1, 1, 3, 4, 3, 1, 4, 1, 1, 1, 1, 1, 1, 1, 1, 3, 4]]

```









