---
title: Network 기초 - 8. 무료 도메인 서비스 이용하기
date: 2019-03-01 00:00:00
categories: Network
tags:
  - Network
header:
  teaser: /assets/images/home_server_teaser.jpg
excerpt: Home Server 를 구축하기 위한 기초 공부. 무료 도메인 등록 서비스인 freenom.com 을 이용해봅니다.
---

> 도메인 등록

![regist_domain](/assets/images/regist_domain.png)

<cite>[출처: 생활코딩 - WEB2-Domain Name System.9.도메인 이름 등록 과정과 원리](https://opentutorials.org/course/3276/20307)</cite>
{: .text-right}

자신이 원하는 도메인을 DNS 서버에 등록하기 위해선 일정 비용을 지불하고 *'등록대행자'* 에게 등록을 요청하면 됩니다.<br>
<br>

<i style="font-size:1.5em;" class="fas fa-exclamation-triangle"></i> **Domain Record Type** <br>
이전 공부에서 **hosts** 파일에 ip주소와 도메인 이름을 넣어줬던것 처럼, 각 DNS 서버에는 이러한 hosts 파일을 갖고 있는데, 이 파일 안의 하나의 정보를 **Domain Record** 라고 합니다.<br>
도메인 레코드는 여러개의 타입을 갖고 있는데, 그 중 **'A'** 타입과 **'NS'** 타입에 대해 알아보겠습니다.
- A type
  - Address 타입입니다. `도메인이름 A ip주소` 의 형식이고, 도메인이름에 ip주소를 연결짓습니다.
- NS type
  - Name Server 타입입니다. `도메인이름 NS 네임서버` 의 형식이고, 도메인이름에 관한 정보를 알고 있는 네임서버 도메인을 연결합니다.

<br>
<br>
<hr>

> 무료로 도메인 등록하기

1년간 무료로 도메인을 등록해 사용할 수 있는 서비스가 있습니다.<br>
[freenom.com](freenom.com) 입니다. 최상위 도메인 중 몇가지 도메인에 대해 1년간 무료로 사용할 수 있습니다.<br>
<br>
![freenom.com](/assets/images/freenom.png)

가입을 하고 도메인 등록 절차를 진행하면 되는데, 홈페이지에 가입 버튼이 보이질 않아 헤맸습니다.<br>
가운데에 *"Find a new FREE domain"* 란에 원하는 도메인을 입력하고 절차를 진행하다보면 마지막에 회원가입을 할 수 있습니다. 일단 먼저 진행해야 합니다.<br>
그리고 공유기의 외부 ip를 해당 도메인 이름에 등록해줌으로써 이제 ip 대신 도메인 이름으로 접속이 가능해졌습니다.<br>
<br>
`결과. 도메인 이름으로 접속`
![noip](/assets/images/noip.png)

도메인 이름은 앞으로 개인적으로 사용할 것이기에 가렸습니다.
<br>