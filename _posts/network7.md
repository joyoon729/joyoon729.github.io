---
title: Network 기초 - 7. Domain name Structure
date: 2019-02-28 00:00:01
categories: Network
tags:
  - Network
header:
  teaser: /assets/images/home_server_teaser.jpg
excerpt: Home Server 를 구축하기 위한 기초 공부. 도메인 이름의 구조를 알아봅니다.
---

> 도메인 이름의 구조

![domain_name_structure](/assets/images/domain_name_structure1.jpeg)

<cite>[출처: 생활코딩 - WEB2-Domain Name System.8.도메인 이름의 구조](https://opentutorials.org/course/3276/20303)</cite>
{: .text-right}

도메인은 위와 같은 구조로 이루어져 있습니다.<br>
- **( . )** : 가장 오른쪽에 찍혀있는 점이고 Root 레벨을 뜻합니다. 보통은 생략되어 있습니다.
- **com** : Top-level
- **example** : Second-level
- **blog** : sub-level

왼쪽이 하위계층, 오른쪽이 상위계층의 도메인입니다.<br>
<br>
<br>
<hr>

> DNS 서버를 통해 ip 주소를 얻는 과정

![domain_name_structure](/assets/images/domain_name_structure2.jpeg)

<cite>[출처: 생활코딩 - WEB2-Domain Name System.8.도메인 이름의 구조](https://opentutorials.org/course/3276/20303)</cite>
{: .text-right}

각 계층마다 DNS 서버가 존재하는데, 각각의 특징은 이렇습니다.
- **Root** : Top-level DNS 서버의 ip 주소를 알고 있습니다. 
- **Top-level** : Second-level DNS 서버의 ip 주소를 알고 있습니다.
- **Second-level** : sub-level DNS 서버의 ip 주소를 알고 있습니다.
- **sub** : 찾고자 하는 서버의 최종 ip 주소를 알고 있습니다.

이러한 특징을 바탕으로 도메인 이름을 통해 ip 주소를 얻는 과정은 이렇습니다.<br>

1. 내 컴퓨터가 `blog.example.com` 이라는 도메인 이름을 Root level 의 DNS 서버에 물어본다.
2. Root DNS 서버는 `com` 을 확인한 후, com DNS 서버의 ip 주소를 알려준다.
3. 내 컴퓨터가 `blog.example.com` 이라는 도메인 이름을 *com DNS 서버* 에 물어본다.
4. com DNS 서버는 `example` 을 확인한 후, example DNS 서버의 ip 주소를 알려준다.
5. 내 컴퓨터가 `blog.example.com` 이라는 도메인 이름을 *example DNS 서버* 에 물어본다.
6. example DNS 서버는 `blog` 를 확인한 후, blog DNS 서버의 ip 주소를 알려준다.
7. blog DNS 서버는 `blog.example.com` 에 해당하는 서버의 ip 주소를 알려준다.

<br>
<br>
<br>