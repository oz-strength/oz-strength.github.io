---
layout: single
title: "Board 작성하기 / 수정하기"
categories: frontend
tag: [react, js, graphql, emotion]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---



# 31강. board 작성하기 / 수정하기



### 1. library 폴더의 utils 파일관리

- 실무용 폴더구조로 Container, Presenter 방식으로 파일을 분리하고, styles, queries파일 또한 분리했다.
- 파일을 분리해서 관리하는 것은 협업과 유지보수에 있어 매우 중요하다. 
- 디자인에 수정사항이 생기면 styles 파일로, api 수정사항이 생기면 queries파일을 확인하면 되기 때문에 수정에 용이하다. 
- 공통적으로 쓰이는 함수를 한 곳에 저장하여 협업하는 사람들과 함께 사용하는 방법도 있다.

```js
freeboard_frontend / src / commons / libraries / utils.js


export const getMyDate = (myDate) => {
	const aaa = new Date(myDate)
	const yyyy = aaa.getFullYear()
	const mm = aaa.getMonth() + 1 ; // 여기서 +1을 하는 이유는 월을 0~11까지 받아오기때문입니다
	const dd = aaa.getDate()
	return `${yyyy}-${mm}-${dd}`
}

```

- utils 파일 안에 함수를 하나 만들어서 export해준다. 
- myDate라는 매개변수를 받아서 원하는 날짜표현방식을 리턴한다. 

```js
import { getMyDate } from '~~/src/commons/libraries/utils';
export default function BoardListUI() {

	...

	return (
	<div>
	...
	{props.data?.fetchBoards.map((el, index) => (
		...
		<div>
			{getMyDate(el.createdAt)}
		</div>
	))}
	</div>
	)
}
```

- 위의 코드처럼 import해서 getMyDate라는 함수를 import하여 사용할 수 있다.

<hr>

### 컴포넌트 재사용성

_페이지를 컴포넌트로 나누어주는 이유는 재사용성과 유지보수가 용이하기 때문이다._

```js
// 08-01-example-new
	
export default function ExampleNewPage() {
	return (
		<div>
			<h1>등록페이지</h1>
			제목: <input type="text" /><br />
			내용: <input type="text" /><br />
			<button> 등록하기 </button>
		</div>
	)
}


// 08-02-example-edit

export default function ExampleEditPage () {
	return (
		<div>
			<h1>수정페이지</h1>
			제목: <input type="text" /><br />
			내용: <input type="text" /><br />
			<button> 수정하기 </button>
		</div>
	)
}

```

- 위와같이 등록페이지와 수정페이지 컴포넌트를 각각 따로 만들면 문제가 발생한다.
- 등록 페이지가 바뀐다고 해도 수정페이지가 바뀌지 않기 때문이다. 

```js
// src/components/unit/08-example/Example.js
	
export default function Example() {
	return (
		<div>
			<h1>등록페이지</h1>
			제목: <input type="text" /><br />
			내용: <input type="text" /><br />
			<button> 등록하기 </button>
		</div>
	)
}

```

- src 폴더 안에 컴포넌트를 만들어 필요한 곳에 import하여 사용하게끔 구현한다. 

```js
// 08-03-example-component-new
import Example from '../../src/components/units/08-example/Example'

export default function ExampleComponentNew () {
	return <Example />
}




// 08-04-example-component-edit
import Example from '../../src/components/units/08-example/Example'

export default function ExampleComponentEdit () {
	return <Example />
}
```

- 하지만 위와 같이 불러와도 등록페이지와 수정페이지가 똑같이 나오는 문제가 있다.
- 등록 페이지와 수정 페이지에 보이는 모습을 다르게 하기 위해서 <span style="color:red">props</span>와 <span style="color:red">삼항연산자</span>를 사용한다. 

```js
// 08-03-example-component-new
import Example from '../../src/components/units/08-example/Example'

export default function ExampleComponentNew () {
	return <Example isEdit={false} />
}




// 08-04-example-component-edit
import Example from '../../src/components/units/08-example/Example'

export default function ExampleComponentEdit () {
	return <Example isEdit={true} />
}
```

- 위의 코드처럼 각 페이지에서 isEdit을 수정하기 페이지일 경우 true, 등록하기 페이지일 경우 false로 주고 이  boolean값을 컴포넌트로 넘겨준다.  

```js
// src/components/unit/08-example/Example.js
	
export default function Example(props) {
	return (
		<div>
			<h1>{props.isEdit ? "수정하기" : "등록하기"}</h1>
			제목: <input type="text" /><br />
			내용: <input type="text" /><br />
			<button> {props.isEdit ? "수정완료" : "등록완료"} </button>
		</div>
	)
}

```

- src 폴더안의 Example.js파일도 isEdit의 boolean 타입에 따라 등록과 수정이 나오게끔 

<span style="color:red">props</span>와 <span style="color:red">삼항연산자</span>를 사용해준다. 

- 이렇게 컴포넌트에서 받은 isEdit을 다시 프리젠터로 넘겨줘서 이 값에 따라 true면 수정, false면 등록이 보이도록 해준다. 

<hr>

### 수정한 값만 바꿔주기

- 게시물을 수정할 때 수정한 값만 업데이트 되어야한다. 
- 수정 인풋에 내가 적었던 글을 가져오려면 인풋태그인 defaultValue에 기존값을 넣어두면 된다. 
- defaultValue에 기존 데이터를 가져오는 과정에서 fetch를 하는데, 수정에만 적용하기 위해 fetch는 페이지 컴포넌트에서 하고, 해당 data를 props로 넘겨준다. 

#### <span style="color:orange">defaultValue와 state</span>

defaultValue를 줬음에도 수정하기를 누르면, 수정하지 않은 부분에 빈 값이 들어가게 된다. 

이는 state의 초깃값 때문이다.

defaultValue는 실제 state가 아닌 input의 속성이기 때문에 실제 눈에 보이는 것과 달리 state에 저장되지 않는다. 

> 이를 해결하기 위해서
>
> 1. defaultValue를 state의 초깃값으로 넣는다.(비효율적)
> 2. 뮤테이션 할 때 변경된 부분만 보내준다.(효율적)

#### <span style="background-color:#FDF7C3">변경된 부분만 mutation 날려주는 코드작성</span>

```js
// 뮤테이션에 변경된 부분만 보내주기

cosnt xxx = async()=>{
const myVariables = {
		number : Number(router.query.mynumber),
	}

	//변경된 값만 객체에 key와 value 추가해주기
if(myWriter !== ""){ myVariables.writer = myWriter}
if(myTitle !== ""){ myVariables.title = myTitle}
if(myContents !== ""){ myVariables.contents = myContents}

	//뮤테이션 보내기
cosnt result = await ww({
			variables : myVariables
	})
}
```

- 위의 코드를 보면 myVariables라는 빈 객체를 선언 후 꼭 들어가야 하는 number를 넣어두고 조건문을 이요해서 state가 빈 값이 아닐 경우에만 key와 value를 추가해준다. 
- 그리고 완성된 객체를 variables에 넣어서 뮤테이션을 날려준다. 



