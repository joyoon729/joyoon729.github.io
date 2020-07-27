---
title: Network 기초 - 3. Port
date: 2019-02-25 00:00:00
categories: Network
tags:
  - Network
header:
  teaser: /assets/images/home_server_teaser.jpg
excerpt: Home Server 를 구축하기 위한 기초 공부. Port 에 대해 알아봅니다.
---

> 외부 ip, 내부 ip 알아내기

외부 ip 를 알아내는 법은 간단합니다. 검색엔진을 통해 'my ip' 라고 검색해보면 간단한 어플리케이션들을 통해 알아낼 수 있습니다.<br>
예시 : [https://www.myip.com/](https://www.myip.com/)
<br>
<br>
내부 ip 를 알아내는 방법은 위보단 조금 복잡한데 어렵진 않습니다.<br>
운영체제 별로 다른데, windows / mac 으로 나누어 설명해보겠습니다.<br>
### windows
- 윈도우키(<i class="fab fa-windows"></i>) + R 을 눌러 실행창을 띄우고 cmd 입력 후 확인을 눌러 명령 프롬프트를 실행시킵니다.
- ipconfig 를 입력합니다.
- IPv4 주소가 해당 디바이스의 내부 ip(private ip) 주소고, 기본 게이트웨이 주소가 라우터 주소를 뜻합니다.

### mac os
- 터미널 창에서 `ifconfig | grep inet` 을 입력합니다.
- inet xxx.xxx.xxx.xxx 가 내부 ip 주소를 뜻합니다.

<br>
<br>
<hr>

> Port

포트(Port) 란 하나의 디바이스에 대해 여러개의 연결이 존재할 때, 이를 구분하기 위해 사용됩니다.<br>
예를 들어, 어떤 서버 컴퓨터가 2개의 웹서버를 운용하고 있다면, 클라이언트가 이 서버 컴퓨터에 요청할 때 2개 중 어떤 웹서버에 요청할 것인지 구분하도록 도와주죠.<br>
포트 번호는 host 주소 뒤 path 앞에 표시합니다.<br>
<br>
```
http://hostname:port#/path
```
이런식입니다.

만약 localhost 서버가 80번 포트에 웹서버 A를, 8080번 포트에 웹서버 B를 운용하고 있고, 자기 자신이 웹서버 A의 index.html에 접속하고자 한다면
`http://localhost:80/index.html` 으로 접속하면 되고, <br>
웹서버 B라면 `http://localhost:8080/index.html` 으로 접속하면 됩니다.<br>

![Port 0 ~ 1023](/assets/images/port0~1023.png)

<cite>[출처: 생활코딩 - WEB2-Home Server. Port](https://opentutorials.org/course/3265/20037)</cite>
{: .text-right}

이러한 포트는 0부터 65535까지 존재하고, 0부터 1023까지의 포트를 **well-known port** 라고 하여 특별히 예약된 포트들을 말합니다.<br>
**well-known port** 는 대표적으로 22번 포트의 SSH, 80번 포트의 HTTP 연결이 있습니다.<br>
<br>

<i style="font-size:1.5em;" class="fas fa-lightbulb"></i> 80번 포트는 HTTP 연결로 예약된 포트이기 때문에 **http://** 로 시작하는 주소는 **:80** 이라는 포트번호가 생략되어 있습니다. <br>
마찬가지로 HTTPS 는 443번 포트에 예약되어 있기 때문에, 본 블로그도 **https://joyoon729.github.io:443/** 으로 접속해도 동일한 블로그에 접속한 것이 됩니다. **:433** 이 생략된 것이죠.
{: .notice--info}
<br>
<br>