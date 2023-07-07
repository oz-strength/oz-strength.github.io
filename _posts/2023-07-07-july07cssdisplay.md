---
layout: single
title: "CSS - display"
categories: frontend
tag: [vscode, css]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---



# css - display

요소를 어떻게 배치할지, 자식 요소를 어떻게 배치할 지 

| 바깥쪽 | 안쪽      |
| ------ | --------- |
| block  | flex      |
| inline | grid      |
|        | table     |
|        | flow-root |
|        | flow      |
|        | ruby      |



### display - block / inline

| block                                   | inline                                                       |
| --------------------------------------- | ------------------------------------------------------------ |
| 한 줄을 차지                            | 컨텐츠 영역만큼만 차지                                       |
| width, height, margin, padding 지정가능 | width, height 지정 불가                                      |
| ```<div>, <h1~6>, <header>```           | margin 좌우에만 적용                                         |
|                                         | padding<br> 1. 시각적 영역: 상하좌우<br> 2. 실제 영역차지: 좌우 |
|                                         | ```<span>, <a>, <button> 등```                               |



### DOM(Document Object Methods)

- document
  - querySelector(cssSelectors): Element
  - getElementById(id): Element
  - addEventListener()

- element
  - setAttribute()
    - setAttribute(name, value)
    - attribute
      - id
      - class
      - disabled
      - ...



### Event

> 시스템에서 일어나는 사건 또는 발생

- 클릭
- 마우스 누를 때 
- 마우스 뗄 때
- 키보드 키 누를 때
- 키보드 키 뗄 때
- 창 크기 조정할 때
- form 제출할 때
- 비디오 재생할 때
- 멈출 때
- 끝날  

=> EventListener !!







