---
layout: post
subclass: post
title: "컴퓨터 네트워크 Chapter1. 컴퓨터 네트워크와 인터넷"
date: 2018-10-01 00:00:01
tags: [dudaji, computer-science, computer_networking]
excerpt: "컴퓨터 네트워크 Chapter1. 컴퓨터 네트워크와 인터넷에 대한 정리입니다."
disqus: True
---

## 1장 컴퓨터 네트워크와 인터넷의 개념 구조도

![network_ch1_recap](/images/computer_networking/network_ch1_recap.jpg)

## 인터넷이란 무엇인가?

인터넷 : 전세계적으로 수십 억개의 컴퓨팅 장치를 연결하는 컴퓨터 네트워크

이 인터넷은 구성요소별로 살펴볼 수 있다.

### 1. Host(End System)

가장 가장자리에 있는 것이 'host(end system)'이다. 컴퓨터에서 application
program을 호스팅하고 있기 때문에 host라고 부르기도 하고, 네트워크의
가장자리에 있기 때문에 end system이라고도 부른다.

### 2. Router

네트워크의 중앙에는 Router라는 특수한 장비가 있다. 사용자 메세지가 목적지를
찾아갈 수 있도록 하는 역할을 한다.

### 3. link

이 Router와 Router 또는 host와 router을 연결해주는 것이 link다. 실제 물리적인
회선을 말한다.

### 4. protocol

이러한 구성요소들이 인터넷에서 정보를 송수신할 수 있도록 제어하는 역할을 하는
것이 '프로토콜(protocol)'이다.

같은 방식으로 이야기해야 통신이 가능하기 때문에 protocol은 표준화가 매우 중요하다.
참고로 IETF(Internet Engineering Task Force)라는 기관에서 RFC(Request for Comments)
라는 표준안을 발표해서 표준화를 지원한다.

"protocols define format, order of messages sent and received among network
entities, and actions taken on message transmission, receipt."

위의 말처럼 프로토콜은 형식, 순서 그리고 메세지를 받았을 때의 동작을 정의하고 있다.

#### 참고) 웹 페이지를 요청할 때 프로토콜의 역할은?

예를 들어 웹 서버에 웹 페이지를 요청한다면?
프로토콜이 정의한 형식대로 요청해야 상대방이 웹 페이지를 요청하는 메세지라는 것을
알 수 있고, 어디서부터 어디까지가 어떤 정보라는 것을 알고 parsing하고 해독하는 것이
가능하다.

순서는 요청이 가면 응답이 뒤따르도록 정의하는 것이다.

마지막으로 동작은 웹 서버가 요청을 받으면 웹 서버의 하드디스크에서 필요로 하는 정보를
찾아서 응답 메세지에 붙여서 응답하는 등의 동작을 프로토콜에서 정의하는 것이다.

**정리하면,
인터넷은 네트워크의 네트워크이고,
인터넷은 호스트, 라우터, 링크, 프로토콜 등의 구성된다.**

**이 인터넷은 크게 두 부분으로 나눌 수 있다. 'network edge'와 'network core'다.**

- network edge(end systems, access networks, links)
- network core(packet switching, circuit switching, network structure)

## 네트워크의 가장자리(network edge)

network edge에는 end system이 있다. 이 end system은 access network에 연결된다.

access network : end system을 다른 먼 거리의 end system과 연결되는 경로상에 있는
첫 번째 라우터에 연결하는 네트워크

간단히 말해 end system이 network에 연결되도록 하는 역할을 하는 것이 access network다.

**여러 종류의 access network와 access network가 사용되는 환경을 살펴보자.**

**환경을 살펴보기 전에 알아보아야 하는 것이 access network들을 특징 짓는 변수들이다.**

### access network를 결정 짓는 parameter

#### 1. bandwidth

bandwidth(대역폭) : 단위 시간당 실어나를 수 있는 bit의 수 ex) Mbps, Gbps

#### 2. shared vs dedicated

shared 라면 그 access network의 bandwidth가 100Mbps라면 그 대역폭을 공유해서
사용하는 것이다.

dedicated 라면 독점적으로 사용한다.

보안 상에서도 차이가 있는데 Wi-fi의 경우 shared access network인데 shared이기
때문에 보안이 매우 취약하다.

### DSL(Digital Subscriber Line)[전화회사에서 제공하는 access network]

![DSL](/images/computer_networking/DSL.png)

전화회사의 central office와 집이 연결되는데 연결하는 선이 옆집과 공유되는 것이
아니라 별도의 선으로 되어 있다. 그래서 전화회사에서 제공하는 access network는
shared가 아니라 dedicated다.

전화회사에서 제공하는 서비스로 인터넷에 연결되고 DSL modem을 가지고 있다.
컴퓨터는 DSL modem에 연결된다.

그리고 central office 내부에 있는 DSLAM(DSL access multiplexer)가 voice와
data를 구분해준다.

인터넷 사용 시 업로드와 다운로드를 하는데 대개 집에서는 데이터 소비자로서 활동하기
때문에 업로드보다 다운로드하는 양이 많다. 그래서 DSL 표준은 12 Mbps 다운 스트림과
1.8 Mbps 의 업스트림을 정의하고 있다. 다운스트림과 업스트림의 전송속도가 다르기 때문에
접속이 비대칭(asymmetric)이라고 한다.

### 케이블 인터넷 접속(cable internet access)[케이블 회사에서 제공하는 access network]

![cable_internet_access](/images/computer_networking/cable_internet_access.png)

케이블 네트워크 회사에서 제공하는 access network에 연결될 때는 집과 cable headend가
연결된다. 여기서 cable headend가 앞서 살펴본 전화회사로 보면 central office다.

케이블 네트워크 회사에서 서비스를 제공할 때는 케이블 모뎀이 제공되고 컴퓨터는 모뎀에 연결된다.

또, 전화회사의 DSLAM처럼 CMTS라는 멀티플렉싱 장비가 있어서 data를 구분해준다.

전화회사 서비스와의 큰 차이점은 shared 서비스라는 것이다. cable 방송은 일반적으로 broadcast다.
전화회사 서비스에서는 별도의 회선을 가지지만 케이블 회사에서 제공하는 서비스는 예를 들면
아파트 한 통에 있는 집들이 한 회선을 공유하는 경우가 많다.

케이블 회사에서 제공하는 서비스에서는 케이블로 연결하는 방식을 살펴볼 필요가 있는데
HFC(Hybrid fiber coax) 방식이다. HFC 방식은 fiber라는 케이블과 coax라는 케이블을
함께 사용하는 방식이라는 말이다.

앞서 살펴본 cable headend는 하나만 있는 것이 아니라 여러 cable headend가 계층적으로 연결되어
있다. 그래서 cable headend와 가정들이 연결되는 쪽보다는 가정들에 대량의 데이터를 공급하고
받아내는 cable headend와 cable headend가 연결되는 쪽이 더 큰 대역폭이 요구된다. 그래서
더 큰 대역폭을 지원하는 케이블인 fiber을 cable headend간의 연결에 사용하고, cable headend와
가정들을 연결하는데 coax를 사용하는 방식이 HFC 방식이다.

이 경우도 upstream과 downstream은 비대칭적이다. upstream은 30Mbps, downstream은 2Mbps 정도를
지원한다. 하지만 upstream의 대역폭이 크더라도 shared 방식이라는 것을 주의해야 한다.

**그런데 가정에서는 Desktop 하나만 연결되어 있는 것은 아니다. 그래서 살펴볼 것이 home network다.**

### home network

![home_network](/images/computer_networking/home_network.png)

가정 안에서는 Wi-fi를 통해 연결된다. 이를 통해 home network를 구성할 수 있다.

home network의 core에는 router가 있다. 이 router에 Desktop은 직접 연결되어 있고,
Wi-fi access point가 연결되어 있을 수 있다. 결국 여러 end system을 묶어서 cable 회사 또는
전화 회사의 network에 연결하는 역할을 하는 것이 router다.

**이 때까지 가정이 access network에 연결되는지를 살펴봤다. 그렇다면 회사나 학교는 어떨까?**

### 회사나 학교에서의 네트워크 연결

![ethernet_internet_access](/images/computer_networking/ethernet_internet_access.png)

회사나 학교와 같은 기관들은 대개 ethernet switch들을 통해 연결된다. 기관 전체에 router를
두고 그 router에 ethernet switch가 여러 개 연결되는 방식이다. ethernet switch는 또 다시
하나의 부서에 할당되는 방식이다. 그래서 그 ethernet switch에 여러 개의 컴퓨터와 액세스 포인트가
연결된다. 웹 서버들이 연결될 수도 있다.

기관에서의 네트워크 연결과 가정에서의 네트워크 연결의 차이점은 가정에서는 대개 전화회사나 케이블 회사를
통해 네트워크에 연결되고 기관에서의 네트워크 연결은 바로 ISP(Internet Service Provider)의 라우터에
연결된다는 것이다. 그리고 가정의 경우 대개 케이블 회사를 통해 shared 방식으로 연결되지만 기관의 경우
dedicated 방식으로 연결된다는 것이다.

**추가적으로 살펴볼 것은 물리적으로 연결되는 방식도 가능하지만 무선 연결도 가능하다는 것이다.**

### Wireless access networks

Wifi, 3G, 5G, LTE 등을 생각해볼 수 있다. 모두 shared 방식이고 wireless이지만 차이점이 있다.

#### wireless LANs

먼저, Wi-fi와 같은 네트워크를 wireless LANs
이라고 한다. Wi-fi는 주로 건물 안에서만 사용이 가능하다. 외부로 나가면 LTE 등과 같은 다른
네트워크를 사용해야 한다. 하지만 Wi-fi은 대역폭이 상당히 높다는 장점이 있다.

참고) Wi-fi protocol

Wi-fi protocol은 다양한데 그 중 살펴볼 수 있는 것은 다음과 같다.

- 802.11b : 11Mbps 제공
- 802.11g : 54Mbps 제공

#### wide area wireless access

Wi-fi와 같은 네트워크는 주로 건물 안에서만 사용 가능하기 때문에 바깥에서 사용하는 네트워크가
wide area wireless access다. cellular network라고 한다. 3G, 5G, LTE 등이 있다.
3G는 10Mbps를 넘어가지 못하지만 LTE의 경우 100Mbps까지 가능하다. (하지만 cellular network
자체가 shared여서 100Mbps를 모두 제공받을 수 있는 것은 아니다.)

**host가 어떻게 연결될 수 있는지를 살펴봤다. 이어서 살펴볼 것은 host의 역할이다.**

### Host의 역할

Host는 왜 Host라고 부를까? application program을 hosting하고 있기 때문이다. 그래서 이 host에서는
사용자의 Application message가 발생하게 된다. Host에서는 이 message를 Access network로 내보는 역할을
한다. 그런데 message를 내보기 전에 packet이라는 chunk로 message를 잘라준다. (이유는 나중에 살펴본다.)

이 때 패킷의 길이를 L bit라고 하고 access network에서의 전송률(transmission rate)이 R일 때 Packet 하나를
host에서 내보는 시간은 L/R seconds다.

```
L(bits)/R(bits/sec) = Packet transmission delay = time needed to transmit L-bit packet into link
```

L bits 인데 sec마다 R bits씩 보낼 수 있으니 L/R이다.

**요점은 Host의 주된 역할은 Application message를 Packet으로 잘라서 access network로 내보낸다는 것이다.**

**다음으로 살펴볼 것은 Host를 연결하는 Link다.**

### Link(Physical Media)

Link는 크게 2가지 종류로 나누어진다. guided media와 unguided media다.

- guided media : 물리적인 wire를 사용하는 Link ex) copper, fiber, coax
- unguided media : 물리적인 wire를 사용하지 않는 Link ex) radio

#### guided media

guided media를 먼저 살펴보면 copper, fiber, coax가 있다.

copper의 다른 이름은 twisted pair(TP)인데 Ethernet cable에 주로 사용된다.

그리고 copper보다 조금 더 안정적이고 지원하는 대역폭이 높은 것이 coax다.
coax를 다른 이름으로 broadband link라고도 한다. 말그대로 대역폭이 넓은 링크라는
것이다.

마지막으로 살펴볼 것이 fiber이다. coax와 twisted pair(copper)는 전기 신호(electronic
signal)을 전달하지만 fiber은 light pulse를 전달한다는 것이다. fiber의 장점은 이 전기 신호가
아니라 light pulse를 사용한다는 것에서 기인한다.

- light pulse를 사용하기 때문에 transmission rate가 상당히 높다. (10Gbps ~ 100Gbps)
- light pulse를 사용하기 때문에 electronic noise의 영향에 강하다. 즉, 영향을 거의 받지
  않는다. 먼 거리를 이동해도 약해지지 않고 error rate도 상당히 낮다.

이 fiber와 coax를 사용하는 것이 앞서 살펴본 HFC 방식이다.

#### unguided media

다음으로 살펴볼 것이 unguided media다. 먼저, unguided media인 radio의 장단점을 살펴보자.

unguided media인 radio의 장점

- 물리적인 wire가 필요 없다.

unguided media인 radio의 단점

- reflection
  가다가 장애물을 만나면 signal이 튈 수 있다. 그러면 신호가 약해지거나 전달이 실패할 수 있다.

- interference
  electronic magnetic spectrum에 의해 전달되기 때문에 주변의 noise에 상당히 민감하다. 또, radio
  signal끼리의 간섭이 있을 수 있다.

- terrestrial microwave
- LAN ex) Wi-fi
- wide-area wireless network
- satellite Radio Channels

radio link의 종류는 다음과 같다.

- Terrestrial radio channels

45Mbps까지 지원 가능하다. 크게 2곳에 사용되는데 LAN과 wide wireless network다.
wireless LAN인 Wi-fi에 사용되고 coverage area는 300M 정도가 된다.
(802.11b : 11Mbps / 802.11g : 54Mbps)

wide wireless network인 cellular network에도 사용되는데 상대적으로 대역폭이 낮다.
3G는 굉장히 대역폭이 낮았지만 LTE는 10Mbps까지 향상되었다.

- Satellite Radio Channels

Satellite Radio Channels의 종류는 다음과 같다.

- geostationary satellites
- low-earth orbiting satellites

**이제 이어서 network core에 대해 살펴보도록 하자.**

## 네트워크의 중심(network core)

network의 core에는 router와 switch들이 복잡하게 연결되어 있다.

이 router와 switch들의 목적은 user의 application message를
source로부터 destination으로 전달해주는 것이다. 그 전달 방식에는
크게 2가지 방식이 있다.

- circuit switching
- packet switching

### Circuit Switching

circuit switching은 telephone networks 즉 전화 네트워크에서 주로
사용되던 방법이다.

![circuit_switching](/images/computer_networking/circuit_switching.png)

Circuit Switching의 특징

- user message가 전달되기 전에 반드시 Call이 있어야 한다.

user가 Call을 하면 call & setup 과정을 거친다. 이 call & setup에서 해주는 것은 2가지다.

- source ~ destination의 경로 결정(어떤 라우터를 경유할 것인지)
- 경로 상의 resource을 예약(reserve)한다.

이렇게 setup 과정을 거치고 난 다음 source쪽에서 user message를 bit로 쭉 밀어넣으면 마치
파이프처럼 데이터를 실어나를 수 있다.

그런데 circuit switching에서 굉장히 중요한 것은 네트워크의 자원을 분할해두는 것이 필요하다는
것이다.

분할해두지 않으면 한 사용자라도 이 링크를 사용하면 그 자원이 한 사용자에게 예약이 되는 것이기
때문에 아무도 즉 다른 사용자들은 사용할 수 없게 되는 것이다. 그래서 분할해두고 분할된 덩어리를
그 사용자에게 제공해주는 것이다.

그래서 Circuit switching 방식에서는 네트워크의 자원을 분할해두는 방법이 필요하다. 크게 2가지다.

- FDM(Frequency Division Multiplexing)
- TDM(Time Division Multiplexing)

![FDM_TDM](/images/computer_networking/FDM_TDM.png)

간단하게 말하면 FDM은 별도의 Frequency를 user에게 할당하는 방법이고, TDM은 시간을 user에게 할당하는
방식이다.

그런데 이 Circuit Switching은 전화 네트워크에서는 적합했지만 인터넷과 같은 데이터 네트워크에서는
불합리한 방식이 되었다. 이유는 전화와 인터넷의 차이를 생각하면 된다. 전화는 call하고 받으면 쭉 통화하지만
인터넷은 요청이 있을 때만 데이터를 왕창 쏟아내는 방식이기 때문이다.

**전화 네트워크는 Frequency와 같은 일정 부분을 user에게 할당하는 방식이기 때문에 데이터가 송신되고 있지 않아도
다른 user와 공유될 수 없기 때문에 낭비된다. 그래서 데이터 네트워크에서는 굉장히 비효율적이다.
그래서 나온 것이 Packet Switching이다.**

### Packet Switching

Packet Switching의 주요 개념은 3가지다.

- no call & setup
- no resource reservation(필요할 때마다 resource 사용)
- 하나의 Data가 link(bandwidth)를 잡고 전송되는 방식

![packet_switching](/images/computer_networking/packet_switching.png)

A와 B 중 먼저 도착한 데이터가 link를 잡고 다음 router로 전송되는 방식이다.
("each packet transmitted at full link capacity")

**여기서 문제는 하나의 user message가 link를 잡고 있는 시간이 굉장히 길다면 자원을
효율적으로 사용할 수 없다는 것이다. 그래서 packet switching 방식에서는 message를
packet으로 나누는 작업이 필요하다. packet의 크기가 일정하게 되면 link를 잡고 있는
시간도 일정해지기 때문이다.**

circuit switching의 경우 source에서 circuit setup을 한 후에는 user message를 밀어넣으면
됐다. 하지만 packet switching에서는 packet을 link를 통해 내보낼 때 circuit이 setup되어 있지 않다.
그러므로 어디로 가야하는지를 모른다.

**그래서 하나 더 해주어야 하는 것이 packet에 목적지 주소를 명시해주어야 한다는 것이다.**

**그렇게 되면 packet에 명시된 목적지 주소를 통해 다음 라우터가 결정된다. 그래서 라우터는
packet 하나를 다 받아야 packet 정보를 parsing해서 다음 목적지를 결정할 수 있다. 그래서 router는
packet 하나가 모두 전달될 때까지는 받는 작업만 하고 있어야 한다. packet을 받으면서 동시에 내보낼
수는 없다. 그래서 store & forward 방식으로 전송한다.**

#### Store and Forward

\*\* 이제 store and forward 방식을 사용하므로 발생하는 일을 생각해보도록 하자.

![packet_switching](/images/computer_networking/packet_switching.png)

패킷마다의 L bits이고 Link의 대역폭이 Rbps라고 가정해보자. 이 때 Link를 통과하는데 L/R second가
걸린다. L/R second가 지나면 라우터에 정보가 다 도착하고 그 데이터를 라우터는 store해야 한다. **
이유는 내보려는 링크가 다른 링크에서 들어오는 데이터에 의해 사용중일 수도 있기 때문이다. 그래서
store해야 한다.** 비어 있으면 L/R second 시간이 걸려 다시 정보를 송신할 수 있다.

ex) 7.5 / 1.5 = 5(sec) [end to end delay는 최소 2 * L/R(10초)]

**Packet Switching은 store & forward 방식이므로 다음과 같은 문제가 발생할 수 있다. 바로
'Queueing Delay와 Loss'다.**

#### Queueing Delay and Loss

대기해야 하기 때문에 packet switching은 Queueing Delay가 발생할 수 있다. 링크로 내보내는
속도보다 데이터가 흘러들어오는 속도가 더 빠르다면? buffer에 대기하게 된다. 이유는 resource를
reserve해두지 않기 때문이다. 예측할 수 없다.

그런데 computer system에서의 buffer는 크기가 유한하다. 그래서 buffer가 다 찰 수 있는데
이 때 데이터를 보내면 packet loss가 발생할 수 있다. queueing delay가 심해져 결국 loss까지
까지 발생하는 상황을 'congestion'이라고 한다.

resource를 reserve하지 않기 때문에 자원을 효율적으로 공유할 수 있지만 이 때문에 packet loss
와 queueing delay가 발생할 수 있는 것이다.

**그렇다면 Packet Switching과 circuit Switching을 resource sharing 관점에서 비교해도록 하자.**

### Packet Switching VS Circuit Switching : Resource Sharing

상황)

- 1Mbps link
- each user : 100kb/s when "active"
  (active 10% of time)

만약 10명의 user가 동시에 사용하려고 하면 10 \* 100kb/s 로 1Mb/s가 된다. 하지만 user들은 동시에
active하지 않다. 이 때 circuit switching은 최대 10명을 수용하는 것이 가능하다.

하지만 Packet Switching은 35명의 사용자여도 10명이 동시에 active일 가능성은 0.0004 밖에 되지
않는다.

하지만 packet switching이 무조건 좋은 것은 아니다. congestion의 가능성이 있기 때문이다. 특히
오디오/비디오 데이터의 경우 delay가 굉장히 중요하다. 하지만 packet switching은 queueing delay를
예측할 수 없기 때문에 delay 보장이 힘들다.

대신 resource sharing이 효율적이고, no call & setup이므로 더욱 단순하고, 오버헤드 없이 전송이
가능하다.

**다음은 network core의 구조에 대해서 살펴보도록 하자.**

### Network Core의 구조

하나의 Host에서 반대편 host로 정보를 송신하려면 access network간에 연결이 되어 있어야 한다.
한 가지 방법은 access network 각각을 모두 연결하는 방법이다. 만약 이렇게 되면 O(n^2)의 연결이
되고 굉장히 확장성이 낮다. (n개의 access network가 있을 때 하나의 access network를 나머지 n-1개의
access network와 연결하는 작업을 모든 access network에 대해 해주어야 하기 때문이다.)

#### Global ISP

그래서 중앙에 ISP를 두고 연결하는 방식을 택한다. ISP를 통해 하나의 host에서 다른 host로 정보를 송수신할
수 있게 된다. 그런데 ISP는 사업자이기 때문에 돈을 받는다. (전화회사나 케이블 회사는 각 가정에 인터넷을
연결해주고 ISP에게도 돈을 낸다.)

#### peering link & IXP

ISP가 사업자이기 때문에 하나의 ISP가 독점하는 것이 아니라 여러 ISP가 경쟁한다. 그리고 하나의 ISP가 모든
네트워크를 감당하기란 쉽지 않다. 그래서 여러 ISP가 서비스를 공급하는 형태가 된다.

여기서 문제가 발생하는데 같은 ISP에 연결된 Host는 통신이 가능하지만 ISP가 다르면 통신할 수 없다는 것이다.
그래서 서로 peering link를 가진다.

또, ISP를 연결해주는 것이 사업이 되는데 이 일을 하는 것이 IXP다.

#### regional ISP & multi-tire hierarchy

매우 방대한 ISP라도 모든 지역을 커버하기란 힘들기 때문에 regional ISP가 생겨나게 된다. 이 regional ISP도
계층적인 구조를 띄게 된다. (multi-tire hierarchy)

#### Content Provider Networks

최근에 추가된 관점이 또 하나 있는데 바로 Content Provider Networks다.
예를 들어 Google은 전세계에 데이터 센터를 보유하고 있고 Data Center 각각을
연결시키고 또 데이터 센터와 User를 연결해야 한다. 문제는 ISP를 이용하면 막대한 비용을
지불해야 한다는 것이다. 그래서 대신 자체 네트워크를 구성한 것이 content provider networks다.

그래도 최소 하나 이상의 first tire ISP와의 연결은 필요하다. 대신 연결 비용을 상당히 줄일 수 있다.

![interconnection_of_ISP](/images/computer_networking/interconnection_of_ISP.png)

network core의 구조를 정리하면 위의 그림과 같다.

**network edge와 network core, 큰 그림을 살펴봤다. 전체 네트워크에서의 성능 지표다.**

## 네트워크에서의 성능 지표

network의 성능 지표에서 중요한 것은 delay, loss, throughput이다. 먼저 delay를 살펴보도록 하자.

### Delay

하나의 패킷이 하나의 hop을 지나가는데 있어 겪게 되는 delay는 4가지 종류가 있다.

![nodal_delay](/images/computer_networking/nodal_delay.png)

공식은 다음과 같다.

![nodal_delay_formula](/images/computer_networking/nodal_delay_formula.png)

하나씩 살펴보자.

- Processing Delay
  - check bit errors : 하나의 hop을 잘 건너왔는지 검사한다.
  - determine output link : 어느 링크로 보내줄 것인지를 결정한다.
  - typically < msec : 일반적으로 밀리세컨드보다 작다.
- Queueing Delay
  - outgoing link가 당장 available하지 않을 수 있기 때문에 queue에 들어간다.
  - 라우터의 congestion level에 따라 결정된다.
  - Processing Delay는 고정적이지만 Queueing Delay는 가변적이다.
- Transmission Delay
  - queue에서 link로 밀어넣는 시간
  - L(packet length) / R(link bandwidth) -> L/R
- Propagation Delay
  - d : length of physical link
  - s : propagation speed in medium
  - Propagation Delay : d/s

### Transmission Delay VS Propagation Delay

Transmission Delay와 Propagation Delay는 다르다. 먼저 Transmission Delay는 라우터에서 패킷을
링크로 밀어내는데 걸리는 시간을 말한다. 그러므로 이는 패킷의 길이와 대역폭에 의해 결정된다.
링크의 길이와는 상관이 없다.

반면, Propagation Delay는 하나의 라우터에서 다음 라우터로 옮겨가는 데 걸리는 시간이기 때문에
두 라우터의 거리에 영향을 받는다.

**위의 지연 중 Queueing Delay에 대해 조금 더 자세히 살펴보도록 하자.**

### Queueing Delay

Queueing Delay가 발생하는 이유는 outgoing link로 내보내는 속도보다 traffic이 유입되는 속도가 더욱 빠르기
때문이다.

조금 더 자세히 살펴보면
R : link bandwidth(bps)
L : packet length(bits)
a : average packet arrival rate

L \* a : 단위 시간당 유입되는 traffic의 양

(L \* a)/R : traffic intensity

(L _ a)/R == 1 : 유입되는 속도와 내보내는 속도가 같다.
(L _ a)/R > 1 : 계속 쌓이는데 service를 받지 못하는 상황이 된다.

![traffic_intensity](/images/computer_networking/traffic_intensity.png)

위 그림을 보면 1에 가기도 전에 0.7쯤 되었을 때부터 Delay가 가파르게 증가한다.
이유는 a에 있다. a는 average packet arrival rate다. 일시적으로는 L \* a가 R에
비해 훨씬 클 수 있다. 그래서 이럴 때는 잠시동안에 queue에 packet이 엄청 쌓이게 된다.

### Packet Loss

두 번째로 살펴볼 지표는 Packet Loss다. queue size는 유한할 수 밖에 없다. 그래서 경우에
따라 queue가 꽉찰 수 있다. 그래서 loss가 발생하는데 loss된 데이터를 전송하기 위해서는
재전송해야 한다. 이 경우 사용자 입장에서는 Delay가 매우 길어지게 되고, 네트워크 입장에서는
resource를 낭비하게 된다.

### Throughput

세 번째 지표는 Throughput이다. 단위시간당 처리량을 말한다. 네트워크에서의 throughput은
단위시간에 source로부터 destination까지 송신한 traffic의 양을 의미한다.

![throughput](/images/computer_networking/throughput.png)

(Rc가 Rs보다 대역폭이 크다.)

위 그림에서 throughput은 Rs와 Rc중 무엇에 의해 결정될까? Rs에 의해 결정된다.
위의 과정을 파이프라이닝으로 볼 수 있다. Rc bits가 송신되는 동안 Rs bits는 전송되고 있다.
결국 이들을 파이프라이닝으로 보아 더 가는 pipe에 맞추어진다는 것을 알 수 있다.

(Rs < Rc -> Rs / Rs > Rc -> Rc)

Source에서 Destination까지 송신하는데 throughput은 결국 가장 대역폭이 낮은 link에 의해
결정된다. 그리고 이런 link를 bottleneck link라고 한다.

![throughput_scenario](/images/computer_networking/throughput_scenario.png)

참고로 core보다는 edge 쪽에서 bottleneck이 발생한다. core는 일부러 bandwidth를 크게 잡기 때문이다.

**네트워크의 큰 그림과 성능 지표를 살펴봤다. 지금부터 살펴볼 것은 송수신을 제어하는 프로토콜이다.**

## 프로토콜(Protocol)

**앞에서 프로토콜을 간단하게 살펴봤는데 프로토콜에 대해 조금 더 자세히 살펴보면 프로토콜은 layering 구조를
가지고 있다. 그 이유는 Protocol이 매우 복잡한 시스템을 가지고 있기 때문이다.**

layering을 했을 때 장단점이 있는데 한 번 살펴보자.

layering의 장점

- 복잡한 시스템의 각 부분을 식별하고 그들간의 관계를 정의하는 것이 매우 용이해진다.
- modularization을 하면 maintenance(유지보수)와 updating(수정)이 매우 용이해진다.
  (큰 프로그램을 하나의 함수로 구현하면 유지보수가 힘든 것과 같다.)

layering의 단점

- layer 각각은 목적이 있다. 고유 목적 때문에 어떤 기능을 실행하는 데 중복이 생길 수 있다.
  ex) layer A : source와 destination간의 송신여부 체크
  layer B : 하나의 hop 사이에서의 송신여부 체크
- 한 layer가 optimal하게 동작하기 위해서는 다른 layer에서의 정보를 필요로 할 수 있다.
  그런데 layer를 엄격하게 구분하면 layer간 통신이 있어야만 필요한 정보를 가지고 올 수 있다.
  이 때 정보를 가져오는 오버헤드가 발생한다. 그리고 종종 다른 layer의 정보를 활용하지 못하는
  상황이 생길 수도 있다.

단점이 이렇게 있지만 현재 인터넷 프로토콜은 계층 구조를 따르고 있다.
인터넷은 5개의 protocol layer로 구성되어 있다.

![protocol_stack](/images/computer_networking/protocol_stack.png)

### application protocol

application protocol의 목적은 network application program을 지원하는 것이다.

application protocol의 하나인 HTTP는 웹 애플리케이션이 실행되기 위해 웹 페이지를 요청하는 메세지는
어떤 형태로 만들어졌고, 어떤 방식으로 전달해야 하는지 또 웹 서버는 어떻게 응답해야 하는지 등을
정의하고 있는 것이다.

또, SMTP라는 프로토콜은 이메일 전송을 위해 사용되고, FTP는 파일 전송을 위해 사용된다. 결국 역할은
웹 애플리케이션에서 유저가 발생시킨 데이터를 캡슐화해서 어떻게 메세지로 만드는가?이다.

### transport protocol

application 계층에서 user message를 만들어내고 나면 transport 계층에다가 process to process delivery를
부탁한다. 그래서 transport layer의 역할은 process to process delivery다.

그런데 process to process delivery가 가능하려면 host to host delivery가 선행되어야 한다. 그래서 transport
layer는 host to host delivery를 진행하기 위해 network 계층에 이를 부탁한다.

### network protocol

network protocol : routing of datgrams from source to destination

이제 네트워크 계층에서 host to host delivery를 위해 길 찾기를 하게 되는 것이다. hop by hop으로 네트워크 노드를
거쳐 간다. 여기서 한 hop을 넘어가는 것은 link 계층에 부탁한다.

### link protocol

link : data transfer between neighboring network elements

링크 계층에서는 하나의 hop을 건너가기 위해 physical medium에다가 bit를 밀어넣는다. 그 작업을 해주는 것이 physical
layer다.

### physical protocol

physical layer : to move the individual bits within the frame from one node to the next.

**정리하면 인터넷 프로토콜은 5계층으로 나누어진다. 각 계층은 고유한 목적을 가진다. 그리고 각 계층은 그 고유한 목적을
달성하기 위해 바로 아래 계층을 이용해 목적을 달성한다.**

### Source에서 Destination까지의 전달되는 과정

![source_to_destination](/images/computer_networking/source_to_destination.png)

- end system만이 protocol의 whole stack을 가진다.
- 각 프로토콜의 계층에서는 자신의 기능을 수행하기 위해 header를 붙인다. header는 자신의 peer가 읽기 위함이다.
- 각 계층에서 생성하는 데이터 유닛(data unit)을 프로토콜 데이터 유닛(Protocol Data Unit)이라고 한다.
  (message - segment - datagram - frame - bit)

**이렇게 통신하는 과정에는 보안의 기능이 필요하다. 보안에 대해 살펴보자.**

## 보안(Security)

처음에는 약속된 소수의 사람이 이용했기 때문에 보안의 문제가 없었다. 하지만 네트워크가 커지면서
보안은 중요한 영역이 되었다.

간단하게 용어와 개념만 살펴보도록 하자.

malware : 컴퓨터 내부에 존재하는 악성 코드를 말한다.

- virus : 다운로드해서 실행해야만 구동이 된다.
- worm : 다운로드받기만 하면 저절로 구동이 된다.

ex) spyware malware can record keystrokes, web sites visited, upload info to collection site

Botnet : 스팸과 DDoS 공격에 사용되는 위해를 입은 컴퓨터의 집합을 말한다.

Denial of Services(DoS) : 서버에 garbage traffic을 계속 발생시켜서 서버가 실질적인 사용자에게 Service를
하지 못하도록 만드는 공격
(타겟을 선정한 다음 그 주변 컴퓨터들을 봇넷으로 만든 다음 봇넷으로부터 타겟에 계속해서 패킷을 전송하도록 하는
공격이다.)

Packet Sniffing : 다른 호스트들의 패킷 교환을 엿듯는 것을 의미한다.

- Wi-fi, Shared ethernet 등의 broadcast media 환경에서 발생한다.
- end system에는 network에 접속하기 위해 NIC(network interface card)가 전부 장착되는데 이 NIC에는 물리적으로
  고유한 번호가 있다. frame에 적힌 주소와 자신의 NIC 번호가 일치하는지 확인하여 위 계층으로 전달하거나 frame을
  drop한다. broadcast의 경우 그 network에 연결된 모든 host에 전달되기 때문에 전달받아서 위 계층으로 올린다.
- 간혹 NIC의 모드를 관리 목적을 위해 promiscuous 모드로 설정할 수 있는데 이를 이용해서 나에게
  오는 frame이 아니더라도 모두 위 계층으로 전달할 수 있다. 이러한 방법을 이용해 malware가 다른 host들의 패킷을
  모두 확인하는 수법을 Packet Sniffing이라고 한다.

IP spoofing : 하나의 호스트가 Source address를 변조해 합법적인 호스트로 가장하여 시스템에 접근하는 수법
