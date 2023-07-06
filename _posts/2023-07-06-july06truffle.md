---
layout: single
title: "Truffle"
categories: ai
tag: [vscode, solidity, blockchain]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---

# Truffle



**vscode**



```npm install -g truffle```

```truffle init```



contracts : 컨트랙트 소스

migration : 배포를 위한 스크립트

test : 테스트용

config.js : 설정파일



solidity 익스텐션 설치 



### contracts/OzContract.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract OzContract {
  struct Student {
    bytes32 name; // string 대신에 bytes32
    bytes32 gender;
    uint age;
  }

  mapping(uint => Student) studentInfo;

  function setStudentInfo(uint _studentId, bytes32 _name, bytes32 _gender, uint _age) public {
    Student storage student = studentInfo[_studentId];
    student.name = _name;
    student.gender = _gender;
    student.age = _age;
    
  }
  function getStudentInfo(uint _studentId) public view 
  returns(bytes32, bytes32, uint) {
    return(studentInfo[_studentId].name,
          studentInfo[_studentId].gender,
          studentInfo[_studentId].age);
  }
}
```



### migrations/deploy_contract.js

```js
const OzContract = artifacts.require('./OzContract.sol');

module.exports = function (deployer) {
  deployer.deploy(OzContract);
};

```



> 터미널 ```truffle develop```

![image-20230706103806371](/images/2023-07-06-july06truffle/image-20230706103806371.png)



> 배포 ```migrate``` 