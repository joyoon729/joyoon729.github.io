---
title: Node.js Study 1
date: 2019-01-04 00:00:00
categories: Web
tags:
  - Node.js
  - JavaScript
header:
  teaser: /assets/images/nodejs_logo.png
excerpt: 한번쯤 나만의 '서버'를 만들어 보고 싶단 생각을 갖고 있었는데, 늘 그에 대한 공부는 뒷전이었습니다.<br> 미루고 미뤘던 Node.js 공부. 이제 시작해봅니다.<br>
---
> Node.js 공부 시작

한번쯤 나만의 '서버'를 만들어 보고 싶단 생각을 갖고 있었는데, 늘 그에 대한 공부는 뒷전이었습니다.<br>
미루고 미뤘던 Node.js 공부. 이제 시작해봅니다.<br>
<br>
공부는 웹에 관해 공부를 시작하려는 사람들을 위한 아주 유용한 강의 사이트가 있는데, 바로 [생활코딩](https://opentutorials.org/course/1) 입니다.<br>
저는 이곳의 Node.js 관련 강의를 듣고 정리해두려 하는데, 수업 내용이 굉장히 초심자를 위한 내용들로 이루어져 있어서 블로그에는 수업 모든 내용을 정리한다기보다 개인적으로 중요하다고 생각되는 부분만 찝어서 정리해두려 합니다.<br>
<br><br>
<hr>
> JavaScript 배경 지식

Node.js 는 JavaScript 로 이루어져있기 때문에 Node.js 스터디 관련 포스트 중간중간 필요한 JavaScript 관련 기초 지식도 함께 작성해두겠습니다.
{: .text-center}

## Template literal
`> template_literal.js`
```js
var name = 'Jo Yoon';
var sentence = 'Hi My name is ' + name + '.';
var sentence2 = `Hi My name is ${name}.`; // Template Literal
console.log(sentence);
console.log(sentence2);
```
`> 결과창`
```bash
$ node template_literal.js
Hi My name is Jo Yoon.
Hi My name is Jo Yoon.
```
sentence 와 sentence2 를 콘솔창에 찍어보면 동일합니다.<br>
표현하는 방식이 다를 뿐인데, 문자열과 변수를 동시에 사용하려할 때 sentence 의 방식보다 sentence2 처럼 **template literal** 을 사용하면 문자열을 더 편리하게 다룰 수 있습니다.<br>
변수는 `${변수명}` 으로 사용합니다.<br>
<br><br>
## Comparison Operator '==' vs '==='
`> comparison.js`
```js
a = '2';
b = 2;
console.log(a==b);
console.log(a===b);
```
`> 결과창`
```bash
$ node comparison.js
true
false
```
JavaScript 에서 비교연산자 `==` 와 `===` 는 서로 다릅니다.<br>
`==` 연산자는 좌우항의 '값'만 비교하는 한편, `===` 연산자는 좌우항의 '값'과 '타입' 까지 **엄격히** 비교합니다.<br>
그래서 이 강의의 강의자께선 두 값을 비교하려는 경우엔 왠만하면 `===` 연산자를 사용하라고 하시네요.<br>
<br><br>
<hr>
> Node.js 설치

[https://nodejs.org/](https://nodejs.org/) 에 들어가 *LTS* 버전을 다운받아 설치하면 됩니다.<br>
![nodejs.org](/assets/images/nodejs_org.PNG){: .align-center}
<br>
- 윈도우키(<i class="fab fa-windows"></i>) + R 을 눌러 실행창을 띄우고 `cmd` 입력후 확인 눌러 명령 프롬프트를 실행시킵니다.
- `node -v` 를 입력해 node.js 의 버전이 뜬다면 설치 끝.

![](/assets/images/nodejs_prompt.PNG)
<br>
<br><br>
<hr>
> Node.js 모듈

node.js 에 유용한 api 들을 '모듈' 이라고 부른다고 합니다.<br>
[Node.js Documentation](https://nodejs.org/dist/latest-v10.x/docs/api/) 이곳에서 다양한 api 들을 찾아보실 수 있습니다.<br>
사용법은 간단합니다.
```js
var fs = require('fs');
fs.readFile('sample.txt', 'utf8', function(err,data){});
```
이와 같은 방식으로 원하는 모듈을 불러와 변수에 선언 후 사용하시면 됩니다.<br>
<br><br>
<hr>
> Node.js 를 이용해 기초적인 로컬호스트용 서버 만들기

`> main.js`
```js
var http = require('http');

var app = http.createServer(function(request,response){

    var template = `
        <!doctype html>
        <html>
          <head>
            <title>node.js study</title>
            <meta charset="utf-8">
          </head>
          <body>
            <h1>>Node.js Study</h1>
            <p>Node.js 시작합니다.</p>
          </body>
        </html>
        `;
        response.writeHead(200);
        response.end(template);
});
app.listen(4000);
```
강의자께서 **_아직은 몰라도 된다_** 고 하는 `require('http')`, `createServer(...)` 등이 보입니다.<br>
하지만 하나씩 뜯어보면 각각 무슨 역할을 하는지 짐작은 할 수 있을 것 같습니다.<br>
- http 모듈 가져오기
- 가져온 http 모듈로 'createServer'를 실행하기
- response.writeHead(200) -> 헤드에 '200' 코드 작성해 보내기
- response.end(template) -> template 변수 보여주기
- 그리고 이 서버는 port number 4000 에 활성화되어 있음

강의자께서 `200` 이라는 코드는 브라우저가 원하는 문서를 성공적으로 응답했을때 같이 보내주는 '알림' 정도라고 말씀하셨고, 서버가 브라우저가 원하는 문서를 찾지 못했을때에는 `404` 라는 코드로 응답한다고 하네요.<br>
아래는 `$node main.js` 를 실행시켰을때 브라우저로 확인해본 것입니다.<br><br>
`> http://localhost:4000/`<br>
![](/assets/images/nodejs_start_browser.PNG)
<br><br>
