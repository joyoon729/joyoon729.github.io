---
title: Node.js Study 3
date: 2019-01-05 00:00:00
categories: Web
tags:
  - Node.js
  - JavaScript
  - HTML
header:
  teaser: /assets/images/nodejs_logo.png
excerpt: form 태그, get방식 & post방식, 새 글 작성하기, 리다이렉션 ...
---
> 목표

사용자가 서버 내의 `_data` 디렉토리에 새 글을 생성할 수 있고<br>
서버는 생성된 새 글을 페이지에 반영하도록 합니다.<br>
<br><br>
<hr>
> 글생성 UI 만들기

사용자가 글을 써 넣을 공간을 만듭니다.
**form** 태그를 이용합니다.
```html
<form action="http://localhost:4000/create_process" method="post">
  <p><input type="text" name="title" placeholder="제목을 입력하세요"></p>
  <p><textarea name="본문을 입력하세요"></textarea></p>
  <input type="submit">
</form>
```
**form** 의 action 속성은 입력된 데이터를 action 속성의 주소로 보내는 역할을 하고, `method="post"` 는 post 방식으로 url에 노출없이 데이터를 보내겠다는 뜻입니다.<br>
post의 반대되는 방식으론 get 방식이 있는데 따로 속성을 지정해주지 않으면 기본값으로 get 방식을 따릅니다.<br>

<i style="font-size:1.5em;" class="fas fa-exclamation-triangle"></i> 서버를 향하는 정보가 url 에 노출되면 안되기 때문에 일반적으로 서버에 정보를 줄땐 post 방식을 사용합니다.
{: .notice--danger}
`> 완성된 UI`<br>
![html-form](/assets/images/html_form.PNG)<br>
<br><br>
<hr>
> POST 방식으로 전송된 데이터 받기

전송된 데이터는 현재 `http://localhost:4000/create_process` 으로 보내져 있습니다.<br>
이제 post 방식으로 묶인 이 데이터를 서버가 받아와야 합니다.<br>
```js
var body = '';

request.on('data', function(data){
  body += data;
});
request.on('end', function(){
  var post = qs.parse(body);
  console.log(post);
  var title = post.title;
  var description = post.description;
});
```
### request.on('data',function)
이 함수는 post 에 담긴 data를 조각내 일부분씩 담아옵니다.<br>
### request.on('end' function)
이 함수는 post 에 담긴 data를 모두 가져왔을때 실행되며, 위 코드에서는 `post.title`, `post.description` 으로 post로부터 데이터를 변수로 가져옵니다.<br>
<br><br>
![html_form_test](/assets/images/html_form_test.PNG)<br>

`> console.log(post) 로 찍어본 화면`

```bash
{title: JoYoon, description: Node.JS}
```
<br><br>
<hr>

> 내 서버 디렉토리에 새 글 작성시키기

node.js 를 이용해 서버 디렉토리에 파일을 생성하려면 `fs.writeFile`을 사용하면 됩니다.<br>
```js
fs = require('fs');
fs.writeFile('filename', 'data', 'utf8', callback);
```
<br><br>
<hr>
> 새로 작성된 글로 리다이렉션하기

`> main.js`
```js
request.on('end', function(){
  var post = qs.parse(body);
  console.log(post);
  var title = post.title;
  var description = post.description;
  fs.writeFile(`data/${title}`, description, `utf8`, function(err){
    response.writeHead(302, {Location: `/?id=${title}`});
    response.end();
  })
});
```
리다이렉션(redirection)은 어떤 특정페이지로 도달한 사용자를 다른 페이지로 돌려보내는 것을 의미합니다. 여기서는 `/?id=${title}`을 통해 작성한 글 페이지로 보냅니다.<br>

<i style="font-size:1.5em;" class="fas fa-lightbulb"></i> Head에 보내는 코드 `'302'`는 다음 페이지로 리다이렉션 하라는 의미이고
코드 `'301'`은 이 페이지는 다음 페이지로 영구히 바뀌었다는 것을 의미합니다.
{: .notice--info}
<br><br>
<hr>
> 전체 소스코드

`> main.js`
```js
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');

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
        <a href="/create">create</a>
        <h2>${title}</h2>
        <p>${description}</p>
      </body>
    </html>
    `;
  //console.log(template);
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
    var pathname = url.parse(_url, true).pathname;
    if(pathname === '/'){
      var id = queryData.id
      if(id === undefined){
        fs.readdir('./data', function(err,filelist){

          var title = 'Welcome';
          var list = templateList(filelist);
          var description = 'Hello, Node.js';
          var template = templateHTML(title, list, description);

          response.writeHead(200);
          response.end(template);
        });
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
    }else if(pathname === '/create'){
      fs.readdir('./data', function(err,filelist){

        var title = 'WEB - create';
        var list = templateList(filelist);
        var description = `
          <form action="http://localhost:4000/create_process" method="post">
            <p>
              <input type="text" name="title" placeholder="제목을 입력하세요">
            </p>
            <p>
              <textarea name="description" placeholder="본문을 입력하세요"></textarea>
            </p>
              <input type="submit">
          </form>
        `;
        var template = templateHTML(title, list, description);

        response.writeHead(200);
        response.end(template);
      });
    }else if(pathname === '/create_process'){
      var body = '';

      request.on('data', function(data){
        body += data;
      });
      request.on('end', function(){
        var post = qs.parse(body);
        console.log(post);
        var title = post.title;
        var description = post.description;
        fs.writeFile(`data/${title}`, description, `utf8`, function(err){
          response.writeHead(302, {Location: `/?id=${title}`});
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
