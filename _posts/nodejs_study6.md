---
title: Node.js Study 6
date: 2019-01-15 00:00:00
categories: Web
tags:
  - Node.js
  - JavaScript
header:
  teaser: /assets/images/nodejs_logo.png
excerpt: Node.js 기초 스터디 최종. 사용자로부터 서버로 어떠한 입력을 받을때에 발생할 수 있는 보안 위험을 방지하는 법을 알아보겠습니다.
---
> 목표

사용자로부터 서버로 어떠한 입력을 받을때에 발생할 수 있는 보안 위험을 방지하는 법을 알아보겠습니다. <br>
<br>
<br>
<hr>
> 상황 가정

입력에 의한 보안 위협이 어떤식으로 발생하는지 알아보기 위해 특정 상황을 가정해 설명해보겠습니다.<br>
<br>
**_어떤 웹 애플리케이션_** 이 있습니다.<br>
사용자로부터 파일 제목을 입력받으면 정해진 디렉토리 안에 있는 파일의 내용을 표시해주는 **_어떤 웹 애플리케이션_**.
<br>
<br>
![directory_tree](/assets/images/dir_tree.PNG)
<br>
이런 디렉토리 트리를 가지고 있구요.<br>
security, data 디렉토리를 갖고있고, main.js 로 실행합니다.<br>

이 **_어떤 웹 애플리케이션_** 은 사용자로부터 파일 제목을 입력받으면 <br>
`data/` 디렉토리 안에 있는 파일의 내용을 표시해줍니다.<br>
<br>
그런데 이 **_어떤 웹 애플리케이션_** 은 사용자 정보를 담은 중요한 파일도 갖고 있습니다.<br>
`security/users.txt`
```bash
--- 사용자들의 ID/PW 가 담긴 파일 ---
Joyoon : 1q2w3e4r
Nodejs : passssword12
Javascript : hellojs!@
```
이렇게 `security/` 디렉토리에 `users.txt` 파일의 형태로 사용자들의 ID/PW 가 저장되어 있다고 해봅시다.<br>
<br>
<br>
<br>
![enter title](/assets/images/enter_title.PNG)
###### (여기에 제목을 입력하면 내용을 불러와줍니다...)
<br>
<br>
그런데 만약 여기서 악의적 사용자가 정상적인 제목을 입력하지 않고,<br>
`../security/users.txt` <br>
라고 입력하게 되면 어떻게 될까요?<br>

`..` 는 상위 디렉토리를 의미하기 때문에 `data/../security/users.txt` 로 users.txt 파일에 바로 접근할 수 있게됩니다.<br>
<br>
![위험..](/assets/images/security_users.gif)
###### (관리자가 원치 않는 데이터도 노출 될 수 있습니다.)
<br>
<br>
<br>
<hr>
> 해결 방법

이러한 보안 취약점은 사용자로부터 받는 입력 코드를 필터링함으로써 해결 할 수 있습니다.<br>
Node.js 가 제공하는 모듈 중, `path` 모듈을 이용해 입력 코드를 필터링 해 보겠습니다. <br>

`dirty.js`
```js
var path = require('path');

var dirty_input = "../security/users.txt";
console.log(path.parse(dirty_input));

var clean_input = path.parse(dirty_input).base;
console.log(clean_input);
```
<br>
`결과창`
```bash
$ node dirty.js
{ root: '',
  dir: '../security',
  base: 'users.txt',
  ext: '.txt',
  name: 'users' }
users.txt
```
<br>
<br>
이렇게 path 모듈의 parse 메소드를 이용하면 <br>
root, dir, base, ext, name 으로 입력값을 잘라서 다룰 수 있습니다.<br>
이를 이용해 오염된 input(dirty_input) 을 필터링해 악의적인 입력 코드를 걸러낼 수 있게 됩니다.<br>
<br>
<br>
<hr>
> Node.js 기초 공부 끝

이번 포스트를 끝으로 Node.js 기초편은 끝이 났습니다.<br>
앞선 포스트들은 [오픈튜토리얼스(https://opentutorials.org)](https://opentutorials.org/) 의 **생활코딩 - Node.js** 강의를 통해 공부한 내용을 정리하기 위해 작성되었습니다.<br>
<br>
