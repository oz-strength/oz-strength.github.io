---
layout: single
title: "타입스크립트 기본"
categories: frontend
tag: [typescript, basic]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---



# TypeScript 기본문법

```tsx
export default function TypescriptPage() {
  // 타입 추론
  let aaa = '안녕하세요'
  aaa = 3 // 에러

  // 타입 명시
  let bbb: string = '반갑습니다'
  bbb = 10 // 에러

  // 타입 명시가 필요한 상황
  let ccc: number | string = 1000
  ccc = '1000원'

  // 숫자타입
  let ddd: number = 10
  ddd = '철수' // 에러

  // 불린타입
  let eee: boolean = true
  eee = false
  eee = 'false' // 에러 true로 작동함

  // 배열타입
  let fff: number[] = [1, 2, 3, 4, 5, 'hi'] // 에러 'hi'
  let ggg: string[] = ['철수', '영희', '훈이', 10] // 에러 10
  let hhh: (string | number)[] = ['철수', '영희', '훈이', 10] // 타입을 추론해서 어떤 타입을 사용하는지 알아보기!!

  // 객체타입
  interface IProfile {
    name: string
    age: number | string
    school: string
    hobby?: string
  }

  const profile: IProfile = {
    name: '철수',
    age: 8,
    school: '다람쥐초등학교',
  }
  profile.name = '훈이' // 타입추론으로 이것만 가능
  profile.age = '8살'
  profile.hobby = '수영'

  // 함수타입
  function add(num1: number, num2: number, unit: string): string {
    return num1 + num2 + unit
  }
  const result = add(1000, 2000, '원') // 결과의 리턴 타입도 예측 가능!!!

  const add2 = (num1: number, num2: number, unit: string): string => {
    return num1 + num2 + unit
  }
  const result2 = add2(1000, 2000, '원') // 결과의 리턴 타입도 예측 가능!!!

  // any타입
  let qqq: any = '철수' // 자바스크립트와 동일!
  qqq = 123
  qqq = true
  return <></>
}

```

