---
title: Node.js Study 2
date: 2019-01-04 00:00:01
categories: Web
tags:
  - Node.js
  - JavaScript
header:
  teaser: /assets/images/nodejs_logo.png
excerpt: Synchronous, Asynchronous, Callback 함수 ...
---
> Node.js - sync, async, and callback

![sync & async](/assets/images/sync&async.PNG){: .align-center}

## Synchronous
동기적 방식은 함수들의 실행이 항상 순서대로 진행됩니다.<br>
A,B,C,D 각각 4개의 함수가 A-B-C-D 순서로 실행이 된다고 할 때, <br>
B는 A 종료 후 <br>
C는 B 종료 후 <br>
D는 C 가 종료된 이후에 실행이 됩니다.<br>
<br>
예를 들어,<br>
`sync.js`
```js
function A(){
  console.log('A');
}
function B(){
  console.log('B');
}
function C(){
  console.log('C');
}
function D(){
  console.log('D');
}
A(); B(); C(); D();
```
이 프로그램의 결과는
```bash
$ node sync.js
A
B
C
D
```
이렇게 나오게 됩니다.<br>
<br>
## Asynchronous
하지만 비동기적 방식은 함수의 실행 순서가 예상치 못한 방향으로 나오게 되는데요, 바로 **_callback_** 때문입니다.<br>
**_callback_** 이란, 어느 함수가 실행되고 작업을 마치면 뒤따라 실행되는 함수를 말합니다.<br>
비동기적 방식에서는 이 **_callback_** 함수를 통해 모든 작업(함수)을 순서대로 실행할 필요없이 각각 작업(함수)마다 비동기적으로 실행하게 됩니다. 말로는 어려운데, 예를 들어볼게요.<br>
`async.js`
```js
var fs = require('fs');
console.log('A');
fs.readFile('', function(err,data){
  console.log('B');
});
console.log('C');
```
이 프로그램의 결과는
```bash
$ node async.js
A
C
B
```
이렇게 됩니다. `async.js` 코드의 순서상으론 A B C 가 찍혀야 맞지만, *fs.readFile* 은 파일을 읽어들이는 데에 시간이 걸리고 이 작업이 끝난 후에 **_callback_** 함수를 실행합니다.<br>
`console.log('C')` 이 코드는 *fs.readFile* 이 작업이 끝나기까지 기다리지 않고 비동기적으로 먼저 실행되는겁니다.<br>
처음엔 익숙치 않지만, Node.js 를 효율적으로 사용하기 위해서는 이 **_비동기적 방식_** 이 필수적이라고 하니 익숙해지도록 신경써야겠습니다.<br>
<br><br>
<hr>
> Node.js 서버 소스 코드

`main.js`
```js
var http = require('http');
var fs = require('fs');
var url = require('url');

function templateHTML(title, list, description){
  var template;
  template = `
    <!doctype html>
    <html>
      <head>
        <title>WEB1 - ${title}</title>
        <meta charset="utf-8">
      </head>
      <body>
        <h1><a href="/">WEB</a></h1>
        ${list}
        <h2>${title}</h2>
        <p>${description}</p>
      </body>
    </html>
    `;
  return template;
}

function templateList(filelist){
  var list = '<ul>';
  for(var i=0; i<filelist.length; i++){
    list = list+`<li><a href=/?id=${filelist[i]}>${filelist[i]}</a></li>`;
  }
  list = list+'</ul>';
  return list;
}

// function parseId(id){
//   if(id === undefined){
//     fs.readdir('./data', function(err,filelist){
//       //console.log(filelist);
//       var title = 'Welcome';
//       var list = templateList(filelist);
//       var description = 'Hello, Node.js';
//       var template = templateHTML(title, list, description);
//       //console.log(template);
//       return template;
//     })
//   }else{
//     fs.readFile(`data/${id}`, 'utf8', function(err, description){
//       fs.readdir('./data', function(err,filelist){
//         var title = id;
//         var list = templateList(filelist);
//         var template = templateHTML(title, list, description);
//         console.log(template);
//         return template;
//       });
//     });
//   }
// }

var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;

    if(url.parse(_url, true).pathname === '/'){
      var id = queryData.id
      if(id === undefined){
        fs.readdir('./data', function(err,filelist){

          var title = 'Welcome';
          var list = templateList(filelist);
          var description = 'Hello, Node.js';
          var template = templateHTML(title, list, description);

          response.writeHead(200);
          response.end(template);
        })
      }else{
        fs.readFile(`data/${id}`, 'utf8', function(err, description){
          fs.readdir('./data', function(err,filelist){
            var title = id;
            var list = templateList(filelist);
            var template = templateHTML(title, list, description);

            response.writeHead(200);
            response.end(template);
          });
        });
      }
    }else{
      response.writeHead(200);
      response.end('Not Found');
    }
});
app.listen(4000);

```
#### 코드작성이 중간에 스킵된 부분이 있는데, 이는 [생활코딩](https://opentutorials.org/course/3332) 강의를 참고하면 될것 같습니다.
- 기존 var app 에 있는 중복된 작업들을 templateHTML, templateList 함수로 묶어 간소화하였습니다.<br>
- queryData.id 가 값이 있느냐 없느냐에 따라 변하는 부분을 parseId 함수로 간소화 하려했으나 비동기적 방식으로 인한 문제가 생겨 제대로 실행되지 않았습니다. 이 부분은 주석처리.
