---
title: Node.js Study 5
date: 2019-01-14 00:00:00
categories: Web
tags:
  - Node.js
  - JavaScript
header:
  teaser: /assets/images/nodejs_logo.png
excerpt: 강의 내용을 바탕으로 자바스크립트 객체에 기본적인 개념을 알고 이를 이용해 `main.js` 의 코드를 **리팩토링(refactoring)** 해보겠습니다...
---
> 목표

강의 내용을 바탕으로 자바스크립트 객체에 기본적인 개념을 알고<br>
이를 이용해 `main.js` 의 코드를 **리팩토링(refactoring)** 해보겠습니다.<br>
<br>
<br>
<hr>

> JavaScript 의 객체

JavaScript 에서는 **객체** 를 사용할 수 있는데, 일단 배열과 유사하다고 생각하면 쉽습니다.<br>

`array.js`
```js
var array = ['item1', 'item2', 'item3'];
for(var i in array){
  console.log(array[i]);
}
```
`결과창`
```bash
$ node array.js
item1
item2
item3
```
<br>
이런식으로 배열에는 index 가 있어 번호로 접근 할 수 있는데,<br>
**객체** 는 property 와 method 로 데이터를 저장 및 데이터에 접근 하는 방법을 기술하고 있고 <br>
이들은 번호 대신 '이름' 의 형태로 객채 내 데이터에 접근할 수 있습니다.<br>

`object.js`
```js
var object = {
  name: "Joyoon",
  getName: function(){ console.log(this.name);} // this 키워드는 다른 언어들과 동일하게 자신의 멤버변수들을 가리킵니다.
}
object.getName();
```
`결과창`
```bash
$ node object.js
Joyoon
```
<br>
<br>
<hr>
> 객체 모듈(module)화 하기

객체들에 여러 정보들을 담아 정리했다면, <br>
이제 이 객체들을 따로 하나의 파일 형태로 코드 밖으로 빼내어 관리할 수 있습니다.<br>
하나의 파일에 모든 코드들이 담기는것 보다 훨씬 관리하기 편하고 가독성도 높일 수 있겠죠.<br>
모듈을 공부할 때 꼭 기억해야 하는것은 **exports** 와 **require** 라고 생각합니다.<br>
<br>
`my_module.js`
```js
/* module.exports 를 통해 이 파일이 다른 파일에서 require 될 수 있도록 합니다. */
module.exports = {
  name: "Joyoon",
  like: "nodejs",
  sayName: function(){
    console.log(this.name);
  },
  saySomething: function(something){
    console.log(something);
  }
}
```
`main.js`
```js
/* require('모듈파일이 저장된 경로') 를 통해 모듈을 불러와 사용할 수 있습니다. */
var jy = require('./my_module.js');
console.log(jy.like);
jy.sayName();
jy.saySomething("Hello World!");
```
`결과창`
```bash
$ node main.js
nodejs
Joyoon
Hello World!
```
<br>
<br>
<hr>
> 리팩토링 (Refactoring)

**리팩토링(refactoring)** 이란,<br>
쉽게말해 코드를 단순화하고 가독성을 높히는 작업을 말합니다. <br>
유지보수를 편리하게 하는 이득을 취할 수 있습니다.<br>
하지만 프로그램에 새로운 기능을 추가하거나 결과가 변하지는 않습니다.<br>
말그대로 코드를 _재조정_ 하는 것입니다.<br>
<br>
예를들어,<br>
`complicated.js`
```js
console.log("1");
console.log("2");
console.log("3");
/* something to do 1...*/
console.log("1");
console.log("2");
console.log("3");
/* something to do 2...*/
console.log("1");
console.log("2");
console.log("3");
```
이와 같은 코드가 있다고 할 때<br>
중간에 'something to do (1,2)...' 무언가 다른 코드가 있다하더라도<br>
1,2,3 을 콘솔창에 찍어주는 코드는 변함없이 반복되고 있음을 알 수 있습니다. <br>
이렇게 중복되고 반복되는 코드들이 있다면 가독성이 떨어지고<br>
해당 코드들에 의미를 부여하기 힘들고<br>
차후에 코드 수정을 해야할 때 반복되는 모든 코드들을 일일이 수정해야 할 것입니다.<br>
<br>
이런 코드를 refactoring 한다면<br>
`simplified.js`
```js
function print123(){
  console.log("1");
  console.log("2");
  console.log("3");
}
print123();
/* something to do 1...*/
print123();
/* something to do 2...*/
print123();
```
이렇게 될 수 있겠습니다.<br>
이 코드는 이제 반복되는 라인들을 줄여 가독성을 높이고<br>
명시해둔 함수명을 통해 '어떤 작업'을 하는 지 의미부여를 할 수 있게 되었습니다. <br>
또한 차후 코드를 수정해야할 때, print123 함수만 수정해 전체 코드의 내용을 수정할 수 있는 효과를 가져와 유지보수에 드는 비용을 줄일 수 있습니다.<br>
<br>
<br>
<hr>
> 모듈을 이용해 main.js 리팩토링 하기

기존 `main.js` 에서는 templateHTML 과 templateList 함수가 각 페이지마다 리스트와 본문을 생성하는 데 반복해서 쓰였었죠. <br>
이제 저는 이렇게 반복해서 쓰이는 두 함수를 하나의 객체에 넣고, <br>
이 객체를 모듈화 해 하나의 프로그램으로 관리할 수 있도록 리팩토링 하려합니다.<br>
<br>
`./lib/template.js`
```js
/* 기존 templateHTML 과 templateList 함수를 하나의 객체의 형태로 저장 */
module.exports = {
  HTML: function(title, list, description, control){
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
          ${control}
          <h2>${title}</h2>
          <p>${description}</p>
        </body>
      </html>
      `;
    return template;
  },
  list: function(filelist){
    var list = '<ul>';
    for(var i=0; i<filelist.length; i++){
      list = list+`<li><a href=/?id=${filelist[i]}>${filelist[i]}</a></li>`;
    }
    list = list+'</ul>';
    return list;
  }
}
```
**HTML** 메소드와 **list** 메소드로 함수 templateHTML/templateList 를 가져왔습니다.<br>
이제 이들은 `main.js` 에서 require(./lib/template.js) 로 불러와 접근자 **'.'** 으로 사용할 수 있습니다.<br>
<br>
기존 templateHTML/templateList 가 쓰였던 변수명만 바꿔준다면<br>
동일한 결과를 나타내지만 함수를 모듈화한 리팩토링이 끝이나게 됩니다.<br>
<br>
<br>
<hr>
> 전체 소스 코드

`./lib/template.js`
```js
module.exports = {
  HTML: function(title, list, description, control){
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
          ${control}
          <h2>${title}</h2>
          <p>${description}</p>
        </body>
      </html>
      `;
    return template;
  },
  list: function(filelist){
    var list = '<ul>';
    for(var i=0; i<filelist.length; i++){
      list = list+`<li><a href=/?id=${filelist[i]}>${filelist[i]}</a></li>`;
    }
    list = list+'</ul>';
    return list;
  }
}
```

`main.js`
```js
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');
var template = require('./lib/template.js')

var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    if(pathname === '/'){
      var id = queryData.id
      if(id === undefined){
        fs.readdir('./data', function(err,filelist){

          var title = 'Welcome';
          var list = template.list(filelist);
          var description = 'Hello, Node.js';
          var control = `
            <a href="/create">create</a>
          `;
          var html = template.HTML(title, list, description, control);

          response.writeHead(200);
          response.end(html);
        });
      }else{
        fs.readFile(`data/${id}`, 'utf8', function(err, description){
          fs.readdir('./data', function(err,filelist){
            var title = id;
            var list = template.list(filelist);
            var control = `
              <a href="/create">create</a>
              <a href="/update?id=${id}">update</a>
              <form action="/delete_process" method="post">
                <input type="hidden", name="id", value="${id}">
                <input type="submit", value="delete">
              </form>
            `
            var html = template.HTML(title, list, description, control);

            response.writeHead(200);
            response.end(html);
          });
        });
      }
    }else if(pathname === '/create'){
      fs.readdir('./data', function(err,filelist){

        var title = 'WEB - create';
        var list = template.list(filelist);
        var description = `
          <form action="/create_process" method="post">
            <p>
              <input type="text" name="title" placeholder="제목을 입력하세요">
            </p>
            <p>
              <textarea name="description" placeholder="본문을 입력하세요"></textarea>
            </p>
              <input type="submit" value="작성">
          </form>
        `;
        var control = '';
        var html = template.HTML(title, list, description, control);

        response.writeHead(200);
        response.end(html);
      });
    }else if(pathname === '/create_process'){
      var body = '';

      request.on('data', function(data){
        body += data;
      });
      request.on('end', function(){
        var post = qs.parse(body);

        var title = post.title;
        var description = post.description;
        console.log(filteredId);
        fs.writeFile(`data/${title}`, description, `utf8`, function(err){
          response.writeHead(302, {Location: `/?id=${title}`});
          response.end();
        })
      });
    }else if(pathname === '/update'){
      fs.readdir('./data', function(err,filelist){
        fs.readFile(`data/${queryData.id}`, `utf8`, function(err, description){
          var title = queryData.id;
          var list = template.list(filelist);
          var _description = `
            <form action="/update_process" method="post">
              <p><input type="hidden" name="id" value="${title}"></p>
              <p>
                <input type="text" name="title" value="${title}">
              </p>
              <p>
                <textarea name="description">${description}</textarea>
              </p>
                <input type="submit">
            </form>
          `;
          var control = '';
          var html = template.HTML(title, list, _description, control);
          response.writeHead(200);
          response.end(html);
        });
      });
    }else if(pathname === '/update_process'){
      var body = '';

      request.on('data', function(data){
        body += data;
      });
      request.on('end', function(){

        var post = qs.parse(body);
        var id = post.id;
        var title = post.title;
        var description = post.description;
        fs.rename(`data/${id}`, `data/${title}`, function(err){
          fs.writeFile(`data/${title}`, description, `utf8`, function(err){
            response.writeHead(302, {Location: `/?id=${title}`});
            response.end();
          });
        });
      });
    }else if(pathname === '/delete_process'){
      var body = '';

      request.on('data', function(data){
        body += data;
      });
      request.on('end', function(){
        var post = qs.parse(body);
        var id = post.id;
        fs.unlink(`data/${id}`, function(err){
          response.writeHead(302, {Location: `/`});
          response.end();
        })
      });
    }else{
      response.writeHead(404);
      response.end('Not Found');
    }
});
app.listen(4000);
```
