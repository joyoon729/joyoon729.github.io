---
title: Network 기초 - 5. DHCP
date: 2019-02-27 00:00:00
categories: Network
tags:
  - Network
header:
  teaser: /assets/images/home_server_teaser.jpg
excerpt: Home Server 를 구축하기 위한 기초 공부. DHCP 에 대해 알아봅니다.
---

> 유동 ip 와 고정 ip

일반적으로 ip 는 바뀌고 있습니다. 만약 어떤 순간에 확인한 내 공유기의 외부 ip가 59.6.66.238 이었다면, 일정 시간이 지난후엔 내 공유기의 외부 ip가 59.6.66.238 이 아닐 수도 있다는 겁니다. 이를 **유동 ip** 라고 합니다.<br>
때문에 집에서 서버를 운용할때 어느 한 ip 주소로 서비스를 하려 했다면, 어느 순간엔 ip 주소가 변경돼 동작하지 않을 수도 있다는 말이죠.<br>
ip 주소를 변경하는 주체는 ISP(Internet Service Provider) 이고, **고정 ip** 를 사용해서 서버를 원활히 서비스하고 싶다면 ISP 에 일정 비용을 지불하고 고정 ip를 할당받을 수 있습니다.<br>
<br>
<br>
<hr>

> DHCP (Dynamic Host Configuration Protocol)

DHCP(Dynamic Host Configuration Protocol). 우리말로 동적 호스트 구성 프로토콜은 각 호스트(디바이스)들의 ip 구성을 관리하는 통신규약입니다.<br>
각 호스트들이 ip 를 할당받는 데 있어서 할당받는 호스트를 **DHCP client** 할당해 주는 호스트를 **DHCP server** 라고 하고,<br>
DHCP client 가 어떤 ip 로 할당받을지, 중복된 ip인지 고민할 필요 없이, DHCP server 에서 일괄적으로 맡은 client 들에게 동적으로 ip를 할당해 줍니다.<br>
동작 방식은 이렇습니다.<br>

![DHCP](/assets/images/dhcp.png)
<cite>[출처: 생활코딩 - WEB2-Home Server. DHCP](https://opentutorials.org/course/3265/20039)</cite>
{: .text-right}

1. 모든 인터넷에 연결되는 디바이스들은 공장에서 출고될때 각각의 MAC(Media Access Control) Address 를 가지고 있습니다. 고유한 주소입니다.
2. `8c:85:90:0c:e3:cc` 라는 MAC 주소를 가진 내 컴퓨터에서 "ip 주소를 주세요" 라는 메시지를 로컬 네트워크 전체에 뿌립니다.
3. DHCP Server 인 `88:36:6C:33:FC:50` 라는 MAC 주소를 가진 라우터가 이 메시지를 받고 `8c:85:90:0c:e3:cc` 로 할당 가능한 ip 주소를 보내줍니다.<br>
   DHCP Server 가 로컬 네트워크 내에 그간 할당해준 ip 주소들을 관리하고 있기 때문에, 사용되지 않은 적절한 ip 주소를 할당해 줄 수 있습니다.
4. 이제 `8c:85:90:0c:e3:cc` 는 할당받은 ip를 사용하면 됩니다.

<br>
<br>
이 DHCP Server 로서의 기능을 하는 라우터를 이용해 라우터 하위에 있는 호스트들에 고정된 내부 ip를 줄 수도 있습니다.<br>
호스트의 MAC 주소만 알면 공유기 설정 페이지에서 원하는 ip로 내부 ip를 정해줄 수 있습니다.<br>
홈 서버를 운용할때, 내부 ip, 내부 포트를 정해둔 고정된 ip로 사용할 수 있는것이죠.<br>
<br>
<br>