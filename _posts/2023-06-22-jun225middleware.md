---
layout: single
title: "Node.js - MiddleWare"
categories: nodejs
tag: [vscode, javascript]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# app.js

```js
// 미들웨어 (Middle Ware)
// Express의 핵심 기능 중 하나 !

// 공식 문서에 따르면 미들웨어는 요청 객체(req), 응답 객체(res),
// 그리고 애플리케이션의 요청-응답 주기 중
// 그 다음의 미들웨어 함수에 대한 액세스 권한을 갖는 함수이며,
// 그 다음의 미들웨어는 일반적으로 next라는 이름의 변수로 표시된다.. 라고 정의
// 요청과 응답의 중간(middle)에 위치하기 때문에 미들웨어라고 부름
// 요청과 응답을 조작해서 기능을 추가하기도 하고,
// 나쁜 요청을 걸러내는 역할을 한다

// app.use(미들웨어)의 형태로 사용  // app.use => Express 객체
// 파라미터의 자리에는 req, res, next인 함수를 넣어주면 되고,
// 위에서 아래로 순서대로 진행되며,
// 요청과 응답 사이에 기능을 추가할 수 있다
// next라는 파라미터가 등장하는데, 다음 미들웨어로 넘어가는 기능 !
// 이 next를 실행하지 않으면 다음 미들웨어가 실행되지 않는다

// 미들웨어가 실행되는 경우
//      app.use(미들웨어) : 모든 요청에서 미들웨어 실행
//      app.use('/abc', 미들웨어) : abc로 시작하는 요청에서 미들웨어 실행
//      app.post('/abc', 미들웨어) : abc로 시작하는 post요청에서 미들웨어 실행

const express = require("express");
const app = express();

app.use((req, res, next) => {
  console.log("모든 요청에 다 실행 됨!");
  next();
});

app.get(
  "/",
  (req, res, next) => {
    console.log("GET / 요청에서만 실행 됨");
    next();
  },
  (req, res) => {
    // 여기에서 에러가 발생!
    throw new Error("에러는 에러처리 미들웨어로 간다!");
  }
);

// 파라미터가 총 4개 !, err에는 에러에 대한 정보가 들어있음!
// 에러처리 미들웨어는 직접 연결하지 않아도
// express가 에러를 기본적으로 처리하기는 하지만,
// 실무에서는 직접 처리해주는 것이 좋음
// 에러처리 미들웨어는 코드의 제일 아래에 위치하는 것이 좋다!
app.use((err, req, res, next) => {
  console.error(err);
  res.status(500).send(err.message);
});

app.listen(3000, () => {
  console.log("3000번 포트입니다~");
});
```

# app2.js

```js
// 자주 사용하는 미들웨어!

// npm install morgan cookie-parser express-session dotenv => 패키지 설치!

// dotenv 제외한 다른 패키지들은 미들웨어이다!
// dotenv는 서버의 환경변수를 뜻하고, 이를 관리해줌

const express = require("express");
const morgan = require("morgan");
const cookieParser = require("cookie-parser");
const session = require("express-session");
const dotenv = require("dotenv");
const path = require("path");

dotenv.config();
const app = express();

// 1. morgan - 요청과 응답에 대한 정보를 콘솔에 알려줌
app.use(morgan("dev"));
// GET / 200 5.759 ms - 23 는 각각 [요청방식] [주소] [상태코드] [응답속도] - [응답 바이트]
// 를 나타내고 있음

// app.get('/', (req, res) => {
//   res.send('연결되었습니다!~');
// });

// app.get => html 파일 불러오기
app.get("/", (req, res) => {
  res.sendFile(path.join(__dirname, "/index.html"));
});

// 'css' 폴더를 하나 만들어서 => 그 안에 index.css 파일 만들고
//  => h1태그 스타일 주기

// 분명 index.css를 html에 연결했는데 css가 먹히지 않음...!

// 2. static : 정적(static) 파일을 제공하는 역할을 해줌
// express 객체 안에 있는 기능이라서 꺼내서 쓰기만 하면 됨
// app.use('요청경로', express.static('실제 경로')); 로 사용

app.use("/", express.static(path.join(__dirname, "css"))); // css폴더를 지정!
// 주소값 뒤에 /index.css를 쳐보면 해당 코드가 나옴!
// 실제 폴더경로에는 css가 들어있지만, 요청 주소에는 css가 안들어가는데,
// 서버의 폴더 경로와 요청 경로가 달라서 외부인이 이 서버의 구조를
// 쉽게 파악할 수 없게 하는 보안에 장점이 있음 !
// css파일뿐만 아니라, js파일, 이미지 파일들을 저 폴더에 집어넣으면
// 브라우저에서 접근할 수 있게 됨!

app.listen(3000, () => {
  console.log("3000번 포트에 연결됨");
});
```

# app3.js

```js
// 쿠키 (Cookie)
// 쿠키는 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각
// 브라우저는 이 데이터 조각을 저장해 놓았다가,
// 동일한 서버에 재요청할 때 저장된 데이터를 함께 전송하게 됨

// 특징
// - key-value 형태의 데이터
// - 브라우저가 저장하는 데이터
// - 쿠키의 만료시간이 무한일 경우 브라우저가 종료되어야 쿠키도 없어진다

// 브라우저는 매 요청마다 서버에 쿠키를 동봉해서 보내는데
// - 쿠키가 없거나 만료되었을 때:
//      서버가 응답을 보낼 때 쿠키를 새롭게 설정해서 같이 보냄
// - 쿠키가 있을 때:
//      쿠키로 사용자 상태 정보를 식별함

// views 폴더 만들어서 => index.html

// npm install cookie-parser nunjucks chokidar => 설치

const express = require("express");
const morgan = require("morgan");
const cookieParser = require("cookie-parser");
const nunjucks = require("nunjucks");

const app = express();
app.use(morgan("dev"));
app.use(cookieParser()); // get방식 요청이 오면 uri변수들이
// 파싱돼서 req.cookies객체 저장됨
app.set("view engine", "html");
nunjucks.configure("views", {
  express: app,
  watch: true,
});

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.get("/", (req, res) => {
  const { user } = req.cookies;
  if (user) {
    res.render("login", { user });
  }

  res.render("index");
});

app.post("/", (req, res) => {
  const { name } = req.body;
  // 쿠키 생성
  res
    .cookie("user", name, {
      expires: new Date(Date.now() + 900000), // 만료기간
      httpOnly: true, // true 설정 시 자바스크립트로 쿠키에 접근할 수 없음
      secure: true, // HTTPS 통신일때만 쿠키 전송하겠다!
    })
    .redirect("/");
});

app.get("/delete", (req, res) => {
  // 쿠키 삭제
  res.clearCookie("user").redirect("/");
});

app.listen(3000, () => {
  console.log("서버 실행 중");
});
```

# app4.js

```js
// 세션 (Session)
// 쿠키와 세션의 차이는 데이터의 저장장소!
// 쿠키는 데이터를 브라우저에 저장하는 반면, 세션은 서버에 저장을 한다
// 세션은 서버에 각 사용자의 개인 저장소를 만들어주는 개념으로 생각
// 그런데 HTTP는 상태를 저장하지 않기 때문에...
// 각 사용자를 식별해줄 수 있는 값이 필요함... => 쿠키!!
// 그 정보를 '세션 쿠키'라고 함

// 특징
//  - 세션은 서버에 저장됨
//  - 서버는 세션쿠키로 사용자를 식별함
//  - 세션 '쿠키'이기 때문에 브라우저가 종료되면 세션쿠키도 사라짐

// npm install express-session => 설치

const express = require("express");
const morgan = require("morgan");
const cookieParser = require("cookie-parser");
const nunjucks = require("nunjucks");
const session = require("express-session");

const app = express();

app.use(morgan("dev"));

app.use(cookieParser());
app.use(
  session({
    resave: true, //요청이 왔을 때 세션에 수정사항이 생기지 않아도
    // 다시 저장할지 여부
    saveUninitialized: false, // 세션에 저장할 내역이 없더라도
    // 세션을 저장할지 여부
    secret: "secretkk", // 쿠키 암호화
    cookie: {
      // 쿠키 설정과 동일
      httpOnly: true,
      secure: false,
    },
    name: "oz-cookie", // 세션쿠키 이름 (connect.sid가 기본값)
    // store : 세션 저장소, 메모리가 기본값
  })
);

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.set("view engine", "html");
nunjucks.configure("views", {
  express: app,
  watch: true,
});

app.get("/", (req, res) => {
  const { user } = req.session; // req.session : 세션값들을 다 볼 수 있음
  if (user) {
    res.render("login", { user });
    return;
  }
  res.render("index");
});

// 이름 등록
app.post("/", (req, res) => {
  const { name } = req.body;
  req.session.user = name;
  res.redirect("/");
  // 개발자모드 - Application - Storage - Cookies에서 확인할 수 있음
});

// 세션 삭제
app.get("/delete", (req, res) => {
  req.session.destroy(); // req.session.destroy() : 세션 모두 제거
  res.redirect("/");
});

// 세션 데이터 추가
app.get("/addSession", (req, res) => {
  req.session.addData = "addData";
  console.log(req.sessionID); // req.sessionID : 세션 아이디 확인
  // (세션 쿠키의 value)
  res.redirect("/");
});

// 세션 데이터 보기
app.get("/lookSession", (req, res) => {
  res.render("sessionData", { sessions: req.session });
});

app.listen(3000, () => {
  console.log("3000번 포트에서 작동 중!");
});
```

# app5.js

```js
const express = require("express");
const morgan = require("morgan");
const nunjucks = require("nunjucks");
const bodyParser = require("body-parser");

const app = express();

app.use(morgan("dev"));
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.set("view engine", "html");
nunjucks.configure("views", {
  express: app,
  watch: true,
});

// views 폴더에 write.html 만들어서
// form안에 제품명 / 가격 / 설명 입력 버튼눌렀을 때 post방식 요청

app.get("/", (req, res) => {
  res.send("kkk");
});

app.get("/products/write", (req, res) => {
  res.render("write");
});

app.post("/products/write", (req, res) => {
  // res.send(req.body);
  res.send(req.body.name);
});

app.listen(3000, () => {
  console.log("3000번 포트에서 작동중!");
});

// body-parser  ?
// node.js의 post 요청 데이터를 추출할 수 있도록 만들어주는 미들웨어
// req.body: JSON 데이터를 담을 때 사용함(주로 POST로 정보 or 파일 업로드)
// bodyParser.json() 은 'application/json' 방식의 Content-Type 데이터를 받아줌
// bodyParser.urlencoded() 는 'application/x-www-form-urlencoded'방식의
//    Content-Type 데이터를 받아줌
//    (보내는 데이터를 URL인코딩해서 웹서버로 보내는 방식)
// urlencoded()의 메소드에는 { extends : true}라는 옵션이 있는데,
// false값이면 querystring 모듈을 사용해서 데이터를 해석하고,
// true값이면 qs모듈을 사용해서 데이터를 해석함
//  => 둘 다 비슷한데 qs모듈이 조금 더 기능이 확장되었음
```

# app6.js

```js
// multer (이미지 / 파일 업로드를 위한 모듈)
// npm install multer@1.4.4 => 설치 (버전 없이 설치하면 이 이상버전에서 한글이 깨짐)
// multer 패키지 안에는 여러 종류의 미들웨어가 들어있음

// storage : 저장할 공간에 대한 정보. 메모리나 디스크 저장 가능
// diskStorage : 하드디스크에 업로드 파일을 저장한다는 것
// destination : 저장할 경로
// filename : 저장할 파일명
// Limits : 파일 개수나 파일 사이즈를 제한할 수 있음

const express = require("express");
const morgan = require("morgan");
const nunjucks = require("nunjucks");
const bodyParser = require("body-parser");
const multer = require("multer");
const fs = require("fs");
const path = require("path");

const app = express();

// 이미지 불러오기
app.use("/uploads", express.static("uploads"));

app.use(morgan("dev"));
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.set("view engine", "html");
nunjucks.configure("views", {
  express: app,
  watch: true,
});

// 아래에 파일 업로드 관련한 설정을 적을 건데...
// 아래 설정을 실제로 활용하기 위해서는 서버에 uploads 폴더가 꼭 존재해야 함
// 없다면 직접 만들어주거나, fs 모듈로 서버를 시작할 때 생성하는 방식이 있음

try {
  fs.readdirSync("uploads"); // 폴더 확인
} catch (err) {
  console.error("uploads 폴더가 없습니다. 폴더를 생성합니다.");
  fs.mkdirSync("uploads"); // 폴더 생성
}

const upload = multer({
  storage: multer.diskStorage({
    // 저장할 공간 확보 : 하드디스크에 저장
    destination(req, file, done) {
      // 저장 위치
      done(null, "uploads"); // uploads라는 폴더 안에 저장
    },
    filename(req, file, done) {
      // 파일명을 어떤 이름으로 업로드할지
      const ext = path.extname(file.originalname); // 파일의 확장자
      done(null, path.basename(file.originalname, ext) + ext);
      // 파일이름 + 확장자 이름으로 저장
    },
  }),
  limits: { fileSize: 10 * 1024 * 1024 }, // 10메가로 용량 제한
});

// multer에 넣은 파라미터들은...
// 먼저 storage 속성에는 - 어디에 (destination) / 어떤 이름으로 (filename)
// 저장할지를 넣었음
// 두 함수의 파라미터에는 요청에 대한 정보인 req,
// 업로드한 파일에 대한 정보가 있는 file,
// 어떤(?) 함수인 done
// done()함수는 첫 번째 파라미터에 에러가 있다면 에러를 넣고,
// 두 번째 파라미터에는 실제 경로나 파일 이름을 넣었음
// req나 file의 데이터를 가공해서 done으로 넘기는 형태
// 현재 설정으로는 upload 폴더에 [파일명.확장자] 파일명으로 업로드 될 것!
// Limits 속성에는 업로드에 대한 제한사항을 걸어두었음
// => 파일 크기를 10 MB로 제한한 상태
```

# views/index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <form action="/" method="post">
      <div>
        <label for="name">이름을 입력하세요</label>
      </div>
      <div>
        <input type="text" name="name" id="name" />
      </div>
      <div>
        <button>등록</button>
      </div>
    </form>
  </body>
</html>
```

# views/login.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    안녕하세요 {{user}} 님
    <div><a href="/lookSession">세션 데이터 보기</a></div>
    <div><a href="/addSession">세션 데이터 추가</a></div>
    <div><a href="/delete">로그아웃</a></div>
  </body>
</html>
```

# views/sessionData.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    세션 데이터
    <ul>
      <!-- {% for key, value in sessions %} -->
      <!-- <li>{{key}} : {{value}}</li> -->
      <!-- {% endfor %} -->
    </ul>
    <div>
      <a href="/">홈으로</a>
    </div>
  </body>
</html>
```

# views/write.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- POST 방식이라면 action 비워도 무방함
        (어차피 마지막에 요청한 주소쪽으로 요청할테니까!)
      -->
    <form action="" method="post">
      <table>
        <tr>
          <th>제품명</th>
          <td>
            <input type="text" name="name" />
          </td>
        </tr>
        <tr>
          <th>가격</th>
          <td>
            <input type="text" name="price" />
          </td>
        </tr>
        <tr>
          <th>설명</th>
          <td>
            <input type="text" name="description" />
          </td>
        </tr>
        <tr>
          <td>
            <button>등록</button>
          </td>
        </tr>
      </table>
    </form>
  </body>
</html>
```

# views/upload.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <form action="upload" method="post" enctype="multipart/form-data">
      <input type="file" name="image" />
      <p></p>
      <input type="text" name="title" />
      <p></p>
      <button>버튼</button>
    </form>
  </body>
</html>
```

# css/index.css

```css
h1 {
  background-color: aquamarine;
}
```

# index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <title>Page Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" type="text/css" media="screen" href="index.css" />
    <script src=""></script>
  </head>
  <body>
    <h1>ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ</h1>
  </body>
</html>
```

# package.json

```json
{
  "name": "05_middleware",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "start": "node app6.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "cowsay": "^1.5.0",
    "multer": "^1.4.4"
  }
}
```
