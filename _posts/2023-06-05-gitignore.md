---
layout: single
title: "git ignore 적용안됨"
categories: spring
tag: [java, git, eclipse]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# 로컬 레파지토리 생성 후 git ignore 적용이 안될 때  

<span style="background-color:yellow">git rm -r --cached .</span> 라는 명령어를 입력 후 git 의 캐시를 삭제하고 커밋한다. 

```plain text
git rm -r --cached .
git add .
git commit -m "clear git cache"
git push
```

<span style="background-color:yellow">git add .</span> 명령어 뒤에 <span style="background-color:yellow">git status</span> 로 한번 확인 후 push



[참조]

https://computer-science-student.tistory.com/470





