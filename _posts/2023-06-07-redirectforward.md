---
layout: single
title: "redirect와 forward"
categories: spring
tag: [java, eclipse]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# Redirect와 Forward

### 1. redirect와 forward의 처리 과정 비교

1. redirect

   - 300 번대 => redirect
   - 다른 url로 재요청
   - head만 있고 body는 없다.
   - 첫 요청은 수동으로 하지만 재요청은 자동으로 이루어진다.
   - 요청 2 / 응답 2

   ![image-20230607171712811](/images/2023-06-07-redirectforward/image-20230607171712811.png)

2. forward

   - 사용자가 요청한 내용을 그대로 전달

   - 요청 1 / 응답 1

     ![image-20230607172013559](/images/2023-06-07-redirectforward/image-20230607172013559.png)

### 2. RedirectView

![image-20230607172326472](/images/2023-06-07-redirectforward/image-20230607172326472.png)

### 3. JstlView

- InternalResourceViewResolver에서 registerForm을 해석 후 JstlView로 보낸다.

  ![image-20230607172644137](/images/2023-06-07-redirectforward/image-20230607172644137.png)

  _servlet-context.xml_

  ```xml
  <!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
  	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  		<beans:property name="prefix" value="/WEB-INF/views/" />
  		<beans:property name="suffix" value=".jsp" />
  	</beans:bean>
  ```



### 4. InternalResourceView

![image-20230607173140350](/images/2023-06-07-redirectforward/image-20230607173140350.png)

### 5. forward의 예시

![image-20230607173328624](/images/2023-06-07-redirectforward/image-20230607173328624.png)



