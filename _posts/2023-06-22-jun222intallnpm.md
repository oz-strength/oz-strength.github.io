---
layout: single
title: "Node.js - 02_Install NPM"
categories: javascript
tag: [vscode, node]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# main.js

```js
// NPM은 Node Package Manager의 약자로,
// Node로 생성한 패키지/프로젝트를 관리하는 도구이다

// NPM은 Java의 Maven이나, Python의 pip처럼
// 만들어놓은 Node.js 모듈들을 공유하는 공간

// 터미널 => npm init -y
//      npm을 쓸 수 있는 초기 환경을 설정
//      (-y : 이걸 안쓰면 일일이 입력해서 설정한 후 yes를 입력해야 하는데
//      입력없이 한번에 처리해 줌)
// 그리고 이런 패키지/프로젝트 정보를 가지고 있는 것이
// package.json 파일

// 구조 살펴보기
// {
//   "name": "sol_nodejs",        // 이 프로젝트의 이름
//   "version": "1.0.0",          // 이 프로젝트의 버전
//   "description": "",           // 이 프로젝트에 대한 설명
//   "main": "index.js",          // 해당 패키지의 진입점이 되는 모듈
//   "scripts": {                 // 스크립트 명령어를 담고 있음
//     "test": "echo \"Error: no test specified\" && exit 1"
//   },
//   "keywords": [],              // 패키지를 문자열 배열로 설명해 줌
//   "author": "",                // 작성자
//   "license": "ISC"             // 패키지에 대한 권한
//   "dependencies" : ...         // 모듈 의존성
// }

// 3rd party module 불러오기!
// 터미널 => npm install cowsay
// => package-lock.json 생기면서 package.json 둘 다 cowsay에 대한
//      dependencies가 생긴 것을 확인
// package-lock.json : 나중에 이 프로젝트를 배포할 때,
//    이 프로젝트에 필요한 모듈의 버전을 명시해 둔 곳
// 이걸 토대로 다른 컴퓨터에서 이 프로젝트를 실행할 때
// 이 내용을 바탕으로 'npm install' 하면 관련된 모듈들이 한번에 받아짐

const cowsay = require('cowsay');
console.log(
  cowsay.say({
    text: '식사 맛있게 드세요~!',
  })
);

// 터미널에서 node main.js 실행

```

```
 ______________________
< 식사 맛있게 드세요~! >
 ----------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```



