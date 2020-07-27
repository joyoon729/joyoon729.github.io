---
title: Network 기초 - 2. NAT
date: 2019-02-20 00:00:00
categories: Network
tags:
  - Network
header:
  teaser: /assets/images/home_server_teaser.jpg
excerpt: Home Server 를 구축하기 위한 기초 공부. NAT 에 대해 알아봅니다.
---

> NAT (Network Address Translation)

![Local Area Network with Router](/assets/images/local_network.png)

<cite>[출처: 생활코딩 - WEB2-Home server. 공유기](https://opentutorials.org/course/3265/20033)</cite>
{: .text-right}
<br>
*private address* 할당에 따라 지금 **'내 노트북'**의 private address 가 `192.168.0.4` 라고 생각해봅시다.<br>
이 192.168.0.4 라는 ip 주소로는 바깥의 인터넷 세계에서는 목표 ip 주소(Destination ip address)로 정해 들어올 수 없습니다. 로컬 네트워크 안에서만 통용되는 주소이기 때문입니다.<br>
그렇다면 '내 노트북'에서 외부 사이트로 어떤 요청을 보내야한다면 '내 노트북'의 ip 주소는 어떤식으로 표기되어야 할까요?<br>
이 문제를 라우터(Router) 가 중간에서 해결해줍니다.<br>
라우터는 ***192.168.0.4 로부터 A서버 로의 요청***이 있을때 `192.168.0.4` 로부터의 요청이 있었음을 **기록** 해 두고, `A 서버` 로 요청을 보냅니다.<br>
대신 여기서 시작 ip 주소(Source ip address)를 라우터의 ip 주소인 `59.6.66.238` 로 표기해 보내게 됩니다.<br>
59.6.66.238 로부터의 요청을 받은 A 서버는, 이 주소로 응답을 하게 되고, 라우터는 이 응답을 로컬 네트워크 내 원래 주소인 192.168.0.4 로 보내주게 되는겁니다.<br>
이 과정을 **NAT(Network Address Translation)** 이라고 합니다.<br>