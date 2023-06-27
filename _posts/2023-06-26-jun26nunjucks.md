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

![code](/images/2023-06-26-jun26nunjucks/code.png)



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

![code](/images/2023-06-26-jun26nunjucks/code-1687827163454-2.png)



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

![code](/images/2023-06-26-jun26nunjucks/code-1687827225514-4.png)



# views/main.html

![code](/images/2023-06-26-jun26nunjucks/code-1687827266128-6.png)











