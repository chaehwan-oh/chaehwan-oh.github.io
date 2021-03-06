---
layout: post
subclass: post
title: "IP 주소와 서브넷 마스크"
date: 2019-10-14
tags: [computer-science, IP, subnet-mask]
excerpt: "IP 주소와 서브넷 마스크에 대한 내용입니다."
disqus: True
---

사내 AWS 교육을 들으며 CIDR 블록, NAT 등 얼핏 들어봤거나 어렴풋이 무엇인지는 알 것 같은데 제대로 설명할 수 없는 것들이 많다는 생각이 들었습니다. CIDR과 NAT에 대한 내용을 정리하기 앞서 선수지식으로 필요한 IP 주소와 서브넷에 대한 내용을 정리해보도록 하겠습니다.

    • IP 주소
    • 라우터의 인터페이스
    • IP 주소의 구성
    • IP 주소 체계의 클래스 분류
    • 서브넷 마스크
    • 서브넷 마스크의 역할
    • 서브넷 마스크의 성질

# IP 주소

IP 주소란 컴퓨터 네트워크에서 각 장치들을 인식하고 통신하기 위한 주소입니다. 편지를 보낼 때 필요한 집주소와 전화 통신에 사용하는 휴대폰 번호처럼 컴퓨터 네트워크에서 통신할 때 식별하는 데 사용되는 주소입니다.

그러므로 각 장치마다 IP 주소는 달라야 하며, 휴대폰 번호처럼 일정한 규칙을 가집니다.

## 1) IP 주소는 이진수 32자리로 구성됩니다.

```
0000 0000. 0000 0000. 0000 0000. 0000 0000
1111 1111. 1111 1111. 1111 1111. 1111 1111
```

각 자리에 0 또는 1이 오고 32자리이므로 2의 32승(4,294,967,296) 즉, 대략 42억 9천개 정도의 주소가 장치들을 식별하는 데 활용됩니다. 하지만 인터넷을 사용하는 사람이 많아지고 한 사람마다 사용하는 장치의 수가 많아지면서 IP 주소는 거의 고갈된 상태입니다.

## 2) 8자리씩 나누어 점을 찍어 십진수 방식으로 표기합니다.

1111 1111. 1111 1111. 1111 1111. 1111 1111 => 255.255.255.255

위와 같이 8자리마다 점을 찍어 사람들에게 익숙한 십진수로 변환하여 표기합니다. 변환된 주소마저 사람들에게는 익숙하지 않기 때문에 'www.naver.com' 와 같은 도메인 이름을 사용합니다.

# 라우터의 인터페이스

라우터는 데이터 전송의 단위인 패킷을 컴퓨터 네트워크 내에서 경로를 지정하여 전송해주는 장치입니다. 이 라우터에는 2가지 종류의 인터페이스(포트)가 있습니다. 하나는 이더넷 인터페이스, 또 하나는 시리얼 인터페이스입니다. 각 인터페이스에 대해 IP 주소를 부여합니다.

이더넷 인터페이스에는 내부 네트워크에 접속하기 위한 주소(LAN side Address)를 부여하고, 시리얼 인터페이스에는 인터넷에 접속하기 위한 주소(WAN side Address)를 부여합니다.

시리얼 인터페이스에 부여하는 주소의 경우 마음대로 부여할 수 없습니다. 접속하는 상대편 라우터(ISP 업체: 인터넷 제공업체)와 맞춰 주어야 합니다.
갑자기 웬 라우터인가 싶을 수 있지만 앞으로의 개념 이해를 위해 추가해보았습니다.

정리)
• 라우터의 인터페이스별로 IP 주소를 부여해준다.
• 인터페이스가 달라지면 다른 네트워크가 된다.
• 시리얼 인터페이스에 부여하는 IP 주소는 상대편 라우터와 주소를 맞춰 주어야 한다.

# IP 주소의 구성

IP 주소는 '네트워크 부분(Network Part)'와 '호스트 부분(Host Part)'로 나누어집니다. 여기서 네트워크는 하나의 브로드캐스트 영역(Broadcast Domain)을 의미합니다.

브로드캐스트 영역이란 라우터에 의해 구분된 네트워크입니다. 그리고 브로드캐스트(Broadcast)란 이 브로드캐스트 영역에 속한 모든 네트워크 장비들에게 보내는 통신입니다.

IP 주소의 네트워크 부분이 같다는 말은 브로드캐스트 영역이 같다는 말이고, 라우터를 거치지 않고도 통신이 가능하다는 것입니다. 비유하자면 마을 이장님이 방송하면 같은 마을에 있는 사람들은 모두 방송을 들을 수 있지만, 다른 마을에 있다면 우체국을 들려서 편지를 보내야 하는 것입니다.

호스트(Host) 부분은 각 PC 또는 장비를 식별하는 부분을 말합니다. 그러므로 하나의 네트워크에서 네트워크 부분은 모두 같아야 하고, 호스트 부분은 각 장치를 식별하기 위해 달라야 합니다. 비유하자면 같은 지역이라면 전화번호의 지역번호는 모두 같고, 전화번호의 나머지 부분은 모두 달라야 하는 것과 같습니다.

정리)
• IP 주소는 네트워크 부분과 호스트 부분으로 나누어진다.
• 네트워크 부분은 브로드캐스트 영역을 의미한다.
• 같은 네트워크라면 네트워크 부분은 같고 호스트 부분은 달라야 한다.

# IP 주소 체계의 클래스 분류

IP 주소는 NIC(Network Information Center)라는 기관에 의해 관리되고 할당됩니다. A, B, C, D, E 클래스로 나누어지고, 이 중 A, B, C 클래스를 나누는 기준은 네트워크의 크기입니다. NIC에서 IP를 할당할 조직의 네트워크 크기를 고려하여 A, B, 또는 C 클래스의 IP 주소를 할당해줍니다. 이 클래스에 따라 IP 주소의 네트워크 부분과 호스트 부분이 나누어집니다. 클래스 A 주소부터 하나씩 살펴보도록 하겠습니다.

클래스 A)

클래스 A는 32개의 이진수 중 맨 앞의 수가 항상 0으로 시작되는 네트워크를 말합니다.

```
0xxx xxxx. xxxx xxxx. xxxx xxxx. xxxx xxxx
```

그러므로 위와 같이 0으로 시작하고 다음 수부터는 0 또는 1 중 하나만 나오면 됩니다.

네트워크 주소의 범위:

0000 0000. 0000 0000. 0000 0000. 0000 0000
~ 1111 1111. 1111 1111. 1111 1111. 1111 1111
(0.0.0.0 ~ 127.255.255.255)

클래스 A의 중요한 규칙은 앞의 8비트가 네트워크 부분을 의미하고, 나머지 24 비트가 호스트 부분을 나타낸다는 것입니다. (네트워크 부분이 0, 127인 네트워크는 제외됩니다. 이건 약속입니다.)

클래스 A 네트워크의 특징을 정리하면 다음과 같습니다.

    • 1 ~ 126으로 시작하는 네트워크 부분
    • 클래스 A 네트워크가 가질 수 있는 호스트의 수: 2의 24승 - 2(16,777,214)개(모두 0인 경우는 네트워크 자체, 그리고 모두 1인 경우는 브로드캐스트 주소이기 때문에 제외합니다.)

클래스 B)

클래스 B는 32개의 이진수 중 맨 앞의 수가 항상 10으로 시작되는 네트워크를 말합니다.

```
10xx xxxx. xxxx xxxx. xxxx xxxx. xxxx xxxx
```

네트워크 주소의 범위:

1000 0000. 0000 0000. 0000 0000. 0000 0000
~ 1011 1111. 1111 1111. 1111 1111. 1111 1111
(128.0.0.0 ~ 191.255.255.255)

클래스 B의 경우 앞의 16비트가 네트워크 부분을 나타내고, 나머지 16 비트가 호스트 부분을 나타냅니다.

    • 128 ~ 191으로 시작하는 네트워크 부분
    • 클래스 B 네트워크가 가질 수 있는 호스트의 수: 2의 16승 - 2(65,534)개(모두 0인 경우는 네트워크 자체, 그리고 모두 1인 경우는 브로드캐스트 주소이기 때문에 제외합니다.)

클래스 C)

클래스 C는 32개의 이진수 중 맨 앞의 수가 항상 110으로 시작되는 네트워크를 말합니다.

네트워크 주소의 범위:

1100 0000. 0000 0000. 0000 0000. 0000 0000
~ 1101 1111. 1111 1111. 1111 1111. 1111 1111
(192.0.0.0 ~ 223.255.255.255)

클래스 C의 경우 앞의 24비트가 네트워크 부분을 나타내고, 나머지 8비트가 호스트 부분을 나타냅니다.

    • 192 ~ 223으로 시작하는 네트워크 부분
    • 클래스 C 네트워크가 가질 수 있는 호스트의 수: 2의 8승 - 2(254)개(모두 0인 경우는 네트워크 자체, 그리고 모두 1인 경우는 브로드캐스트 주소이기 때문에 제외합니다.)

클래스 D, E)

클래스 D, E의 주소의 경우 일반적으로 사용되는 주소는 아닙니다. 클래스 D는 멀티캐스트용으로 사용되는 주소, 클래스 E는 연구용으로 사용되는 주소입니다. (멀티 캐스트용 주소는 간단히 필요한 그룹에만 한꺼번에 데이터를 전송할 때 사용하는 주소입니다.)

# 서브넷 마스크

서브넷 마스크(subnet mask)란 주어진 네트워크 환경에 맞게 브로드캐스트 도메인(Broadcast domain)을 나누기 위해 사용하는 이진수 조합입니다.

앞서 살펴본 클래스 기반의 주소체계에서 IP 주소를 배정받으면 보통은 이 주소를 바로 사용하지 않습니다. 주어진 네트워크 환경에 맞게 적절히 사용해야 하기 때문입니다. 예를 들어 클래스 B에 해당하는 IP 주소를 할당받았다고 가정하겠습니다. 클래스 B에 해당하는 네트워크가 가질 수 있는 호스트의 수는 65,534(2의 16승 - 2)개입니다. 이렇게 큰 네트워크를 구성하면 브로드캐스트의 영향이 너무 크다는 문제점이 생깁니다.

앞서 살펴봤듯이 브로드캐스트는 같은 로컬 랜에 붙어있는 모든 네트워크 장비에게 보내는 통신입니다. 마을 이장님의 방송을 마을 사람들 모두가 들을 수 밖에 없는 것처럼 해당 호스트와 관계 없는 데이터도 CPU에 전달해 다른 일을 멈추고 처리해야 하므로 네트워크 내 호스트들의 CPU의 성능 저하가 발생합니다.

이러한 문제를 막기 위해서는 하나의 네트워크를 더 작은 네트워크들의 집합으로 나누어 주어야 합니다. 이렇게 브로드캐스트 도메인을 작게 나누어 주는 것을 '서브넷팅(subnetting)'이라고 합니다. 그리고 이렇게 서브넷(subnet)을 나누기 위해 사용하는 이진수 조합을 서브넷 마스크라고 합니다.

# 서브넷 마스크의 역할

서브넷 마스크의 역할은 IP 주소에서 어디까지가 네트워크 부분이고, 어디까지가 호스트 부분인지를 나타내는 것입니다. 서브넷 마스크를 보면 그 IP 주소의 네트워크 부분과 호스트 부분을 알 수 있습니다.

이 때 네트워크 부분은 서브넷 마스크가 이진수로 1인 부분이고, 호스트는 서브넷 마스크가 이진수로 0인 부분입니다.

모든 IP 주소에는 서브넷 마스크가 붙습니다. 클래스 C에 해당하는 주소이더라도 그 주소를 나눈 것인지 나누지 않은 것인지 식별하기 위해 필요합니다. 서브넷을 나누지 않더라도 서브넷 마스크는 존재합니다. 이를 '디폴트 서브넷 마스크(default subnet mask)'라고 합니다. 클래스별 디폴트 서브넷 마스크는 아래와 같습니다.

클래스 A 디폴트 서브넷 마스크: 255.0.0.0
클래스 B 디폴트 서브넷 마스크: 255.255.0.0
클래스 C 디폴트 서브넷 마스크: 255.255.255.0

그럼 아래 예제들과 함께 서브넷 마스크에 대해 살펴보겠습니다.

ex)
IP 주소: 210.100.100.1
서브넷 마스크: 255.255.255.0

1101 0010. 0110 0100. 0110 0100. 0000 0001(210.100.100.1) => IP 주소
1111 1111. 1111 1111. 1111 1111. 0000 0000(255.255.255.0) => 서브넷 마스크
1101 0010. 0110 0100. 0110 0100. 0000 0000(210.100.100.0) => 서브넷 네트워크

IP 주소와 서브넷 마스크를 논리 AND 연산을 취해주면 됩니다. (양쪽이 모두 1인 경우만 1이 됩니다.) 위 예제의 경우 앞 24비트까지가 네트워크 부분, 마지막 8비트가 호스트 부분이라는 것을 알 수 있습니다. 클래스 C의 기본 성격과 같으므로 디폴트 서브넷 마스크가 됩니다.

ex2)
IP 주소: 150.150.100.1
서브넷 마스크: 255.255.255.0

위 150.150.100.1 는 클래스 B에 해당하는 IP 주소입니다. 디폴트 서브넷 마스크는 255.255.0.0 이지만 서브넷 구성을 위해 255.255.255.0을 서브넷 마스크로 사용했다고 가정해보겠습니다.

1001 0110. 1001 0110. 0110 0100. 0000 0001(150.150.100.1) => IP 주소
1111 1111. 1111 1111. 1111 1111. 0000 0000(255.255.255.0) => 서브넷 마스크
1001 0110. 1001 0110. 0110 0100. 0000 0000(150.150.100.0) => 서브넷 네트워크

서브넷 네트워크는 150.150.100.0이 됩니다. 즉, 앞 24비트가 네트워크 부분으로 사용되고 나머지 8비트가 호스트 부분으로 사용되게 됩니다. 클래스 B 주소이지만 클래스 C의 주소처럼 사용되는 것입니다. 150.150.100.1과 150.150.200.1은 다른 브로드 캐스트 영역을 가지게 되는 것입니다.

# 서브넷 마스크의 성질

마지막으로 서브넷 마스크의 성질에 대해 정리해보겠습니다.

    • 서로 나누어진 서브넷(서브넷 마스크로 만들어진 네트워크)은 라우터를 통해서만 통신이 가능하다.
    •  서브넷 마스크는 이진수로 1이 앞에서부터 연속적으로 나오는 수이어야 한다. ex) 255.255.255.10 (X) 255.255.255.252(O)
    • 서브넷 마스크의 적용범위는 항상 네트워크의 호스트 부분이다.

'후니의 쉽게 쓴 CISCO 네트워킹' 책을 읽고 참고하여 정리해보았습니다. 이 글에 이어 CIDR, NAT에 대한 내용 또 앞서 작성했던 HTTPS에 대한 내용 역시 추가적으로 정리해나가도록 하겠습니다. 감사합니다.
