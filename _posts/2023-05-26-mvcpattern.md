---
layout: single
title: "관심사의 분리, MVC패턴"
categories: spring
tag: [mvc, spring, solid]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# _MVC 패턴_

### 1. 관심사의 분리 Seperation of Concerns

OOP 5대 설게원칙 - SOLID

- S - SRP : 단일 책임의 원칙
  - 하나의 메서드는 하나의 책임
  - 객체지향 프로그래밍 → 분리
    - 분리
      - 1. 관심사
        2. 변하는 것, (자주)변하지 않는 것 (common / uncommon)
        3. 공통(중복)코드

### 2. 공통 코드의 분리 - 입력의 분리

*분리 전*

```
1. 입력 - request.getParameter()
2. 처리
3. 출력

1. 입력 - request.getParameter()
2. 처리                                     **3가지 Controller
3. 출력

1. 입력 - request.getParameter()
2. 처리
3. 출력
```

*분리 후*

```
1. 입력
			→	2. 처리
				3. 출력
			
			→	2. 처리
            	3. 출력
            	
            →	2. 처리
            	3. 출력
			  
```

<hr>



*분리 전*

```java
@RequestMapping("/getYoil")
	public void main(HttpServletRequest request, HttpServletResponse response) throws IOException {
	// 1.입력
	 String year = request.getParameter("year");
	 String month = request.getParameter("month");
	 String day = request.getParameter("day");
	 
	 int yyyy = Integer.parseInt(year);
	 int mm = Integer.parseInt(month);
	 int dd = Integer.parseInt(day);
```

*분리 후*

```java
@RequestMapping("/getYoil")
	public void main(String year, String month, String day, HttpServletResponse response) throws IOException {
	 
	 int yyyy = Integer.parseInt(year);
	 int mm = Integer.parseInt(month);
	 int dd = Integer.parseInt(day);
```

*한번 더 분리 (스프링이 문자열을 자동으로 int형으로 변환해준다)*

```java
@RequestMapping("/getYoil")
	public void main(int year, int month, int day, HttpServletResponse response) throws IOException {
        
     // 2. 처리
     Calendar cal = Calendar.getInstance();
	 cal.set(year, month -1 , day);
```

### 3. 출력(view)의 분리 - 변하는 것과 변하지 않는 것의 분리

```java
 // 2. 처리
        Calendar cal = Calendar.getInstance();
        cal.set(yyyy, mm - 1, dd);

        int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);
        char yoil = " 일월화수목금토".charAt(dayOfWeek);   // 일요일:1, 월요일:2, ... 

        // 3. 출력
//        System.out.println(year + "년 " + month + "월 " + day + "일은 ");
//        System.out.println(yoil + "요일입니다.");
        response.setContentType("text/html");    // 응답의 형식을 html로 지정
        response.setCharacterEncoding("utf-8");  // 응답의 인코딩을 utf-8로 지정
        PrintWriter out = response.getWriter();  // 브라우저로의 출력 스트림(out)을 얻는다.
        out.println("<html>");
        out.println("<head>");
        out.println("</head>");
        out.println("<body>");
        out.println(year + "년 " + month + "월 " + day + "일은 ");
        out.println(yoil + "요일입니다.");
        out.println("</body>");
        out.println("</html>");
        out.close();
```

입력은 DispatcherServlet, 처리는 Controller, 출력은 View, 처리 결과 데이터를 주고 받을 수 있는 객체인 Model

![image-20230601092703398](/images/2023-05-26-mvcpattern/image-20230601092703398.png)

### 4. MVC 패턴

![image-20230601093133169](/images/2023-05-26-mvcpattern/image-20230601093133169.png)
