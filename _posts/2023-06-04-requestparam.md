---
layout: single
title: "@RequestParam과 @ModelAttribute"
categories: spring
tag: [java, anotation]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# @RequestParam과 @ModelAttribute

### 1. @ RequestParam

요청의 파라미터를 연결할 매개변수에 붙이는 애너테이션

![image-20230604221301627](/images/2023-06-04-requestparam/image-20230604221301627.png)

- RequestParam 필수여부 => required. 
- 아무것도 입력하지 않으면 null 값.
- 파라미터 키값만 입력하면 ""빈문자열이 들어감.
- 400번대 : client error
- 500번대 : server error

![image-20230604221617869](/images/2023-06-04-requestparam/image-20230604221617869.png)

- required가 false여서 아무것도 입력하지 않았지만 파라미터가 int형이기 때문에 null 값을 받으면 500번대 에러가 발생.
- required가 true일때, null 값이 들어가면 400번대 에러가 발생.
- required가 true일때, 빈문자열이 들어가도 int형으로 변환이 안되기때문에 400번대 에러가 발생.

 

### 2. @ModelAttribute

적용 대상을 Model의 속성으로 자동 추가해주는 애너테이션 

반환 타입 또는 컨트롤러 메서드의 매개변수에 적용 가능 

컨트롤러의 매개변수 : 

- @RequestParam → 기본형, String 일 때 (생략가능)
- @ModelAttribute → 참조형일 때 (생략가능)



### 3. WebDataBinder

타입변환과 데이터 검증을 통해 BindingResult에 저장 후 컨트롤러에 넘겨줄 수 있다. 

바인딩 할 객체의 바로 뒤에 온다. 

```java
@RequestMapping("/getYoilMVC5")
public String main(@ModelAttribute MyDate date, BindingResult result) {}
```



http://localhost/ch2/getYoilMVC5?<span style="color:red">year=2023&month=10&day=1</span>

파라미터 값이 String 이지만 MyDate 타입의 객체에는 int 로 변환되어 저장 





