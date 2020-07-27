---
title: Network 기초 - 4. Port Forwarding
date: 2019-02-26 00:00:00
categories: Network
tags:
  - Network
header:
  teaser: /assets/images/home_server_teaser.jpg
excerpt: Home Server 를 구축하기 위한 기초 공부. Port Forwarding 에 대해 알아봅니다.
---

> 포트 포워딩 (Port Forwarding)

라우터의 기능을 이용해 외부 클라이언트가 내 서버로 접속하기 위해 포트 포워딩을 하는법을 공부해봅니다.<br>
<br>
원리는 간단합니다. <br>
외부 클라이언트는 내 서버로 접속하기 위해 라우터의 주소를 보고 접속하기 때문에, 라우터 쪽에서 외부 클라이언트의 요청을 받으면 내 서버로의 주소로 넘겨주는 겁니다.<br>
단, 라우터에 연결된 디바이스들이 여러개이기 때문에, 라우터 주소의 포트(외부 포트)를 통해 내부의 어느 디바이스로 보낼지 결정하게 됩니다.<br>

![port_forwarding](/assets/images/port_forwarding.png)

<cite>[출처: 생활코딩 - WEB2-Home Server. Port](https://opentutorials.org/course/3265/20037)</cite>
{: .text-right}

위의 그림에서 알 수 있듯이, 외부 클라이언트가 `59.6.66.238:8080` 으로 요청하면 라우터는 로컬 네트워크의 `192.168.0.3:80` 으로 요청을 보내줍니다.<br>
<br>
<br>
<hr>

> 포트 포워딩 적용하기

먼저 라우터(공유기) 설정 창에 접속해야 합니다. 인터넷 브라우저에서 라우터 주소를 입력하면 접속 가능합니다.<br>
저는 현재 KT 공유기를 사용중이기 때문에, KT 공유기 기준으로 설명하겠습니다..<br>
<br>
`장치설정 - 트래픽관리 - 포트포워딩 설정`
![port_forwarding_kt_router](/assets/images/port_forwarding2.png)

- 여기서 **외부 포트** 란 외부 클라이언트가 공유기의 **public address** 로 접속할 때 몇번 포트로 접속할 지를 말합니다.<br> 저는 제가 자주 사용하는 3000 으로 설정했습니다.<br>
- **내부 IP 주소** 는 이전 포스트에서 알아냈던 그 내부 ip 주소를 말합니다.<br> 해당 공유기의 로컬 네트워크에 속한 디바이스의 **private address** 를 뜻하는 거죠.<br> 저는 현재 제 컴퓨터의 내부 IP 주소로 설정했습니다.<br>
- **내부 포트** 는 이제 내 컴퓨터의 서버가 어느 포트로 '리스닝' 하고 있는지에 따라 결정됩니다.<br> 라우터는 내부 ip 주소의 내부 포트로 클라이언트의 요청을 보내게됩니다.<br> 마찬가지로 3000 으로 설정했습니다.

<br>
<br>