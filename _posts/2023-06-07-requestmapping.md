---
layout: single
title: "@GetMapping & @PostMapping"
categories: spring
tag: [java, eclipse]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# @GetMapping 과 @PostMapping 

### 1. @GetMapping, @PostMapping

*@RequestMapping 대신 @GetMapping, @PostMapping 사용 가능*

- method가 다르면 url이 같아도 상관없다.

```java
@Controller 
public class RegisterController {
//	@RequestMapping(value="/register/add", method=RequestMethod.GET, method=RequestMethod.POST)
//	@RequestMapping("/register/add") // 신규회원 입력화면 
	@GetMapping("/register/add")
	public String register() {
		return "registerForm"; // WEB-INF/views/registerForm.jsp
	}
	
//	@RequestMapping(value="/register/add", method=RequestMethod.POST)
	@PostMapping("/register/add") // 신규회원 등록
	public String save(User user, Model m) throws Exception {
		// 1. 유효성 검사
		if(!isValid(user)) {
			String msg = URLEncoder.encode("id를 잘못 입력하셨습니다.", "utf-8");
			
			m.addAttribute("msg", msg);
			return "redirect:/register/add"; 
//			return "redirect:/register/add?msg="+msg; // URL재작성(rewriting)
		}
```

![image-20230607115002635](/images/2023-06-07-requestmapping/image-20230607115002635.png)





*맵핑될 URL의 공통부분을 @RequestMapping으로 클래스에 적용*

### 2. 클래스에 붙이는 @RequestMapping

```java
@Controller 
@RequestMapping("/register")
public class RegisterController {
//	@RequestMapping(value="/register/add", method=RequestMethod.GET, method=RequestMethod.POST)
//	@RequestMapping("/register/add") 
	@GetMapping("/add") // 신규 회원 입력 화면
	public String register() {
		return "registerForm"; // WEB-INF/views/registerForm.jsp
	}
	
//	@RequestMapping(value="/register/add", method=RequestMethod.POST)
	@PostMapping("/add") // 신규회원 등록
	public String save(User user, Model m) throws Exception {
		// 1. 유효성 검사
		if(!isValid(user)) {
			String msg = URLEncoder.encode("id를 잘못 입력하셨습니다.", "utf-8");
			
			m.addAttribute("msg", msg);
			return "redirect:/register/add"; 
//			return "redirect:/register/add?msg="+msg; // URL재작성(rewriting)
		}
```

![image-20230607115104254](/images/2023-06-07-requestmapping/image-20230607115104254.png)



### 3. @RequestMapping의 URL패턴

- URL 패턴 
  - @WebServlet : 서블릿
  - @RequestMapping : 스프링

_?는 한 글자, *는 여러글자, **는 하위경로 포함. 배열로 여러 패턴 지정_

우선순위로 나열

| 종류                 | URL pattern     | 매칭 URL                                                     |
| -------------------- | --------------- | ------------------------------------------------------------ |
| 1. exact mapping     | /login/hello.do | http://localhost/ch2/login/hello.do                          |
| 2. path mapping      | /login/*        | http://localhost/ch2/login/<br />http://localhost/ch2/login/hello<br />http://localhost/ch2/login/hello.do<br />http://localhost/ch2/login/test/ |
| 3. extension mapping | *.do            | http://localhost/ch2/hi.do<br />http://localhost/ch2/login/hello.do |

1. exact mapping : 정확히 일치하는 패턴

2. path mapping : 경로 매핑
3. extension mapping : 확장자 매핑

### 4. URL인코딩 - 퍼센트 인코딩

_URL에 포함된 non-ASCII 문자를 문자 코드(16진수) 문자열로 변환_

- URLEncoder.encode() 
- URLDecoder.decode()

![image-20230607162601280](/images/2023-06-07-requestmapping/image-20230607162601280.png)

- Base64 : 바이너리 → Text로 변환 (혼동 주의!!)



