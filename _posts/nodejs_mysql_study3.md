---
title: Node.js & MySQL Study 3
date: 2019-02-08 00:00:00
categories: Web
tags:
  - Node.js
  - MySQL
header:
  teaser: /assets/images/nodejs_mysql.png
excerpt: 코드 리팩토링 하기
---

> main.js refactoring

이제 어느정도 기본적인 기능을 구현해놓으니 `main.js` 가 제법 길어지고 복잡해졌습니다.<br>
`main.js` 가 좀더 단순해지고 가독성이 높아졌으면 좋겠어서 코드 리팩토링을 한번 하고 가겠습니다.<br>
리팩토링을 통해 가독성을 높일 수 있고, 나중에 있을 유지/보수에도 기여할 수 있을겁니다.<br>
<br>
<br>
<hr>

> 목표

`main.js` 가 도맡아 하던 일들을 몇개의 모듈로 나눠 관리하려합니다.<br>
+ `db.js` : 데이터베이스 연결에 필요한 정보를 `main.js` 에서 따로 빼내 관리하는 모듈
+ `topic.js` : 글 상세보기, 생성, 수정 등의 여러 기능들을 구현해 둔 모듈

<br>
<br>
<hr>

> db.js

`db.js`
```js
var mysql = require('mysql');

var db = mysql.createConnection({
    host: 'localhost',
    user: 'joyoon',
    password: '1234',
    database: 'opentutorials'
});

module.exports = db;
```
`main.js` 에 데이터베이스 연결에 필요한 로그인 정보들이 노출되면 안되기 때문에 `db.js` 모듈에 따로 관리합니다.<br>
그렇기 때문에 버전 관리시에도 이 모듈은 포함시켜선 안됩니다.<br>
하나의 property? 만 모듈에 존재할 경우, `module.exports = db` 와 같이 **module.exports** 를 이용해 export 합니다.<br>
<br>
<br>
<hr>

> topic.js

`topic.js` 에는 여러개의 기능별 함수들을 모두 포함하고 있기 때문에 아래와 같이 사용합니다.<br>

`/lib/topic.js`
```js
exports.aa = function(){
    /* TO DO */
}
exports.bb = function(int1, int2){
    /* TO DO */
}
```
<br>
`main.js`
```js
var topic = require('/lib/topic.js');

topic.aa();
topic.bb(1, 2);
```
<br>
<br>
'Home' 페이지를 구현하는 부분만 예를 들어보겠습니다.<br>
`./lib/topic.js`
```js
var db = require('./db.js');
var template = require('./template.js');
exports.index = function(request, response){
	db.query(`SELECT * FROM topic`, function(error, topics){
		if(error){
			console.log("index error");
			throw error;    
		} 
		var title = 'Welcome';
		var description = 'Hello, Node.js';
		var list = template.list(topics);
		var html = template.HTML(title, list,
			`<h2>${title}</h2>${description}`,
			`<a href="/create">create</a>`);
		response.writeHead(200);
		response.end(html);
	});
}
```
`main.js`
```js
var http = require('http');
var url = require('url');
var db = require('./lib/db.js');
var topic = require('./lib/topic.js');

db.connect();

var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    if(pathname === '/'){
      if(queryData.id === undefined) topic.index(request, response); // 'home' 페이지 구현
      else /* TO DO */
    } 
    else if(pathname === '/create') /* TO DO */
    else if(pathname === '/create_process') /* TO DO */
    else if(pathname === '/update') /* TO DO */
    else if(pathname === '/update_process') /* TO DO */
    else if(pathname === '/delete_process') /* TO DO */
    else {
        response.writeHead(404);
        response.end('Not found');
    }
});
```
이런식으로 하면 되겠습니다.<br>
단순히 코드를 `topic.js` 로 옮기고, 필요한 의존성들을 찾아서 추가해주는 식입니다.<br>
<br>
<br>
<hr>

> 전체 소스코드

`main.js`
```js
var http = require('http');
var url = require('url');
var db = require('./lib/db.js');
var topic = require('./lib/topic.js');

db.connect();

var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    if(pathname === '/'){
      if(queryData.id === undefined) topic.index(request, response);
      else topic.page(request, response);
    } 
    else if(pathname === '/create') topic.create(request, response);
    else if(pathname === '/create_process') topic.create_process(request, response);
    else if(pathname === '/update') topic.update(request, response);
    else if(pathname === '/update_process') topic.update_process(request, response);
    else if(pathname === '/delete_process') topic.delete(request, response);
    else {
			response.writeHead(404);
			response.end('Not found');
    }
});
app.listen(3000);

```

`db.js`
```js
var mysql = require('mysql');

var db = mysql.createConnection({
    host: 'localhost',
    user: 'joyoon',
    password: '1234',
    database: 'opentutorials'
});

module.exports = db;
```

`topic.js`
```js
var url = require('url');
var qs = require('querystring');
var db = require('./db.js');
var template = require('./template.js');

exports.index = function(request, response){
	db.query(`SELECT * FROM topic`, function(error, topics){
		if(error){
			console.log("index error");
			throw error;    
		} 
		var title = 'Welcome';
		var description = 'Hello, Node.js';
		var list = template.list(topics);
		var html = template.HTML(title, list,
			`<h2>${title}</h2>${description}`,
			`<a href="/create">create</a>`);
		response.writeHead(200);
		response.end(html);
	});
}

exports.page = function(request, response){
	var _url = request.url;
	var queryData = url.parse(_url, true).query;
	db.query(`SELECT * FROM topic`, function(error, topics){
		if(error){
				console.log("page error");
				throw error;
		}
		db.query(`SELECT * FROM topic LEFT JOIN author ON topic.author_id=author.id WHERE topic.id=?`, [queryData.id], function(error, topic){

			var title = topic[0].title;
			var list = template.list(topics);
			var description = topic[0].description;
			var html = template.HTML(title, list,
				`<h2>${title}</h2>${description}
				<p>by ${topic[0].name}</p>`,
				` <a href="/create">create</a>
				<a href="/update?id=${queryData.id}">update</a>
				<form action="delete_process" method="post">
					<input type="hidden" name="id" value="${queryData.id}">
					<input type="submit" value="delete">
				</form>`);
			response.writeHead(200);
			response.end(html);
		});
	}); 
}

exports.create = function(request, response){
	db.query(`SELECT * FROM topic`, function(error, topics){
		db.query(`SELECT * FROM author`, function(error, authors){
			var title = 'WEB - create';
			var list = template.list(topics);
			var html = template.HTML(title, list, 
				`<form action="/create_process" method="post">
					<p><input type="text" name="title" placeholder="title"></p>
					<p>
						<textarea name="description" placeholder="description"></textarea>
					</p>
					<p>
						${template.selectAuthor(authors)}
					</p>
					<p>
						<input type="submit">
					</p>
				</form>`,
				'');
			response.writeHead(200);
			response.end(html);
		});
	});
}

exports.create_process = function(request, response){
	var body = '';
	request.on('data', function(data){
		body = body + data;
	});
	request.on('end', function(){
		var post = qs.parse(body);
		var title = post.title;
		var description = post.description;
		var author = post.author_id
			
		db.query(`INSERT INTO topic (title, description, created, author_id) VALUES(?, ?, NOW(), ?)`, [title, description, author], function(error, result){
			if(error) throw error;
			response.writeHead(302, {Location: `/?id=${result.insertId}`});
			response.end();
		});
	}); 
}

exports.update = function(request, response){
	var _url = request.url;
	var queryData = url.parse(_url, true).query;
	db.query(`SELECT * FROM topic`, function(error, topics){
		db.query(`SELECT * FROM topic WHERE id=?`, [queryData.id], function(error, topic){
			db.query(`SELECT * FROM author`, function(error, authors){
				var id = topic[0].id;
				var title = topic[0].title;
				var list = template.list(topics);
				var description = topic[0].description;
				var html = template.HTML(title, list,
						`<form action="/update_process" method="post">
							<input type="hidden" name="id" value="${id}">
							<p><input type="text" name="title" placeholder="title" value="${title}"></p>
							<p>
									<textarea name="description" placeholder="description">${description}</textarea>
							</p>
							<p>
									${template.selectAuthor(authors, topic[0].author_id)}
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
	});
}

exports.update_process = function(request, response){
	var body = '';
	request.on('data', function(data){
		body = body + data;
	});
	request.on('end', function(){
		var post = qs.parse(body);
		var id = post.id;
		var title = post.title;
		var description = post.description;
		var author = post.author_id;
		db.query(`UPDATE topic SET title=?, description=?, author_id=? WHERE id=?`, [title, description, author, id], function(error, result){
			response.writeHead(302, {Location: `/?id=${id}`});
			response.end();
		});
	});
}

exports.delete = function(request, response){
	var body = '';
	request.on('data', function(data){
		body = body + data;
	});
	request.on('end', function(){
		var post = qs.parse(body);
		var id = post.id;
		db.query(`DELETE FROM topic WHERE id=?`, [id], function(error, result){
			response.writeHead(302, {Location: `/`});
			response.end();
		});
	});
}
```