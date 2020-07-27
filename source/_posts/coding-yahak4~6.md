---
title: 코딩야학 4~6일차
date: 2019-01-02 00:00:01
categories: 코딩야학
tags:
  - 코딩야학
  - HTML
header:
  teaser: /assets/images/coding-yahak.png
excerpt: HTML 문서 기본구조, 배운 태그 정리, 원시웹, client & server
---

> HTML 문서 기본 구조

```html
<!doctype html>
<html>
  <head>
    <!--- 해당 html 페이지를 설명함 --->
    <title></title>
    <meta>
  </head>
  <body>
    <!--- 본문 --->
  </body>
</html>
```
<br><br>
<hr>
> 배운 태그 정리

```html
<a href="link_path" target="_blank" title="title_name">Hello</a>
<!--- <a> Tag.
속성으로 'href'(hypertext reference) 를 반드시 갖는다.
새 창 띄우고 싶을땐 target="_blank" 속성 추가. --->
<ul>
  <li>item1</li>
  <li>item2</li>
</ul>
<ol>
  <li>item1</li>
  <li>item2</li>
</ol>
<!-- <ul>, <ol>, <li> Tag.
<ul> : unordered list
<ol> : ordered list
<li> : 반드시 <ul> 이나 <ol> 을 부모태그로 갖는다. --->
```
<br><br>
<hr>
> 원시웹

**[info.cern.ch](http://info.cern.ch)**
<br>
개인적으로 원시웹에 관한 설명을 들으며 실제 저 주소를 찾아가 들어가 봤을때 소름이 돋았다.
<br><br>
<hr>
> client & server

![client & server](/assets/images/coding-yahak4~6-client_server.PNG)<br>

<cite>[출처: 생활코딩 - 인터넷을 여는 열쇠: 서버와 클라이언트](https://opentutorials.org/course/3084/18890)</cite>
{: .text-right}
<br>
