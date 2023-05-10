---
layout: single
title: "Client & Server"
categories: coding
tag: [python, blog, spring]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---



# 클라이언트와 서버

### 1. 클라이언트와 서버

클라이언트(client) : 서비스를 요청하는 애플리케이션(or 컴퓨터)

서버(server) : 서비스를 제공하는 애플리케이션(or 컴퓨)

*클라이언트와 서버는 역할로써 구분한다*

### 2. 서버의 종류

어떤 서비스를 제공하는지로 나뉜다.

- 이메일을 서비스 → 이메일 서버
- 파일을 서비스 → 파일 서버
- 웹을 서비스 → 웹 서버

### 3. 서버의 포트

하나의 아이피 주소에 여러개의 서버가 있다면 클라이언트는 아이피 주소만으로 구분할 수 없다. 

그래서 포트가 연결되어서 각각의 서버를 구분지어준다.

이메일 서버 → 25

파일 서버 → 22

웹 서버 → 80

아이피가 111.22.33.44 라고 했을 때 

이메일 서버에 접속하려면 111.22.33.44:<span style="color:blue">25</span>로 입력한다.

기본값으로 예시를 든 것이고, 다른 포트번호를 사용할 수 있다. 

### 4. 웹 애플리케이션 서버(WAS)란?

웹 애플리케이션 서버(WAS) : 웹 애플리케이션을 서비스하는 서버

애플리케이션 : 프로그램 

> <span style="background-color:pink">*애플리케이션을 서비스한다* </span>의 의미?
>
> 클라이언트가 서버에 있는 애플리케이션(프로그램)을 사용할 수 있게 해준다

톰캣이라는 WAS가 서버에 설치되고 실행중이면 클라이언트가 서버에게 원격 프로그램을 호출할 수 있다. 

+ 스프링부트에서는 내장된 톰캣을 사용한다. 

###   5. HTTP요청부터 응답까지의 과정

웹서버에 요청하기 전에 IP주소와 포트번호로 TCP/IP연결을 맺어야 한다. 

1. DNS 서버에서 도메인의 IP를 조회한다. 

   클라이언트가 URL로 도메인이름을 요청하면 DNS(Domain Name System) 서버가 ip주소로 변환한다.

   해당 ip주소가 있는 웹서버로 요청한다. (SYN)

   서버가 응답한다.(ACK) 

2. 웹브라우저와 웹서버간의 TCP 연결을 맺는다.(3-way handshake)

   클라이언트와 서버간의 stream(I/O스트림)이 만들어진다. 

   클라이언트가 출력을 하면(OUT) 웹서버에 입력(IN)으로 들어간다.

   서버가 클라이언트에게 연결하자는 요청을 하고(SYN)

   클라이언트가 응답한다.(ACK)

   서버(OUT) - 클라이언트(IN) 의 I/O스트림이 생성된다.

   *3-way handshake?*

   *서버가 보내는작업을 (ACK,SYN)을 한번에 처리해서 총3 번의 통신으로 *

   *클라이언트와 서버간의 연결이 이루어진다.*

3. 웹브라우저가 웹서버에 HTTP 요청을 보낸다.

   연결이 이루어지면 클라이언트가 서버에 요청을 할 수 있다.  

4. 웹서버는 웹브라우저에게 HTTP 응답을 보낸다.

   서버가 요청을 받아서 응답을 한다.

   대부분 HTML형식의 결과를 보낸다.

5. TCP 연결을 끊고, 웹브라우저는 HTTP 응답(주로 HTML)을 보여준다. 

   요청과 응답이 끝나면 TCP 연결이 끊어진다.

   재사용 하는 방법도 있다. 

### 6. IP, TCP, HTTP

IP(<span style="color:red">Internet</span>Protocol) : IP주소를 이용한 전송 프로토콜 . 비연결 기반

TCP(<span style="color:red">Transfer Control</span> Protocol) : 패킷 <span style="color:red">전송을 제어</span>. 연결 기반

HTTP(Hyper <span style="color:red">Text</span> Transfer Protocol) : <span style="color:red">텍스트</span> 프로토콜  



