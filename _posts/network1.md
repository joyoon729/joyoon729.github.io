---
title: Network 기초 - 1. Router
date: 2019-02-10 00:00:00
categories: Network
tags:
  - Network
header:
  teaser: /assets/images/home_server_teaser.jpg
excerpt: Home Server 를 구축하기 위한 기초 공부. Router 에 대해 알아봅니다.
---

**making my server** 프로젝트를 진행하기 위해 집에서 서버를 동작시키기 위한<br>
네트워크에 대한 기초적인 이해가 필요함을 알았습니다.<br>
학교 수업에서 전반적인 네트워크 이론을 통해 배웠던 내용이지만, 실제로 적용해보진 못했는데요.<br>
Opentutorials.org 생활코딩의 [WEB2 - Home Server](https://opentutorials.org/course/3265) 강의를 통해<br>
이론을 적용해보는 공부를 해보도록 합니다.<br>
<br>
<br>
<hr>

> Router (공유기)

![Local Area Network with Router](/assets/images/local_network.png)

<cite>[출처: 생활코딩 - WEB2-Home server. 공유기](https://opentutorials.org/course/3265/20033)</cite>
{: .text-right}

ip address 는 IPv4 기준, 4,294,967,296 개가 존재할 수 있습니다.<br>
하지만 이 ip 주소 수는 전세계에 인터넷에 연결되는 모든 디바이스들을 할당하기엔 턱없이 부족합니다.<br>
이러한 문제를 *라우터(Router)* 가 도움을 줍니다. 주변에서 익숙하게 본 *공유기*라고도 하는데요.<br>
일반적으로 인터넷에 직접 **닿아있는, 연결되는** 디바이스에 *public address* 를 부여하고 <br>
이 디바이스를 통로로 삼아 작은 네트워크를 구성하는 구성원(디바이스)에 *private address* 를 부여하는 식입니다.<br>
*public address* 는 전세계에 한개만 존재할테지만, *private address* 는 서로 다른 로컬 네트워크 사이에서 중복해서 존재할 수 있겠죠.<br>
그리고 이 로컬 네트워크와, 광역 네트워크 사이를 이어주는 디바이스를 라우터(Router) 라고 부릅니다.<br>
로컬 네트워크를 **LAN(Local Area Network)** , 광역 네트워크를 **WAN(Wide Area Network)** 라고 합니다.<br>
로컬 네트워크 안에서, 라우터가 갖는 ip address 를 특별히 *Gateway address* 또는 *Router address* 라고 부릅니다.<br>
<br>
여기서 *public address* 와 *private address* 가 같은 값을 가지면 안되겠죠.<br>
그래서 *private address* 는 특정 범위의 주소들만 할당하도록 약속되어있습니다.<br>
<br>

|**ip address 범위**|**ip address 수**|
|:--:|:--:|
|10.0.0.0 ~ 10.255.255.255|16,777,216 개|
|172.16.0.0 ~ 172.31.255.255|1,048,576 개|
|192.168.0.0 ~ 192.168.255.255|65,436 개|

이 범위 안에있는 ip 주소는 무조건 *private address* 인 셈입니다.<br>
<br>
<br>