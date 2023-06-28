---
layout: single
title: "Mongoose"
categories: nodejs
tag: [nosql, javascript, cmd]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# schemas/index.js



```js
const mongoose = require('mongoose');

const connect = () => {
  // 개발 환경일 때만 콘솔을 통해서 몽구스가 생성하는 쿼리 내용을 확인할 수 있는 코드
  if (process.env.NODE_ENV !== 'production') {
    mongoose.set('debug', true);
  }

  // 몽구스랑 몽고디비를 연결
  // 주소 형식은 mongodb://[username:password]@[IP Address]:[Port]/[Database]
  // 접속을 시도하는 주소의 데이터베이스는 admin
  // 실제로 사용할 데이터베이스는 nodejs이어서
  // dbName 옵션을 줘서 nodejs 데이터베이스를 사용하게 함
  mongoose
    .connect('mongodb://oz:1@localhost:27017/admin', {
      dbName: 'nodejs',
      useNewUrlParser: true,
    })
    .then(() => {
      console.log('몽고디비 연결 성공');
    })
    .catch((err) => {
      console.error('몽고디비 연결 에러', err);
    });
};

// 이벤트 연결 => 에러 발생시, 에러 내용을 기록하고, 종료시 재연결을 시도함
// 실행시 MongooseServerSelectionError : 데이터베이스 실행 안했을 때
//        MongoServerError : 비밀번호 틀렸을 때
// 주로 이런 에러가 발생할 것
mongoose.connection.on('error', (error) => {
  console.error('몽고디비 연결 에러', error);
});
mongoose.connection.on('disconnected', () => {
  console.error('몽고디비 연결이 끊어졌습니다. 연결을 재시도 합니다.');
  connect();
});

module.exports = connect;

```



# app.js

```js
const express = require('express');
const path = require('path');
const morgan = require('morgan');
const nunjucks = require('nunjucks');

const connect = require('./schemas');

const indexRouter = require('./routes/index');
const usersRouter = require('./routes/users');
const commentsRouter = require('./routes/comments');

const app = express();
app.set('port', process.env.PORT || 3000);
app.set('view engine', 'html');
nunjucks.configure('views', {
  express: app,
  watch: true,
});

connect();

app.use(morgan('dev'));
app.use(express.static(path.join(__dirname, 'public')));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/comments', commentsRouter);

app.use((req, res, next) => {
  const error = new Error(`${req.method} ${req.url} 라우터가 없습니다.`);
  error.status = 404;
  next(error);
});

app.use((err, req, res, next) => {
  res.locals.message = err.message;
  res.locals.error = process.env.NODE_ENV !== 'production' ? err : {};
  res.status(err.status || 500);
  res.render('error');
});

app.listen(app.get('port'), () => {
  console.log(app.get('port'), '번 포트에서 동작 중!');
});

```



# schemas/user.js

```js
const mongoose = require('mongoose');

// 스키마 : 데이터베이스를 매핑하는데 사용되는 정보(문서)의 형식
//  몽구스의 모든것은 스키마로 시작!

// 몽구스 모듈에서 _id는 알아서 기본키로 생성하므로 딱히 명시할 필요 X
// 몽구스에서의 자료형은 몽고디바와는 다르고, 그 종류는
// String : 문자열 / Number : 숫자 / Date : 날짜
// Buffer : 버퍼 (이진데이터) / Boolean : 참, 거짓
// Mixed : 혼합타입 (다양한 타입 저장 가능)
// ObjectId (각 문서마다 만들어지는 Id) / Array : 배열

// 옵션
//  type : 자료형 지정
//  required : NOT NULL 과 같은 기능
//  unique : true면 고유한 값이 들어가야 함
//  default : 기본값

const { Schema } = mongoose;
const userSchema = new Schema({
  name: {
    type: String,
    required: true,
    unique: true,
  },
  age: {
    type: Number,
    required: true,
  },
  married: {
    type: Boolean,
    required: true,
  },
  comment: String,
  createAt: {
    type: Date,
    default: Date.now,
  },
});

module.exports = mongoose.model('User', userSchema);

```



# schemas/comment.js

```js
const mongoose = require('mongoose');

const { Schema } = mongoose;
const {
  Types: { ObjectId },
} = Schema;

const commentSchema = new Schema({
  // commenter의 자료형은 ObjectId이다
  // 옵션으로 ref 속성의 값이 'User'이다
  // 나중에 JOIN과 유사한 기능을 할 때 사용
  commenter: {
    type: ObjectId,
    required: true,
    ref: 'User',
  },
  comment: {
    type: String,
    required: true,
  },
  createAt: {
    type: Date,
    default: Date.now,
  },
});

module.exports = mongoose.model('Comment', commentSchema);

```



# views/mongoose.html



![code](/images/2023-06-28-jun28mongoose/code-1687930531370-2.png)

# views/error.html

![code](/images/2023-06-28-jun28mongoose/code-1687930559141-4.png)

# public/mongoose.js



```js
// get / post / patch / delete
//              수정  / 삭제

// 사용자 이름을 눌렀을 때 댓글 로딩
document.querySelectorAll('#user-list tr').forEach((el) => {
  el.addEventListener('click', function () {
    const id = el.querySelector('td').textContent;
    getComment(id);
  });
});

// 사용자 로딩
async function getUser() {
  try {
    const res = await axios.get('/users');
    const users = res.data;
    console.log(users);
    const tbody = document.querySelector('#user-list tbody');
    tbody.innerHTML = '';
    users.map(function (user) {
      const row = document.createElement('tr');
      row.addEventListener('click', () => {
        getComment(user.id);
      });
      // 로우 셀 추가
      let td = document.createElement('td');
      td.textContent = user.id;
      row.appendChild(td);

      td = document.createElement('td');
      td.textContent = user.name;
      row.appendChild(td);

      td = document.createElement('td');
      td.textContent = user.age;
      row.appendChild(td);

      td = document.createElement('td');
      td.textContent = user.married ? '기혼' : '미혼';
      row.appendChild(td);
      tbody.appendChild(row);
    });
  } catch (err) {
    console.log(err);
  }
}

// 댓글 로딩
async function getComment(id) {
  try {
    const res = await axios.get(`/users/${id}/comments`);
    const comments = res.data;
    const tbody = document.querySelector('#comment-list tbody');
    tbody.innerHTML = '';
    comments.map(function (comment) {
      // 로우 셀 추가
      const row = document.createElement('tr');
      let td = document.createElement('td');
      td.textContent = comment.id;
      row.appendChild(td);

      td = document.createElement('td');
      td.textContent = comment.User.name;
      row.appendChild(td);

      td = document.createElement('td');
      td.textContent = comment.comment;
      row.appendChild(td);

      const edit = document.createElement('button');
      edit.textContent = '수정';
      edit.addEventListener('click', async () => {
        // 수정 클릭 시
        const newComment = prompt('바꿀 내용을 입력하세요.');
        if (!newComment) {
          return alert('내용을 반드시 입력하세요');
        }
        try {
          await axios.patch(`/comments/${comment.id}`, { comment: newComment });
          getComment(id);
        } catch (err) {
          console.error(err);
        }
      });
      const remove = document.createElement('button');
      remove.textContent = '삭제';
      remove.addEventListener('click', async () => {
        // 삭제 클릭 시
        try {
          await axios.delete(`/comments/${comment.id}`);
          getComment(id);
        } catch (err) {
          console.error(err);
        }
      });
      // 버튼 추가
      td = document.createElement('td');
      td.appendChild(edit);
      row.appendChild(td);

      td = document.createElement('td');
      td.appendChild(remove);
      row.appendChild(td);
      tbody.appendChild(row);
    });
  } catch (err) {
    console.error(err);
  }
}

// 사용자 등록 시
document.getElementById('user-form').addEventListener('submit', async (e) => {
  // a 태그나 submit 태그는 누르게 되면 href를 통해 이동하거나,
  // 창이 새로고침해서 실행됨
  // preventDefault를 통해서 위의 동작들을 막아주는 역할
  e.preventDefault();
  const name = e.target.username.value;
  const age = e.target.age.value;
  const married = e.target.married.checked;
  if (!name) {
    return alert('이름을 입력하세요');
  }
  if (!age) {
    return alert('나이를 입력하세요');
  }

  try {
    await axios.post('/users', { name, age, married });
    getUser();
  } catch (err) {
    console.error(err);
  }
  e.target.username.value = '';
  e.target.age.value = '';
  e.target.married.checked = false;
});

// 댓글 등록 시
document
  .getElementById('comment-form')
  .addEventListener('submit', async (e) => {
    e.preventDefault();
    const id = e.target.userid.value;
    const comment = e.target.comment.value;
    if (!id) {
      return alert('아이디를 입력하세요');
    }
    if (!comment) {
      return alert('댓글을 입력하세요');
    }

    try {
      await axios.post('/comments', { id, comment });
      getComment(id);
    } catch (err) {
      console.error(err);
    }
    e.target.userid.value = '';
    e.target.comment.value = '';
  });

```



# routes/index.js

```js
const express = require('express');
const User = require('../schemas/user');

const router = express.Router();

// User.find({}) 로 모든 사용자를 찾은 후에
// mongoose.html을 렌더링 할 때 users 변수를 넣음
router.get('/', async (req, res, next) => {
  try {
    const users = await User.find({});
    res.render('mongoose', { users });
  } catch (err) {
    console.error(err);
    next(err);
  }
});

module.exports = router;

```



# routes/users.js

```js
const express = require('express');
const User = require('../schemas/user');
const Comment = require('../schemas/comment');

const router = express.Router();

router
  .route('/')
  .get(async (req, res, next) => {
    // get방식으로 데이터를 JSON형태로 반환
    try {
      const users = await User.find({});
      res.json(users);
    } catch (err) {
      console.error(err);
      next(err);
    }
  })
  // POST 방식으로 데이터를 등록(.create({}))
  // 정의한 스키마에 부합하지 않는 데이터를 넣게 되면 에러!
  // _id는 자동으로 생성됨
  .post(async (req, res, next) => {
    try {
      const user = await User.create({
        name: req.body.name,
        age: req.body.age,
        married: req.body.married,
      });
      console.log(user);
      res.status(201).json(user);
    } catch (err) {
      console.error(err);
      next(err);
    }
  });

// 댓글 조회하는 라우터
// find으로 댓글을 쓴 사용자의 아이디로 댓글을 조회한 후
// populate 메소드로 관련있는 컬렉션의 다큐먼트를 불러옴
// Comment 스키마 commenter의 ref가 User로 되어있으므로,
// 자동으로 user 컬렉션에서 사용자 다큐먼트를 찾아서 합치게 됨
router.get('/:id/comments', async (req, res, next) => {
  try {
    const comments = await Comment.find({ commenter: req.params.id }).populate(
      'commenter'
    );
    console.log(comments);
    res.json(comments);
  } catch (err) {
    console.error(err);
    next(err);
  }
});

module.exports = router;

```



# routes/comments.js

```js
const express = require('express');
const Comment = require('../schemas/comment');

const router = express.Router();

// 다큐먼트를 등록하는 라우터
// create 메소드로 댓글을 저장한 후,
// populate 메소드로 반환된 comment 객체의 다른 컬렉션 다큐먼트를 불러옴
// 이후, path 옵션으로 어떤 필드를 합칠지 설정하고, 합쳐진 결과를 클라이언트로 응답
router.post('/', async (req, res, next) => {
  try {
    const comment = await Comment.create({
      commenter: req.body.id,
      comment: req.body.comment,
    });
    console.log(comment);
    const result = await Comment.populate(comment, { path: 'commenter' });
    res.status(201).json(result);
  } catch (err) {
    console.error(err);
    next(err);
  }
});

router
  .route('/:id')
  // patch는 update에 대한 라우터
  // update or updateOne 메소드를 사용
  // 첫 번째 파라미터로 어떤 다큐먼트를 수정할 지 지정
  // 두 번째 파라미터로 수정할 값을 지정
  // 몽고디비와 다르게 $set 연산자를 사용할 필요 없음!
  .patch(async (req, res, next) => {
    try {
      const result = await Comment.updateOne(
        {
          _id: req.params.id,
        },
        {
          comment: req.body.comment,
        }
      );
      res.json(result);
    } catch (err) {
      console.error(err);
      next(err);
    }
  })
  // delete는 삭제하기 위한 라우터
  // remove or deleteOne 메소드를 사용
  // 첫 번째 파라미터로 어떤 다큐먼트를 삭제할 지 지정
  .delete(async (req, res, next) => {
    try {
      const result = await Comment.deleteOne({ _id: req.params.id });
      res.json(result);
    } catch (err) {
      console.error(err);
      next(err);
    }
  });

module.exports = router;

```





# public/mongoose.js

```js
// get / post / patch / delete
//              수정  / 삭제

// 사용자 이름을 눌렀을 때 댓글 로딩
document.querySelectorAll('#user-list tr').forEach((el) => {
  el.addEventListener('click', function () {
    const id = el.querySelector('td').textContent;
    getComment(id);
  });
});

// 사용자 로딩
async function getUser() {
  try {
    const res = await axios.get('/users');
    const users = res.data;
    console.log(users);
    const tbody = document.querySelector('#user-list tbody');
    tbody.innerHTML = '';
    users.map(function (user) {
      const row = document.createElement('tr');
      row.addEventListener('click', () => {
        getComment(user._id);
      });
      // 로우 셀 추가
      let td = document.createElement('td');
      td.textContent = user._id;
      row.appendChild(td);

      td = document.createElement('td');
      td.textContent = user.name;
      row.appendChild(td);

      td = document.createElement('td');
      td.textContent = user.age;
      row.appendChild(td);

      td = document.createElement('td');
      td.textContent = user.married ? '기혼' : '미혼';
      row.appendChild(td);
      tbody.appendChild(row);
    });
  } catch (err) {
    console.log(err);
  }
}

// 댓글 로딩
async function getComment(id) {
  try {
    const res = await axios.get(`/users/${id}/comments`);
    const comments = res.data;
    const tbody = document.querySelector('#comment-list tbody');
    tbody.innerHTML = '';
    comments.map(function (comment) {
      // 로우 셀 추가
      const row = document.createElement('tr');
      let td = document.createElement('td');
      td.textContent = comment._id;
      row.appendChild(td);

      td = document.createElement('td');
      td.textContent = comment.commenter.name;
      row.appendChild(td);

      td = document.createElement('td');
      td.textContent = comment.comment;
      row.appendChild(td);

      const edit = document.createElement('button');
      edit.textContent = '수정';
      edit.addEventListener('click', async () => {
        // 수정 클릭 시
        const newComment = prompt('바꿀 내용을 입력하세요.');
        if (!newComment) {
          return alert('내용을 반드시 입력하세요');
        }
        try {
          await axios.patch(`/comments/${comment._id}`, {
            comment: newComment,
          });
          getComment(id);
        } catch (err) {
          console.error(err);
        }
      });
      const remove = document.createElement('button');
      remove.textContent = '삭제';
      remove.addEventListener('click', async () => {
        // 삭제 클릭 시
        try {
          await axios.delete(`/comments/${comment._id}`);
          getComment(id);
        } catch (err) {
          console.error(err);
        }
      });
      // 버튼 추가
      td = document.createElement('td');
      td.appendChild(edit);
      row.appendChild(td);

      td = document.createElement('td');
      td.appendChild(remove);
      row.appendChild(td);
      tbody.appendChild(row);
    });
  } catch (err) {
    console.error(err);
  }
}

// 사용자 등록 시
document.getElementById('user-form').addEventListener('submit', async (e) => {
  // a 태그나 submit 태그는 누르게 되면 href를 통해 이동하거나,
  // 창이 새로고침해서 실행됨
  // preventDefault를 통해서 위의 동작들을 막아주는 역할
  e.preventDefault();
  const name = e.target.username.value;
  const age = e.target.age.value;
  const married = e.target.married.checked;
  if (!name) {
    return alert('이름을 입력하세요');
  }
  if (!age) {
    return alert('나이를 입력하세요');
  }

  try {
    await axios.post('/users', { name, age, married });
    getUser();
  } catch (err) {
    console.error(err);
  }
  e.target.username.value = '';
  e.target.age.value = '';
  e.target.married.checked = false;
});

// 댓글 등록 시
document
  .getElementById('comment-form')
  .addEventListener('submit', async (e) => {
    e.preventDefault();
    const id = e.target.userid.value;
    const comment = e.target.comment.value;
    if (!id) {
      return alert('아이디를 입력하세요');
    }
    if (!comment) {
      return alert('댓글을 입력하세요');
    }

    try {
      await axios.post('/comments', { id, comment });
      getComment(id);
    } catch (err) {
      console.error(err);
    }
    e.target.userid.value = '';
    e.target.comment.value = '';
  });

```



