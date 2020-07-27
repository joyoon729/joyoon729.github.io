---
title: Network 기초 - 6. hosts file
date: 2019-02-28 00:00:00
categories: Network
tags:
  - Network
header:
  teaser: /assets/images/home_server_teaser.jpg
excerpt: Home Server 를 구축하기 위한 기초 공부. 'hosts' 파일을 조작해 봅니다.
---

> ip 와 host


## ip
ip 는 이전 공부에서도 많이 봤었죠. 인터넷에 연결된 각 컴퓨터들에 대한 주소입니다.<br>
0부터 255까지 256개의 숫자를 4개 조합해 만듭니다.<br>
<pre>총 ip 수 : 256*256*256*256 = 4,294,967,296 개</pre><br>
```
ip 주소 예) 127.0.0.1
```
<br>
## host
![host](/assets/images/host.png)

<cite>[출처: 생활코딩 - WEB2-Domain Name System.2.IP주소와 hosts](https://opentutorials.org/course/3276/20296)</cite>
{: .text-right}

host 는 인터넷에 연결된 각 컴퓨터들을 부를때 쓰는 대명사입니다.<br>
두 개 이상의 host 들이 인터넷으로 연결되려면 서로 각각의 ip 주소를 알아야합니다.<br>
상대방에게 전화를 걸려면 전화번호를 알아야 하는것처럼요.<br>
<br>
이제 서로의 ip 주소를 알고 있다면, 인터넷을 통해 정보를 주고받을 수 있는데요.<br>
**하지만** 4,294,967,296 개 중의 하나일 ip 주소를 매번 기억하기란 어렵습니다.<br>
이 문제를 해결하기 위해 생겨난 것이 **Domain Name** 입니다.
<br>
<br>
<hr>

> DNS (Domain Name System)

도메인 이름은 어려운 ip주소를 대체합니다. 숫자로 이루어진 주소 대신에 어떠한 이름을 부여하는거죠.<br>
<br>
![domain server](/assets/images/dns_server.png)

<cite>[출처: 생활코딩 - WEB2-Domain Name System.1.수업소개](https://opentutorials.org/course/3276)</cite>
{: .text-right}

우리가 웹 브라우저에 `www.icann.org` 라고 입력한다면, OS 가 **DNS Server(Domain Name System Server)**에 *www.icann.org* 라는 도메인 이름이 있는지 물어보고, 있다면 해당 도메인 이름에 대응되는 ip 주소를 받아옵니다.<br>
이러한 방식으로 사용자는 복잡한 ip 주소를 외우는 대신에 도메인 이름만 기억하고도 인터넷 활동이 가능한 것이죠.<br>
<br>
<br>
<hr>

> hosts file

![hosts file](/assets/images/hosts_file.jpeg)

이번 포스트에서는 **DNS Server** 와 비슷한 일을 하는 **hosts** 파일에 대해 알아봅니다.<br>
hosts 파일은 쉽게말해 내 컴퓨터 안에 있는 DNS Server 라고 보면 됩니다.<br>
이 파일을 수정함으로써 내 컴퓨터 내부적으로 DNS Server 가 하는 일을 대신 기능하게 할 수 있습니다.<br>
물론 내 컴퓨터 안에서만 동작할 뿐이지만요.<br>
<br>
hosts 파일은 OS 별로 각기 다른 곳에 보관되고 있는데, [위키피디아 - hosts file](https://en.wikipedia.org/wiki/Hosts_(file))에 자세히 설명돼 있습니다. windows 10 의 경우엔 `windows\System32\drivers\etc\hosts` 에 저장돼 있다고 합니다.<br>
이 파일을 열어보면,<br>
![hosts file](/assets/images/hosts_file2.png)
이런게 보일텐데요.<br>
<br>
이 파일에 `ip주소 (공백) 도메인이름` 이라고 수정하는것만으로 도메인 이름으로 접속이 가능해집니다.
```
# 주석은 빼고 입력
93.184.216.34  www.example.com
``` 
<br>
<br>
