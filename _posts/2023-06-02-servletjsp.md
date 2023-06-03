---
layout: single
title: "Servlet & JSP"
categories: spring
tag: [java, jsp, servlet]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# 서블릿과 JSP

### 1. 서블릿과 컨트롤러의 비교

- 서블릿은 @WebServlet 어노테이션 사용 => @Controller + @RequestMapping 

- 서블릿은 HttpServlet을 상속받음 

- 반면, JSP는 상속을 받지 않음

- JSP는 @Controller 를 사용 

- 서블릿은 url매핑을 클래스 단위로 함 → 클래스를 여러개 생성
- JSP는 메소드 단위로 url매핑 → 하나의 클래스 안에 여러개의 메소드 생성해서 url 매핑

![image-20230602122803011](/images/2023-06-02-servletjsp/image-20230602122803011.png)

### 2. 서블릿의 생명주기

서블릿의 기본적인 메서드 3개 

- init () {} : 서블릿 초기화 - 서블릿이 생성 또는 리로딩 때, 한번만 수행됨. 
- service () {} : 호출될 때마다 반복적으로 수행됨.
- destroy () {} : 뒷정리 작업 - 서블릿이 제거될 때, 단 한번만 수행됨. 

```java
@WebServlet("/hello")
public class HelloServlet extends HttpServlet{

	@Override
	public void init() throws ServletException {
		// 서블릿이 초기화될 때 자동 호출되는 메서드
		// 1. 서블릿의 초기화 작업 담당 
		System.out.println("[HelloServlet] init() is called");
	}
	
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// 1. 입력 
		// 2. 처리
		// 3. 출력 
		System.out.println("[HelloServlet] service() is called");
	}

	@Override
	public void destroy() {
		// 3. 뒷정리 - 서블릿이 메모리에서 정리될 때 서블릿 컨테이너에 의해서 자동 호출 
		System.out.println("[HelloServlet] destroy() is called");
	}


}
```

![image-20230602140801750](/images/2023-06-02-servletjsp/image-20230602140801750.png)

서블릿 : Singleton 싱글톤 (1개의 인스턴스 생성 후 재활용)

### 3. JSP(Java ServerPage) 란?

- .jsp는 자동으로 서블릿 파일로 변환

- [src/main/webapp/twoDice.jsp]

```java
<%@ page contentType="text/html;charset=utf-8"%>
<%@ page import="java.util.Random" %>
<%-- <%! 클래스 영역 %> --%>
<%!  
	int getRandomInt(int range){
		return new Random().nextInt(range)+1;
	}
%>
<%-- <%  메서드 영역 - service()의 내부 %> --%>
<%
	int idx1 = getRandomInt(6);
	int idx2 = getRandomInt(6);
%>
<html>
<head>
	<title>twoDice.jsp</title>
</head>
<body>
	<img src='resources/img/dice<%=idx1%>.jpg'>
	<img src='resources/img/dice<%=idx2%>.jpg'>
</body>
</html>
```

- Servlet : 느린 초기화 lazy-init
- Spring : early-init

### 4. JSP와 서블릿의 비교

![image-20230602145914644](/images/2023-06-02-servletjsp/image-20230602145914644.png)

### 5. JSP의 호출 과정



![image-20230602145654059](/images/2023-06-02-servletjsp/image-20230602145654059.png)



### 6. JSP와 서블릿으로 변환된 JSP의 비교

- 함수 선언부, 지역변수, 출력(html) 모두 변환된다.

![image-20230602152259974](/images/2023-06-02-servletjsp/image-20230602152259974.png)



### 7. JSP의 기본 객체

- 생성없이 사용할 수 있는 객체

![image-20230602152648658](/images/2023-06-02-servletjsp/image-20230602152648658.png)

### 8. 유효 범위(scope)와 속성(attribute)

- HTTP 특징 - 상태정보 저장 X (stateless) => 저장소가 필요

- 4개의 저장소  map 형태(key, value)로 존재
  - 접근 범위 
  - 생존기간 

  
  
  1. pageContext
  
     - 접근 범위 : 페이지 안에서만 접근 가능(읽기, 쓰기) 
     - EL <span style="background-color:yellow">${}</span> 을 사용하기 위함 => pageContext에 저장한 후 읽어올 수 있다. 
     - 생존기간 : 요청할 때마다 초기화 
  
  2. application
  
     - 접근 범위 : WebApp 전체에서 접근 가능 (1개 존재)
  
       ![image-20230602154845872](/images/2023-06-02-servletjsp/image-20230602154845872.png)
  
       클라이언트들을 분간하기 위해 세션이라는 저장소가 나옴
  
       ![image-20230602155104279](/images/2023-06-02-servletjsp/image-20230602155104279.png)
  
       
  
  3. session
  
     - id, 장바구니 정보 담기에 적합. 
     - 접근 범위 : 클라이언트가 접근 가능한 페이지들.
     - 가장 간편하게 저장이 가능한다.
     - 메모리 부담이 크다. 
     - 사용자별 1개의 객체가 생성되므로 최소한의 data만 넣기. 
  
     
  
     ![image-20230602155430100](/images/2023-06-02-servletjsp/image-20230602155430100.png)
  
  4. request
  
     - 요청시 생기고 서로 독립적이다. 
     - 요청이 처리되는 동안만 존재. 
     - 메모리 부담이 제일 적다. 
     - 다른 페이지에 데이터를 전달할 때 request객체를 사용할 수 있는지 먼저 고려. 
     - request가 안되면 session을 사용하되 바로 삭제 가능하도록 만들기. 
  
     ![image-20230602155844730](/images/2023-06-02-servletjsp/image-20230602155844730.png)

<hr>

| 기본 객체   | 유효 범위      | 설명                                                         |
| ----------- | -------------- | ------------------------------------------------------------ |
| pageContext | 1개 JSP페이지  | JSP페이지의 시작부터 끝까지. 해당 JSP 내부에서만 접근 가능. 페이지당 1개 |
| request     | 1+개 JSP페이지 | 요청의 시작부터 응답까지. 다른 JSP로 전달 가능. 요청마다 1개 |
| session     | n개 JSP페이지  | session 시작부터 종료까지(로그인~로그아웃). 클라이언트마다 1개 |
| application | context 전체   | Web Application의 시작부터 종료까지. context내부 어디서나 접근 가능. 모든 클라이언트가 공유. context마다 1개 |

| 속성 관련 메서드                                             | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| void  <span style="color:red">setAttribute</span>(String name, Object value) | 지정된 값(value)을 지정된 속성 이름(name)으로 저장 [<span style="background-color:yellow">쓰기</span>] |
| Object<span style="color:red">getAttribute</span>(String name) | 지정된 이름(name)으로 저장된 속성의 값을 반환  [<span style="background-color:yellow">읽기</span>] |
| void removeAttribute(String name)                            | 지정된 이름(name)의 속성을 삭제                              |
| Enumeration getAttributeNames()                              | 기본 객체에 저장된 모든 속성의 이름을 반환                   |



### 9. URL 패턴

- @WebServlet 으로 서블릿을 URL에 매핑할 때 사용 

  | 종류                 | URL pattern     | 매칭 URL                                                     |
  | -------------------- | --------------- | ------------------------------------------------------------ |
  | 1. exact mapping     | /login/hello.do | http://loclahost/ch2 <span style="color:red">/login/hello.do</span> |
  | 2. path mapping      | /login/*        | http://loclahost/ch2<span style="color:red">/login/</span> <br />http://loclahost/ch2<span style="color:red">/login/hello</span> <br />http://loclahost/ch2 <span style="color:red">/login/hello.do</span><br />http://loclahost/ch2 <span style="color:red">/login/test/</span> |
  | 3. extension mapping | *.do            | http://loclahost/ch2/hi<span style="color:red">.do</span><br />http://loclahost/ch2/login/hello<span style="color:red">.do</span><br /> |
  | 4. default mapping   | /               | http://loclahost/ch2<span style="color:red">/</span> <br />http://loclahost/ch2<span style="color:red">/hello.do</span> <br />http://loclahost/ch2<span style="color:red">/login/</span> <br />http://loclahost/ch2<span style="color:red">/login/hello</span> <br />http://loclahost/ch2<span style="color:red">/login/hello.do</span> <br /> |

  

### 10. EL (Expression Language)

- <%=값%> 	=>	${값} : 간단하고 편리하게 사용가능

- el은 null을 출력하지 않는다. 

- lv(지역변수) 를 사용할 수 없다. 

- "1" + 1 = 2 (JS와 동일)

- null + 1 = 0

- null + null = 0

  ```
  << EL >>
  
  person.getCar().getColor()=red
  person.getCar().getColor()=red
  person.getCar().getColor()=red
  name=남궁성
  name=남궁성
  name=남궁성
  id=null
  id=
  id=
  "1"+1 = 2
  "1"+="1" = 11
  "2">1 = true
  null =
  null+1 = 1
  null+null = 0
  "" + null = 0
  ""-1 = -1
  empty null=true
  empty list=true
  null==0 = false
  null eq 0 = false
  name == "남궁성"=true
  name != "남궁성"=false
  name eq "남궁성"=true
  name ne "남궁성"=false
  name.equals("남궁성")=true
  ```

   

### 11. JSTL(JSP Standard Tag Library)

- <%@ taglib prefix="c"   uri="http://java.sun.com/jsp/jstl/core"%>
  <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
- 위 두가지는 기본으로 적용. 

```java
<%@ page contentType="text/html;charset=utf-8"%>
<%@ taglib prefix="c"   uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<html>
<head>
	<title>JSTL</title>
</head>
<body>
<c:set var="to"   value="10"/>		**저장소에 저장 key=to valu=10.
    								**scope = "page" 가 생략.
<c:set var="arr"  value="10,20,30,40,50,60,70"/> 
<c:forEach var="i" begin="1" end="${to}">
	${i}
</c:forEach>
<br>
<c:if test="${not empty arr}">
	<c:forEach var="elem" items="${arr}" varStatus="status"> 
        										**status=>																	1. count : 1부터 시작
        											2. index : 0부터 시작
		${status.count}. arr[${status.index}]=${elem}<BR>
	</c:forEach>
</c:if>	
<c:if test="${param.msg != null}">
	msg=${param.msg} 
	msg=<c:out value="${param.msg}"/>	**out => 태그를 그대로 찍어준다. 
											=> Script 공격을 막아준다.         
</c:if>
<br>
<c:if test="${param.msg == null}">메시지가 없습니다.<br></c:if>
<c:set var="age" value="${param.age}"/>
<c:choose>
	<c:when test="${age >= 19}">성인입니다.</c:when>
	<c:when test="${0 <= age && age < 19}">성인이 아닙니다.</c:when>
	<c:otherwise>값이 유효하지 않습니다.</c:otherwise>
</c:choose>
<br>
<c:set var="now" value="<%=new java.util.Date() %>"/>
Server time is <fmt:formatDate value="${now}" type="both" pattern="yyyy/MM/dd HH:mm:ss"/>	
</body>
</html>
	
	
```



### 12. Filter

- 공통적인 요청 전처리와 응답 후처리에 사용. 로깅, 인코딩 등 

​	![image-20230602174131163](/images/2023-06-02-servletjsp/image-20230602174131163.png)

​			![image-20230602174319838](/images/2023-06-02-servletjsp/image-20230602174319838.png)

​		



*[출처] 패스트캠퍼스 - 남궁성 (스프링의 정석)*
