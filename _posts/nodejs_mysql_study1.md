---
title: Node.js & MySQL Study 1
date: 2019-01-31 00:00:00
categories: Web
tags:
  - Node.js
  - MySQL
header:
  teaser: /assets/images/nodejs_mysql.png
excerpt: 이전 공부에서 Node.js 를 이용해 글 작성 및 수정 삭제, 그리고 작성된 글을 HTML 형식으로 Generate 해주는 웹 어플리케이션을 만들어 보았고 MySQL 을 통해 관계형 데이터베이스의 기본적인 구조와 동작방법을 배웠습니다. 이제 이번 공부를 진행하면서 **file** 을 통해 글을 저장하는 것이 아니라, **Database** 를 이용해 글을 저장할 수 있을것이고 데이터베이스가 가진 기능들로 *저자* 나 *글쓴 날짜* 등도 표현할 수 있게 될 것입니다.
---

> Node.js & MySQL 연계하기

이전 공부에서 Node.js 를 이용해 글 작성 및 수정 삭제, 그리고 작성된 글을 HTML 형식으로 Generate 해주는 웹 어플리케이션을 만들어 보았고<br>
MySQL 을 통해 관계형 데이터베이스의 기본적인 구조와 동작방법을 배웠습니다.<br>
<br>
이제 이번 공부를 진행하면서 **file** 을 통해 글을 저장하는 것이 아니라, **Database** 를 이용해 글을 저장할 수 있을것이고<br>
데이터베이스가 가진 기능들로 ***저자*** 나 ***글쓴 날짜*** 등도 적은 비용으로 표현할 수 있게 될 것입니다.<br>
<br>
<br>
<hr>

> 소스코드 받기

본 포스트는 [Opentutorials: Nodejs-MySQL] 강의에서 제공하는 소스코드를 바탕으로 시작하였습니다.<br>
[여기](https://github.com/web-n/node.js-mysql/releases/tag/1)에서 소스코드를 받으실 수 있습니다.<br>
<br>
<br>
<hr>

> MySQL package 설치

Node.js 에는 기본 제공하는 MySQL 모듈이 없습니다.<br>
때문에 npm 을 이용해 MySQL 패키지를 따로 설치하였습니다.<br>
<br>
```bash
npm install --save mysql
```
npm install 을 이용하면 `package.json` 에 있는 의존성들(dependencies)을 자동으로 검사해 필요한 패키지들을 알아서 설치해줍니다.<br>
여기서 `--save` 옵션은 `package.json` 의 dependencies 에 mysql 을 추가해주는 옵션입니다.<br>
안정성을 위해 옵션을 추가해주는 것이 좋습니다.<br>
<br>
<br>
<hr>

> Node.js 에서 MySQL DB 에 접속하기

MySQL 패키지를 설치하였다면 이제 Node.js 로 MySQL DB 에 접근할 수 있습니다.<br>

### 사용법
`mysql.js`
```js
var mysql       = require('mysql');
var connection  =  mysql.createConnection({
    host    : 'localhost',
    user    : 'joyoon',
    password: '1234',
    database: 'opentutorials'
});

connection.connect();

connection.query('SELECT * FROM topic', function (error, results, fields){
    if (error) {
        console.log(error);
    }
    console.log(results);
});

connection.end();
```
알아둘 점은, *connection.query* 함수의 콜백함수 2번째 인자로 들어오는 **results** 에 MySQL DB 에서 가져온 정보가 배열의 형태로 넘어온다는 것입니다.<br>

`결과`
```bash
$ node mysql.js
[ RowDataPacket {
    id: 1,
    title: 'MySQL',
    description: 'MySQL is ...',
    created: 2019-01-30T05:28:51.000Z,
    author_id: 1 },
  RowDataPacket {
    id: 2,
    title: 'Oracle',
    description: 'Oracle is ...',
    created: 2019-01-30T05:32:28.000Z,
    author_id: 1 },
  RowDataPacket {
    id: 3,
    title: 'SQL Server',
    description: 'SQL Server is ...',
    created: 2019-01-30T05:39:34.000Z,
    author_id: 2 } ]
```
콘솔창에 표현되는 것으로 알 수 있듯이, DB의 정보가 배열 속 객체에 넘어옵니다.<br>
<br>
<br>
<hr>

> 추가1. Bitnami MySQL root 계정 비밀번호 변경법

저는 지금 Bitnami wamp stack 을 통해 MySQL 을 돌리고 있는데, root 계정의 비밀번호 변경하는 방법이 살짝 다릅니다.<br>
[docs.bitnami.com](https://docs.bitnami.com/aws/apps/processmakerenterprise/administration/change-reset-password/)을 참고하였습니다.<br>

```bash
/opt/bitnami/mysql/bin/mysqladmin -p -u root password NEW_PASSWORD
```
mysqladmin 을 실행하면 되는데, 경로는 bitnami 가 설치된 곳의 경로를 따르면 됩니다.<br>
<br>
<br>

> 추가2. MySQL 새 계정 생성하기

"Node.js & MySQL" 실습용으로 진행할 새 계정을 만들어 보겠습니다.<br>
### 1. 먼저 MySQL 에 접속합니다.
<br>
### 2. 계정을 생성합니다.

```bash
mysql> CREATE USER 'joyoon'@'%' IDENTIFIED BY '1234';
```
여기서 `'joyoon'@'%'` 의 %는 어디에서 접속하든 계정명이 joyoon 이면 해당된다는 뜻입니다.<br>
<br>
### 3. 생성한 계정에 권한을 부여합니다.

```bash
mysql> GRANT ALL PRIVILEGES ON opentutorials.* TO 'joyoon'@'%';
```
`GRANT ALL PRIVILGES` 는, 모든 권한을 부여하겠다는 뜻이고,<br>
`ON opentutorials.*` 는, opentutorials 라는 DB에 있는 모든(*) Table 에 관한 권한을 의미합니다.<br>
<br>
### 4. 변경점을 활성화합니다.
```bash
mysql> FLUSH PRIVILEGES;
```