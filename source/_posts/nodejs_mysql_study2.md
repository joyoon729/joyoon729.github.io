---
title: Node.js & MySQL Study 2
date: 2019-02-07 00:00:00
categories: Web
tags:
  - Node.js
  - MySQL
header:
  teaser: /assets/images/nodejs_mysql.png
excerpt: 파일 형태로 글을 저장하던 방식에서 데이터베이스를 이용해 글을 저장하는 방식으로 구현하기...
---

> 기존 코드를 MySQL 을 이용한 코드로 바꾸기

기존 코드는 **파일**의 형태로 글을 저장하도록 짜여져있었습니다.<br>
이제 Node.js에 MySQL 을 사용할 수 있도록 패키지를 설치했으니 **파일** 대신 **데이터베이스** 에 글을 저장하도록 바꿔봅시다.<br>
<br>
<br>
## 'Home' 페이지 구현
`main.js`
```js
if(queryData.id === undefined){
  /* fs.readdir('./data', function(error, filelist){
      var title = 'Welcome';
      var description = 'Hello, Node.js';
      var list = template.list(filelist);
      var html = template.HTML(title, list,
        `<h2>${title}</h2>${description}`,
        `<a href="/create">create</a>`
      );
      response.writeHead(200);
      response.end(html);
    }); */
  db.query(`SELECT * FROM topic`, function(error, contents){
    var title = 'Welcome';
    var description = 'Hello, Node.js';
    var list = template.list(contents);
    var html = template.HTML(title, list,
      `<h2>${title}</h2>${description}`,
      `<a href="/create">create</a>`);
    response.writeHead(200);
    response.end(html);
  });
}
```
위 코드는 주소가 http://localhost:4000/ 일때, 즉 /(루트) 일때 작동하는 코드조각입니다.<br>
주석 처리된 부분이 이전 **파일**로 처리됐었던 부분입니다.<br>
db.query() 함수의 콜백함수 인자로 들어오는 *contents* 변수에 mysql query 결과가 배열 형태로 들어오게됩니다.<br>
때문에 /lib/template.js 의 template.list() 함수를 조금 수정해줬습니다.<br>
<br>
`변경된 template.js`
```js
list:function(contents){    
    var list = '<ul>';
    var i = 0;
    while(i < contents.length){
      list = list + `<li><a href="/?id=${contents[i].id}">${contents[i].title}</a></li>`; // query 결과가 배열 형태로 오므로 수정
      i = i + 1;
    }
    list = list+'</ul>';
    return list;
  }
```
<br>
<br>
## '글 상세보기' 페이지 구현
`main.js`
```js
else {
  /* fs.readdir('./data', function(error, filelist){
    var filteredId = path.parse(queryData.id).base;
    fs.readFile(`data/${filteredId}`, 'utf8', function(err, description){
      var title = queryData.id;
      var sanitizedTitle = sanitizeHtml(title);
      var sanitizedDescription = sanitizeHtml(description, {
        allowedTags:['h1']
      });
      var list = template.list(filelist);
      var html = template.HTML(sanitizedTitle, list,
        `<h2>${sanitizedTitle}</h2>${sanitizedDescription}`,
        ` <a href="/create">create</a>
          <a href="/update?id=${sanitizedTitle}">update</a>
          <form action="delete_process" method="post">
            <input type="hidden" name="id" value="${sanitizedTitle}">
            <input type="submit" value="delete">
          </form>`
      );
      response.writeHead(200);
      response.end(html);
    });
  }); */
  db.query(`SELECT * FROM topic`, function(error, contents){
    db.query(`SELECT * FROM topic WHERE id=?`, [queryData.id], function(error, content){ // *설명1.
      var title = content[0].title;
      var list = template.list(contents);
      var description = content[0].description;
      var html = template.HTML(title, list,
        `<h2>${title}</h2>${description}`,
        ` <a href="/create">create</a>
          <a href="/update?id=${queryData.id}">update</a>
          <form action="delete_process" method="post">
            <input type="hidden" name="id" value="${queryData.id}">
            <input type="submit" value="delete">
          </form>`)
      response.writeHead(200);
      response.end(html);
    });
  });   
}
```
코드조각만 가져와서 다소 이해하기 힘들 수도 있을것 같습니다..<br>
위 코드는 글을 클릭했을때 글 내용을 보여주는 '상세보기' 페이지를 처리하는 코드입니다.<br>
주석처리된 부분은 마찬가지로 **파일** 형태로  처리했었던 코드입니다.<br>
코드를 보면 아시겠지만, 크게 다른점은 없고 기존 node.js 모듈인 ***fs(filesystem)*** 대신에 mysql 패키지를 이용해 `db.query()` 함수를 이용했다는 점만 다릅니다.<br>
<br>
### *설명1.
보안상의 문제로 query 문에 모든 정보를 넣지 않고 **? 키워드**를 이용해 가리고, 원래 정보를 그 다음 인자에 배열 형태로 넣습니다.<br>
가리는 정보가 여러개라면 **? 키워드**를 여러개 사용하고, 배열에 순서대로 넣어주면 됩니다.<br>
C언어의 printf 문을 생각하면 쉬울 것 같습니다.<br>
<br>
<br>
## '글 작성, 수정, 삭제' 구현
<br>
마찬가지로 CREATE, UPDATE, DELETE query 문을 이용해 글 작성, 수정, 삭제를 구현하면 됩니다.<br>
### 알아둘 점1. 
**Database** 를 이용하게 되면서, 이제 글 제목 대신에 글에 `id` 값을 주어 구분할 수 있게 되었는데요.<br>
이 때 글을 새로 생성할 때 생성된 글의 **id** 값을 가져오는 방법입니다.<br>
```js
db.query(`INSERT INTO topic (title, description, created, author_id) VALUES(?, ?, NOW(), ?)`, [title, description, 1], function(error, result){
  if(error) throw error;
  response.writeHead(302, {Location: `/?id=${result.insertId}`}); // result.insertId 여기에 생성된 글의 id가 있습니다.
  response.end();
});
```
콜백함수의 2번째 인자 **result** 의 `insertId` property 에 **Database**에 생성된 해당 글의 id가 담겨져 있습니다.<br>
<br>
### 알아둘 점2.
**post** 방식으로 정보를 넘겨줄 때, `<input>` 태그의 속성 `type="hidden"` 을 이용해 숨겨서 넘겨주면<br>
받았을 때의 변수명이 앞에 'undefined' 가 추가되어 있습니다.<br>
```js
<input type="hidden" name="id" value="123">
```
예를 들어 id 변수에 '123' 값을 hidden 속성을 추가해 넘겨주면,<br>
`undefinedid=123` 의 형태로 받게됩니다.<br>
<br>
<br>
<hr>

> 전체 소스코드

```js
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');
var template = require('./lib/template.js');
var path = require('path');
var sanitizeHtml = require('sanitize-html');
var mysql = require('mysql');
var db = mysql.createConnection({
  host: 'localhost',
  user: 'joyoon',
  password: '1234',
  database: 'opentutorials'
});
db.connect();

var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    if(pathname === '/'){
      if(queryData.id === undefined){
        /* fs.readdir('./data', function(error, filelist){
          var title = 'Welcome';
          var description = 'Hello, Node.js';
          var list = template.list(filelist);
          var html = template.HTML(title, list,
            `<h2>${title}</h2>${description}`,
            `<a href="/create">create</a>`
          );
          response.writeHead(200);
          response.end(html);
        }); */
        db.query(`SELECT * FROM topic`, function(error, contents){
          var title = 'Welcome';
          var description = 'Hello, Node.js';
          var list = template.list(contents);
          var html = template.HTML(title, list,
            `<h2>${title}</h2>${description}`,
            `<a href="/create">create</a>`);
          response.writeHead(200);
          response.end(html);
        });
      } else {
        /* fs.readdir('./data', function(error, filelist){
          var filteredId = path.parse(queryData.id).base;
          fs.readFile(`data/${filteredId}`, 'utf8', function(err, description){
            var title = queryData.id;
            var sanitizedTitle = sanitizeHtml(title);
            var sanitizedDescription = sanitizeHtml(description, {
              allowedTags:['h1']
            });
            var list = template.list(filelist);
            var html = template.HTML(sanitizedTitle, list,
              `<h2>${sanitizedTitle}</h2>${sanitizedDescription}`,
              ` <a href="/create">create</a>
                <a href="/update?id=${sanitizedTitle}">update</a>
                <form action="delete_process" method="post">
                  <input type="hidden" name="id" value="${sanitizedTitle}">
                  <input type="submit" value="delete">
                </form>`
            );
            response.writeHead(200);
            response.end(html);
          });
        }); */
        db.query(`SELECT * FROM topic`, function(error, contents){
          db.query(`SELECT * FROM topic WHERE id=?`, [queryData.id], function(error, content){
            var title = content[0].title;
            var list = template.list(contents);
            var description = content[0].description;
            var html = template.HTML(title, list,
              `<h2>${title}</h2>${description}`,
              ` <a href="/create">create</a>
                <a href="/update?id=${queryData.id}">update</a>
                <form action="delete_process" method="post">
                  <input type="hidden" name="id" value="${queryData.id}">
                  <input type="submit" value="delete">
                </form>`)
            response.writeHead(200);
            response.end(html);
          });
        });   
      }
    } else if(pathname === '/create'){
      /* fs.readdir('./data', function(error, filelist){
        var title = 'WEB - create';
        var list = template.list(filelist);
        var html = template.HTML(title, list, `
          <form action="/create_process" method="post">
            <p><input type="text" name="title" placeholder="title"></p>
            <p>
              <textarea name="description" placeholder="description"></textarea>
            </p>
            <p>
              <input type="submit">
            </p>
          </form>
        `, '');
        response.writeHead(200);
        response.end(html);
      }); */
      db.query(`SELECT * FROM topic`, function(error, contents){
        var title = 'WEB - create';
        var list = template.list(contents);
        var html = template.HTML(title, list, 
          `<form action="/create_process" method="post">
          <p><input type="text" name="title" placeholder="title"></p>
          <p>
            <textarea name="description" placeholder="description"></textarea>
          </p>
          <p>
            <input type="submit">
          </p>
        </form>`,
          '');
        response.writeHead(200);
        response.end(html);
      });
    } else if(pathname === '/create_process'){
      /* var body = '';
      request.on('data', function(data){
          body = body + data;
      });
      request.on('end', function(){
          var post = qs.parse(body);
          var title = post.title;
          var description = post.description;
          fs.writeFile(`data/${title}`, description, 'utf8', function(err){
            response.writeHead(302, {Location: `/?id=${title}`});
            response.end();
          })
      }); */
      var body = '';
      request.on('data', function(data){
          body = body + data;
      });
      request.on('end', function(){
          var post = qs.parse(body);
          var title = post.title;
          var description = post.description;
          
          db.query(`INSERT INTO topic (title, description, created, author_id) VALUES(?, ?, NOW(), ?)`, [title, description, 1], function(error, result){
            if(error) throw error;
            response.writeHead(302, {Location: `/?id=${result.insertId}`});
            response.end();
          });
      }); 
    } else if(pathname === '/update'){
      /* fs.readdir('./data', function(error, filelist){
        var filteredId = path.parse(queryData.id).base;
        fs.readFile(`data/${filteredId}`, 'utf8', function(err, description){
          var title = queryData.id;
          var list = template.list(filelist);
          var html = template.HTML(title, list,
            `
            <form action="/update_process" method="post">
              <input type="hidden" name="id" value="${title}">
              <p><input type="text" name="title" placeholder="title" value="${title}"></p>
              <p>
                <textarea name="description" placeholder="description">${description}</textarea>
              </p>
              <p>
                <input type="submit">
              </p>
            </form>
            `,
            `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
          );
          response.writeHead(200);
          response.end(html);
        });
      }); */
      db.query(`SELECT * FROM topic`, function(error, contents){
        db.query(`SELECT * FROM topic WHERE id=?`, [queryData.id], function(error, content){
          var id = content[0].id;
          var title = content[0].title;
          var list = template.list(contents);
          var description = content[0].description;
          var html = template.HTML(title, list,
            `<form action="/update_process" method="post">
              <input type="hidden" name="id" value="${id}">
              <p><input type="text" name="title" placeholder="title" value="${title}"></p>
              <p>
                <textarea name="description" placeholder="description">${description}</textarea>
              </p>
              <p>
                <input type="submit">
              </p>
            </form>`,
            `<a href="/create">create</a> <a href="/update?id=${id}">update</a>`);
            response.writeHead(200);
            response.end(html);
        });
      });

    } else if(pathname === '/update_process'){
      /* var body = '';
      request.on('data', function(data){
          body = body + data;
      });
      request.on('end', function(){
          var post = qs.parse(body);
          var id = post.id;
          var title = post.title;
          var description = post.description;
          fs.rename(`data/${id}`, `data/${title}`, function(error){
            fs.writeFile(`data/${title}`, description, 'utf8', function(err){
              response.writeHead(302, {Location: `/?id=${title}`});
              response.end();
            })
          });
      }); */
      var body = '';
      request.on('data', function(data){
        body = body + data;
      });
      request.on('end', function(){
        var post = qs.parse(body);
        var id = post.undefinedid;
        var title = post.title;
        var description = post.description;
        db.query(`UPDATE topic SET title=?, description=? WHERE id=?`, [title, description, id], function(error, result){
          response.writeHead(302, {Location: `/?id=${id}`});
          response.end();
        });
      });
      
    } else if(pathname === '/delete_process'){
      /* var body = '';
      request.on('data', function(data){
          body = body + data;
      });
      request.on('end', function(){
          var post = qs.parse(body);
          var id = post.id;
          var filteredId = path.parse(id).base;
          fs.unlink(`data/${filteredId}`, function(error){
            response.writeHead(302, {Location: `/`});
            response.end();
          });
      }); */
      var body = '';
      request.on('data', function(data){
        body = body + data;
      });
      request.on('end', function(){
        var post = qs.parse(body);
        console.log(post);
        var id = post.id;
        db.query(`DELETE FROM topic WHERE id=?`, [id], function(error, result){
          response.writeHead(302, {Location: `/`});
          response.end();
        });
      });
    } else {
      response.writeHead(404);
      response.end('Not found');
    }
});
app.listen(4000);

```