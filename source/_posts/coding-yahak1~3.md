---
title: 코딩야학 1~3일차
date: 2019-01-02 00:00:00
categories: 코딩야학
tags:
  - 코딩야학
  - HTML
header:
  teaser: /assets/images/coding-yahak.png
excerpt: 코딩야학 코딩수업의 내용은 HTML 기초이다. 이미 학교에서 수강했던 내용들이라 너무 쉽게만 느껴졌지만, [생활코딩](https://opentutorials.org/course/1)의 node.js 내용을 들으려는 사람들은 HTML 수업을 선수강하기를 권장해 복습할 겸 수강하기로 결정했다. 앞으로 수업 내용을 블로그에 정리해두려 한다.
---

코딩야학 5기를 신청해 수강중이다. 원래 일정은 1월 3일부터 시작이지만, 사실 날짜에 크게 구애받는 수업은 아니어서 바로 시작했다.<br>
코딩야학의 코딩수업을 신청하면 진도표에 자기가 공부한 만큼 진도를 체크할 수 있게 되는데, 하루에 한 강의에 10분 정도 되는 영상을 3~4개 수강하도록 진도가 짜여져있다.<br>
1일차에 30분 정도 투자하는 셈인데, 첫 부분은 30분만 듣자니 감질만 나고 아쉬워서 3일차 진도까지 수강했다.<br>
<br>
코딩야학 코딩수업의 내용은 HTML 기초이다. 이미 학교에서 수강했던 내용들이라 너무 쉽게만 느껴졌지만, [생활코딩](https://opentutorials.org/course/1)의 node.js 내용을 들으려는 사람들은 HTML 수업을 선수강 하기를 권장해 복습할 겸 수강하기로 결정했다.<br>
<br>
앞으로 수업 내용을 블로그에 정리해두려 한다.
<br>
<hr>
> HTML 태그(Tag)

```html
<tag_name></tag_name>
```
여는 태그와 닫는 태그로 이루어진다. *(`<br>`과 같은 예외도 있음)*
<br>
<br>
<hr>
> 의미에 맞는 태그 사용

```html
<span style="font-size:22px">`Title`</span>
```
VS
```html
<h1>`Title`</h1>
```
위의 `<span>` 은 단지 디자인적으로 제목에 맞춘것에 불과.
검색엔진 등에 노출되기 위해서는 **제목1** 을 뜻하는 `<h1>` 태그를 사용하는 것이 적절하며 의미론적으로 해석하기에도 쉽다.

**<i style="font-size:1.5em;" class="fas fa-lightbulb"></i> "`<h1>` 태그를 사용했으니 제목이겠구나!"**
{: .notice--success}

<br>
<hr>
> 배운 태그 정리

```html
<h1...6></h1...6>
```
제목(header). 1이 가장 중요(가장 크다)하고 6이 가장 덜 중요한(가장 작다) 제목을 의미.<br>
<br><br>
```html
<br>
```
줄바꿈(linebreak). .html 파일에서 줄바꿈을 하더라도 브라우저에 반영되지 않는다. `<br>` 태그를 이용해야함.<br>
<br><br>
```html
<p></p>
```
단락(paragraph). <br>

**<i style="font-size:1.5em;" class="fas fa-exclamation-triangle"></i> `<br>` 로 단락을 구분하는 것 보다도 `<p>` 태그를 사용해 단락을 구분하는 것이 더 적절함.**
{: .notice--danger}
<br>
<br><br>
```html
<img src="file-path" width="100%">
```
이미지(image) 삽입. 태그는 태그 그 자체로 사용되기도 하지만 `<img>` 태그와 같이 **_속성(attribute)_** 을 통해 태그가 갖는 정보를 추가하기도 한다.<br>
`<img>` 태그에서는 **src**, **width** 등의 속성값들을 갖는다.<br>
<br><br>
