---
layout: single
title: "Node.js - Nunjucks"
categories: nodejs
tag: [vscode, javascript]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# index.js

```js
// 템플릿이란 ?
// 모든 웹사이트의 포괄적인 '레이아웃'에 대한 구조를 보여주는
// 미리 디자인된 자원
// 화면 이동시에 기본 틀은 한번만 만들어놓고 계속해서 사용하는 것이 가능 !
// Node.js의 대표적인 템플릿 엔진에는 pug와 nunjucks가 있음
// 성능면에서는 pug가 렌더링 속도가 nunjucks보다 빠르다.
// pug는 따로 html태그를 pug전용 문법으로 변환해야 하지만,
// nunjucks는 html 문법을 그대로 사용하는 것이 가능 => 호환성이 아주 좋다~!

const express = require('express');
const nunjucks = require('nunjucks');

const app = express();

app.set('view engine', 'html');

const testRouter = require('./routes/route');
app.use('/test', testRouter);

nunjucks.configure('views', {
  // 'views' 폴더가 Nunjucks 파일의 위치가 됨
  express: app, // express속성에 app 객체를 연결
  watch: true, // true로 지정하면, HTML 파일이 변경될 때 템플릿 엔진을 reload 함
});

app.get('/', (req, res) => {
  res.send('START !');
});

app.get('/include', (req, res) => {
  res.render('main');
});

app.get('/extend', (req, res) => {
  res.render('body');
});

app.listen(3000, () => {
  console.log('3000번 포트에서 동작 중 !');
});

```

# routes/route.js

```js
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  res.render('index', { title: 'Nunjucks', content: '테스트 중 !' });
  // index.html 에 title, content라는 변수를 전달
});

module.exports = router;

```

# views/index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <title>Page Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <!-- <link rel='stylesheet' type='text/css' media='screen' href='main.css'>
  <script src='main.js'></script> -->
  </head>
  <body>
    <h1>{{title}}</h1>
    <input placeholder="{{content}}" />
    <!--
      1. 변수
      변수를 사용할 때 : 중괄호를 이중으로 사용함 {{ 변수 }} 의 형태
      
      내부에서 변수를 선언할 때에는
      {% set 변수명 = 값 %} 의 형태로 사용함
      { {와 %는 붙여서 쓰기 !! => 문법 인식이 안됨! }
      없는 변수값을 출력하면 EL처럼 에러발생 X !
    -->
    {% set node = 'Node.js' %} {% set js = 'Javascript' %}
    <h1>{{node}} 와 {{js}}</h1>
    <!-- 
      역시나 중괄호 안쪽에서 계산도 가능 !
    -->
    {{ 2**7 }}

    <!--
      이스케이프 : &lt; &gt; ..등을 일컫음
      {{ data | safe }} safe라는 필터를 사용하지 않으면 
      태그를 인식하지 못하고 그냥 문자열로 받아들인다. 
    -->
    <p>{{'<b>이스케이프</b>'}}</p>
    <p>{{'<b>이스케이프</b>' | safe}}</p>

    <!--
      JS에서 Nunjucks 변수 사용하기
    -->
    <!-- <script>
      let node = '{{node}}';
      alert(node);
    </script> -->
    <!--
      2. 반복문
      { 와 % 둘이 주석 안에서 붙어있으면 에러나는 경우가 있음 
      { % % } 안에 for ~ in 작성하고
      반복문이 끝나는 부분에는 { % endfor % }를 입력해서
      반복문이 끝남을 알려줘야 함 
      반복문 내에서 index를 사용하고 싶다면
      loop.index라는 특수한 변수를 사용할 수 있음 ! 
    -->
    <ul>
      {% set fruits = ['사과', '배', '바나나', '복숭아', '레몬'] %} {% for fruit
      in fruits %}
      <li>{{ loop.index}} 번째 : {{ fruit }}</li>
      {% endfor %}
    </ul>
    <!--
      3. 조건문
      { % if 변수 % } { % elif % } { % else % } { % endif % }로 구성 
    -->
    {% set isLoggedIn = true %} {% if isLoggedIn %}
    <div>로그인 되었습니다.</div>
    {% else %}
    <div>로그인 하세요.</div>
    {% endif %}
    <!--
      { { } } 안에서 사용하는 방법 
    -->
    <div>{{ '로그인 되었습니다.' if isLoggedIn else '로그인 하세요.' }}</div>

    <!--
      4. include
      웹 제작시 공통되는 부분들을 따로 관리할 수 있다는 장점 ! 
      페이지마다 동일한 HTML 코드를 넣을 필요가 없음 
      (header, footer, main.html)
    -->

    <!--
      5. extend와 block
      레이아웃을 정할 수 있으며, 공통되는 레이아웃 부분을 따로 관리할 수 있다는 장점 ! 
      include와 함께 사용되기도 함 
      (layout, body.html)

      뼈대가 될 파일에 공통된 내용(태그)를 넣으나, 
      페이지마다 달라지는 부분은 block이라는 것으로 비워둔다 
      block은 여러개 만들어도 되고,
      { % block [블록이름] % } 으로 선언하고, { % endblock % } 으로 종료

      block이 되는 파일에서는 { % extend 경로 % } 키워드로
      뼈대(레이아웃) 파일을 지정하고 block 부분을 넣는다.
      후에 express에서 res.render('body')를 사용해서
      하나의 HTML로 합쳐서 렌더링을 할 수 있는데...
        => 이 때 같은 이름의 block 부분이 합쳐진다. 
      
      템플릿에서 { % block % } 과 { % endblock % } 사이에
      내용이 있는 경우, 
        => { % extends % } 가 실행되면 기존 내용은 사라진다. 
      
      그리고 한 파일에, 여러개의 block을 지정해 줄 수 있다. 
        => BUT ! 블록의 이름에 주의할 것 ! 
    -->
  </body>
</html>

```

# views/header.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>상단</h1>
    <div>
      <a href="">HOME</a>
      <a href="">Test</a>
    </div>
  </body>
</html>

```

# views/body.html

```html
{% extends 'layout.html' %} {% block content %}
<div>여기
{% endblock %}는 메인 내용!</div>

```

# views/footer.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div>여기는 아래쪽 부분</div>
  </body>
</html>

```

# views/layout.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <title>Page Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" type="text/css" media="screen" href="main.css" />
    <script src="main.js"></script>
  </head>
  <body>
    <div>대충 메뉴인 공간</div>

    {% block content %} {% endblock %}
    <div>대충 끝부분인 공간</div>
  </body>
</html>

```

# views/main.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    {% include "header.html" %}
    <div>
      <h1>메인부분</h1>
      <p>다른 파일을 include 할 수 있다 !</p>
    </div>
    {% include "footer.html" %}
  </body>
</html>

```









