---
title: Node.js Study 4
date: 2019-01-07 00:00:00
categories: Web
tags:
  - Node.js
  - JavaScript
  - HTML
header:
  teaser: /assets/images/nodejs_logo.png
excerpt: 수정 및 삭제 구현 ...
---
> 목표

`_data` 디렉토리에 생성된 글을 수정 및 삭제할 수 있도록 합니다.<br>
<br><br>
<hr>
> 수정 및 삭제 링크 만들기

생성 링크를 만들어보았으니 수정 및 삭제 링크도 같은 방법으로 만들 수 있습니다.<br>
`control` 변수에 update 와 delete 링크를 추가하면 되는데, delete 링크의 경우에는 주의사항이 있으니 참고합니다.<br>
먼저 update 부터 시작해보겠습니다.<br>
`> main.js`
```js
var control = `
...
<a href="/update?id=${title}">update</a>
...
`;
```
`<a>` 태그를 이용해 update 링크를 만들고, 원하는 특정 글을 수정해야 하므로 queryString 에 제목 `${title}` 을 담아둡니다.<br>
이러면 update 링크는 완성입니다. 이 링크에서 수정작업을 할 수 있는 코드를 추가하면 됩니다.<br>
<br><br>
그런데 delete 는 이런 방식으로 삭제 링크를 만들면 안됩니다. update 링크와 마찬가지로 queryString 이 delete 에 적용된다면, 사용자가 **http://localhost:4000/delete?id=${제목}** 와 같이 url을 임의로 수정해 접속하는것만으로도 삭제 작업이 이루어질 수 있기 때문입니다.<br>
때문에 delete 는 queryString 이 url에 노출되는 get 방식이 아닌 post 방식으로 id 값을 보이지않게 서버로 건네주기 위해 `<form method="post">` 태그를 이용합니다.<br><br>
`> main.js`
```js
var control = `
...
<form action="/delete_process" method="post">
  <input type="hidden" name="id" value="${title}">
  <input type="submit" value="delete"
</form>
...
`;
```
이렇게 delete 버튼을 만들어 클릭하면 `/delete_process` 페이지로 이동하고, 이 페이지에서 삭제작업을 마치고 홈페이지 같은 곳으로 리다이렉션 해주는 방법을 쓰면 됩니다.<br>
<br>
<br>
<hr>
> 수정 & 삭제 구현

이제 버튼을 만들었으니 각 기능을 구현해봅니다.<br>
먼저 수정(update).<br>
`> main.js`
```js
...
}else if(pathname === '/update'){
  fs.readdir('./data', function(err,filelist){
    fs.readFile(`data/${queryData.id}`, `utf8`, function(err, description){
      var title = queryData.id;
      var list = templateList(filelist);
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
      var template = templateHTML(title, list, _description, control);
      response.writeHead(200);
      response.end(template);
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
...
```
실행 순서도는 <br>

*update 버튼 클릭 -> /update 페이지 수정입력 -> 제출 -> /update_process 페이지에서 _data 디렉토리 내 파일 변경 -> 변경된 제목의 /?id=${title} 페이지로 이동* <br>
{: .notice--info}
이렇게 됩니다.<br>
form 을 통해 넘어가는 값들은 3개. id(원래 제목), title(바뀐 제목), description(바뀐 본문). <br>
form 으로 넘어온 값들은 /update_process 에서 request.on('data', function()) 함수를 통해 변수 body에 저장됩니다.<br> request.on('end', function()) 함수에서 세 값들을 각 변수에 저장하고 fs.rename() 함수로 `_data` 디렉토리에 있는 원래 파일 이름을 변경, 그리고 변경한 파일명으로 fs.writeFile() 함수를 통해 새 제목, 새 본문으로 덮어씌우는 방식입니다.<br>
<br>
<br>
`> main.js`
```js
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
```
delete 구현은 /delete 페이지 없이 바로 /delete_process 페이지로 넘어가며 구현은 update_process 와 동일합니다.<br>
`_data` 디렉토리 내 파일을 삭제할 때는 fs.unlink(`filepath`, function(err)) 함수를 사용합니다.<br>
<br>
<br>
<hr>
> 전체 소스 코드

`> main.js`
```js
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');

function templateHTML(title, list, description, control){
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
          var control = `
            <a href="/create">create</a>
          `;
          var template = templateHTML(title, list, description, control);

          response.writeHead(200);
          response.end(template);
        });
      }else{
        fs.readFile(`data/${id}`, 'utf8', function(err, description){
          fs.readdir('./data', function(err,filelist){
            var title = id;
            var list = templateList(filelist);
            var control = `
              <a href="/create">create</a>
              <a href="/update?id=${title}">update</a>
              <form action="/delete_process" method="post">
                <input type="hidden", name="id", value="${title}">
                <input type="submit", value="delete">
              </form>
            `
            var template = templateHTML(title, list, description, control);

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
        var template = templateHTML(title, list, description, control);

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

        var title = post.title;
        var description = post.description;
        fs.writeFile(`data/${title}`, description, `utf8`, function(err){
          response.writeHead(302, {Location: `/?id=${title}`});
          response.end();
        })
      });
    }else if(pathname === '/update'){
      fs.readdir('./data', function(err,filelist){
        fs.readFile(`data/${queryData.id}`, `utf8`, function(err, description){
          var title = queryData.id;
          var list = templateList(filelist);
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
          var template = templateHTML(title, list, _description, control);
          response.writeHead(200);
          response.end(template);
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
<br><br>
<br>
<hr>
> create, update, delete 구현 화면

![test](/assets/images/create_update_delete.gif)<br>
<br>
