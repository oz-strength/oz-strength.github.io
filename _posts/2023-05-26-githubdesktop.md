---
layout: single
title: "이클립스와 깃헙 연동"
categories: blog
tag: [github, spring, eclipse]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# eclipse와 GitHub연동하기

### _1. 프로젝트를 생성할때 저장할 폴더를 지정한다._

ex) 프로젝트 생성 시 바탕화면에 springTest라는 폴더를 생성

### _2. 해당 폴더를 github desktop에 드래그앤 드롭한다._

- 드래그앤 드롭하면 로컬 레파지토리를 생성하고
- github에 레파지토리를 생성한다.

### 3. 이클립스로 와서 프로젝트를 로컬 리파지토리로 연동한다.

- window - perspective - open perspective 에서 git으로 연다.
- 로컬 리파지토리를 추가하는 항목을 클릭후 해당 경로를 찾아 연결해준다.

### 4. 연동이 되면 해당 폴더에 .gitignore파일이 생성된다.

- https://www.toptal.com/developers/gitignore 사이트로 가서 eclipse, java, window 등의 키워드로 git에 올릴필요 없는 항목들을 긁어와서 복붙한다.

