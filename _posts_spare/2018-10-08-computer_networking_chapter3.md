---
layout: post
subclass: post
title: "컴퓨터 네트워크 Chapter3. 트랜스포트 계층"
date: 2018-10-08 00:00:03
tags: [dudaji, computer-science, computer_networking]
excerpt: "컴퓨터 네트워크 Chapter3. 트랜스포트 계층에 대한 정리입니다."
disqus: True
---

# 3장 트랜스포트 계층 단원의 개념 구조도

![network_ch3_recap1](/images/computer_networking/network_ch3_recap1.jpg)

![network_ch3_recap2](/images/computer_networking/network_ch3_recap2.jpg)

![network_ch3_recap3](/images/computer_networking/network_ch3_recap3.jpg)

# 트랜스포트 계층의 서비스

## 트랜스포트 계층 서비스의 역할

트랜스포트 계층의 서비스는 한마디로 '어플리케이션 프로세스간의 논리적인
통신을 제공하는 것'이다.
(여기서 논리적이라는 말은 어플리케이션 관점에서는 직접 연결된 것처럼
보인다는 의미다.)

## 네트워크 계층과의 관계로 본 트랜스포트 계층의 서비스

네트워크 계층은 '호스트들간의 논리적인 통신'을 제공한다.
반면, 트랜스포트 계층은 '프로세스들간의 논리적인 통신'을 제공한다.
이것이 트랜스포트 계층과 네트워크 계층의 차이점이다.

## 트랜스포트 계층의 서비스 모델

트랜스포트 계층의 서비스는 프로세스들간의 논리적인 통신을 제공하는 것이
가장 기본이다. 여기에 네트워크 계층을 보완하고 개선하기 위한 추가적인 서비스를
제공한다.

IP 서비스 모델은 호스트들간의 논리적 통신을 제공하는 best effort delivery service다.
세그먼트들 전달하기 위해 최선을 다하지만, 어떠한 보장도 하지 않는다는 것이다. 특히,
IP는 세그먼트가 순서대로 전달되는 것을 보장하지 않는다. 내부 데이터의 무결성도
보장하지 않는다.

그래서 각 프로토콜에서는 다음과 같은 추가적인 서비스를 제공한다.

- UDP : 추가적인 서비스가 딱히 없다.
- TCP : 흐름제어, 순서번호, 확인응답, 타이머, 혼잡제어

참고로 TCP, UDP 모두 delay gurantees, bandwidth gurantees는 보장하지 않는다.

**이러한 추가적인 서비스를 제공하지만 가장 기본적인 서비스는 process to process delivery다.
이 process to process delivery를 위해 필요한 것이 'multiplexing과 demultiplexing이다.**

# 다중화와 역다중화(multiplexing and demultiplexing)

![multiplexing_demultiplexing](/images/computer_networking/multiplexing_demultiplexing.png)

3개의 host 중 가운데 호스트를 서버 호스트라고 가정하고 나머지 두 개의 호스트를
클라이언트 호스트라고 가정하자. 이 때 여러 클라이언트 호스트가 서버 프로세스에
contact할 수 있다.

이 때 receiver 이 상황에서는 서버가 클라이언트의 세그먼트 데이터를 받아서 올바른 소켓에
전달하는 작업을 '역다중화(demultiplexing)'이라고 한다.

반대로 이 상황에서 클라이언트는 서버에게 세그먼트 데이터를 전송하기 위해 소켓으로부터 데이터를
모아서 헤더 정보를 붙이고 세그먼트를 생성한 다음 네트워크 계층으로 전달하는데 이 작업을
'다중화(multiplexing)'이라고 한다.

## 역다중화의 동작방식(How demultiplexing works)

![segment](/images/computer_networking/segment.png)

위 그림은 트랜스포트 계층의 세그먼트 형식이다. 헤더에는 출발지의 포트 번호와
목적지의 포트 번호가 기록된다. 이는 역다중화에 사용된다. 원래 최종 목적지 주소는
2가지로 구성되는데 host address는 network 계층에서 붙여주는 것이다.

### Connectionless demultiplexing (UDP)

먼저, 기억해야 할 것은 demultiplexing은 receiver에서 일어난다. 여기서의 receiver는
Client와 Server 모두가 될 수 있다.

- 소켓 생성 시 host local port number가 할당된다.
  (Client port number는 없는 것이 아니라 implicit하게 지정되는 것이다. 프로그래머가
  모를 뿐이지 OS가 host 내에서 unique한 port 번호로 할당한다.)
- UDP socket으로 보낼 때는 반드시 목적지 IP주소와 port number를 명시해주어야 한다.

이렇게 작업한 다음 receiver 호스트가 수신하게 된다.

- UDP segment를 수신하고 segment에서 목적지 port number를 확인한 다음 해당 소켓으로
  전달한다.

그림으로 살펴보면 다음과 같다.

![connectionless_demultiplexing](/images/computer_networking/connectionless_demultiplexing.png)

정리하면 Connectionless demultiplexing에서는 하나의 application이 하나의 Socket을 열고 있고
그 Socket을 식별하기 위해 목적지 IP주소와 포트 번호를 사용한다.

### Connection-oriented demultiplexing (TCP)

Connection-oriented demultiplexing을 위해서는 4가지 정보를 필요로 한다.

- Source IP address
- Source port number
- Destination IP address
- Destination port number

왜 각각의 정보가 필요한지 살펴보도록 하자.

예를 들어 receiver인 Server host가 먼저 socket을 생성해서 열고 있는 상황을
생각해도록 하자. 이 때 Client가 contact하면 Connection-oriented 방식에서는
그 client만을 위한 socket을 열어준다. 이 때 demultiplexing을 하려면 위의 4가지
정보가 필요하다.

- Destination IP address : 목적지 host를 제대로 찾았는지 파악하는데 사용된다.
- Destination port number : app을 제대로 찾았는지 파악하는데 사용된다.

그리고 Connection-oriented 방식은 2가지 정보가 더 필요하다. Client만을 위한 Socket을
별도로 생성하기 때문에 그 Client만을 위한 Socket을 찾으려면 아래의 두 가지 정보가 더
필요한 것이다.

- Source IP address
- Source port number

Connectionless 방식은 app마다 하나의 port만을 열기 때문에 목적지 IP주소와 목적지 port번호만
있으면 식별 가능하지만 Connection-oriented 방식은 어느 소켓인지 파악하려면 Source를 알아야
하는 것이다.

![connection_oriented_demultiplexing](/images/computer_networking/connection_oriented_demultiplexing.png)

위 그림을 보면 application layer에 프로세스도 여러 개다. 이는 C로 Socket programming을
해보면 Socket을 생성할 때 fork해서 자식 프로세스를 생성한다. 자식 프로세스는 Socket으로
데이터를 주고 받는 일을 담당하게 된다. 그런데 프로세스가 여러 개면 관리하는 오버헤드가 커지므로
threading 기법으로 구현하게 된다.

**이제 transport 계층의 두 가지 프로토콜을 각각 살펴보도록 하자.**

# Connectionless transport : UDP

## UDP의 특징

### connectionless

**UDP의 가장 큰 특징은 Connectionless하다는 것이다.**

이 Connectionless하다는 말의 의미는 no handshaking 즉 connection을
setup하는 과정이 없다는 말이다.

또 연결이 없기 때문에 각각의 UDP segment는 독립적으로 전달된다는 것이다.
("each UDP segment handled **independently** of others")
즉, 동일한 클라이언트에서 UDP로 보낸 두 패킷이 있다면, 받는 쪽은
동일한 클라이언트로부터 온 것이라는 것을 인식하지 못한다는 말이다.

### no-frills

UDP에서는 네트워크 계층을 보완하기 위한 추가적인 작업을 하지 않는다.

### "best effort" service

최선을 다할 뿐이다. 전달과정에서 패킷이 손실되거나 패킷의 순서가 뒤바뀐 채로
전달되더라도 알 방법이 없고 송신한 host에 알려주지도 않는다.

- lost
- delivered out of order to app

만약 각 전달이 독립적이기 때문에 패킷 순서가 뒤바뀐 채로 전달된다면 그대로
전달되는 것이다.

**이렇게 best effort service지만 패킷 손실과 패킷 순서의 역전되는 상황에
대한 대책이 없다는 것은 큰 문제다. 하지만 UDP의 활용사례는 많다.
하나씩 살펴보도록 하자.**

## UDP의 활용사례

### Streaming Multimedia

왜일까? 크게 2가지 이유다.

- loss tolerant

영상의 미묘한 loss는 사람의 눈으로 인지하는데 큰 지장이 없다.

- rate sensitive

연속적으로 비디오나 오디오가 재생되려면 minimum throughput이 보장되어야
하는데 TCP의 경우 congestion control로 이를 막을 수 있다. 그래서 rate sensitive한
매체들은 대개 UDP를 사용한다. 재생되는 속도가 맞게 데이터가 밀어넣어져야 하기 때문이다.

### DNS

왜일까? 'DNS query response'를 잘 생각해보면 된다. DNS에서 query를 보내고 response를
받는 과정을 잘 살펴보면 연속적인 데이터 교환이 아니라 한 번의 transaction으로 이루어지는
것을 알 수 있다. 그래서 이러한 one time transaction의 경우 TCP와 같은 프로토콜은 오버헤드가
크므로 UDP를 사용한다.

### SNMP - 네트워크 관리 프로토콜의 일종

네트워크 관리를 위해 각 노드에서 status를 보고한다. 그런데 이 경우 한 번 전달이 되지 않더라도
큰 문제는 없다. 왜냐하면 정기적으로 status를 보고하기 때문에 더욱 최신의 정보가 나중에 송신되기
때문이다.

## reliable transfer over UDP

UDP를 사용하더라도 reliablity를 확보할 수 있는 방법은 있다. reliablity를 확보하기 위해
data integrity를 확인하는 작업을 application layer에서 해주면 되는 것이다. 그리고
전달에서의 오류가 생겼을 때 application-specific error recovery를 application layer에서
해주면 된다.

**앞서 UDP의 활용사례를 살펴봤다. 또 data integrity를 확보해야 하는 상황에서도 대안이 있다는
것을 확인했다. 그러면 앞서 살펴본 UDP 활용사례에서의 활용 이유를 조금 더 자세히 정리해보도록
하자.**

## Why is there a UDP

### no connection establishment

connection을 set up하는 과정이 별도로 없기 때문에 별도의 지연이 없다.

### simple : no connection state at sender, receiver

연결을 하지 않기 때문에 연결 상태를 별도로 유지할 필요가 없다.

### no congestion control

비디오나 오디오의 경우 congestion control이 없기 때문에 재생속도만큼
그에 따라 필요한 데이터를 밀어낼 수 있다.

### small header size -> small traffic overhead

![UDP_segment](/images/computer_networking/UDP_segment.png)

header를 살펴보면 위의 4개 필드 정도다. 이 말 자체가 traffic overhead가
작다는 말이고 실어보내야 하는 데이터의 양이 작다고 말할 수 있다.

**UDP를 사용하는 이유를 살펴봤다. 마지막으로 header의 size가 작다는 것을
살펴봤는데 UDP segment header의 필드 중 하나인 UDP checksum을 살펴보고
넘어가자.**

## UDP checksum

UDP checksum : UDP segment header에 있는 checksum 필드

목적은 세그먼트를 전달하는 과정에서 오류를 감지하기 위해서다.

그렇다면 어떻게 오류를 감지할 수 있을까?

![UDP_checksum](/images/computer_networking/UDP_checksum.png)

먼저 송신하는 호스트는 UDP segment를 16 bit integer의 연속으로 본다.
(as sequence of 16-bit integers)

그래서 각각의 16비트를 다 더한다. 위의 그림처럼 두 개의 16bit integer를
더해준다. 그리고 캐리가 발생할 경우 그 캐리 비트를 최하위 비트에 더해준다.

그렇게 전부 다 더해서 얻은 결과값에 대해 1의 보수값을 구한다. 이 1의 보수값을
checksum 필드에 넣는다.

그렇게 데이터를 송신한다.

수신하는 호스트도 똑같은 과정을 거친다. 그리고 이를 통해 checksum을 구한 후
checksum 필드와 일치여부를 비교한다. 일치하지 않는다면 송신되는 과정에서
transmission bit error가 발생한 것이다.

그런데 이 checksum이 오류를 다 잡을 수는 없다. 예를 들어 두 16비트의 integer의
같은 자리 bit가 서로 뒤집히더라도 checksum은 같기 때문에 이를 발견하지 못한다.

그렇다면 UDP는 reliable data transfer을 하지도 않는데 checksum을 어디에 사용할까?
오류 여부를 체크한 다음은 protocol에서 정의하고 있지 않다. 그러므로 application에
오류를 알리는 방식으로 처리할 수도 있고 drop할 수도 있다. 구현의 차이라고 할 수 있다.

**UDP에 대해 살펴봤다. 신뢰성 있는 전송을 하는 TCP를 살펴보기 전에 신뢰성 있는 데이터
전송의 원리를 먼저 살펴보자. TCP를 이해하는데 더욱 도움이 될 것이다.**

# 신뢰성 있는 데이터 전송의 원리

가장 먼저 신뢰성 있는 통신을 하기 위해서는 메세지가 정확하게 수신되었는지 또는
잘못 수신되어 반복이 필요한지 송신자에게 알려줄 수 있어야 한다. 이를 위해 필요한 것이
ACK과 NACK이다.

## 긍정 확인응답 : ACK(Acknowledgement)

패킷 또는 집합이 정확하게 수신되었다는 응답을 송신자에게 하기 위해 수신자에 의해 사용된다.
확인응답을 프로토콜에 의존하여, 개별적이거나 누적될 수 있다. (하나의 메시지에 대해 확인응답을
주거나 특정 패킷까지 정확하게 수신되었다는 것을 전달할 수 있다.)

## NACK(Negative acknowledgement)

패킷이 정확히 수신되지 않았다는 응답을 송신자에게 하기 위해 수신자에 의해서 사용된다. 일반적으로
정확히 수신되지 않은 패킷의 순서번호를 전달한다.

ACK 또는 NACK을 송신하기 위해서는 메세지에 오류가 있는지 감지하는 메커니즘이 필요한데, 그것이
체크섬(checksum)이다.

## 체크섬(checksum)

전송된 패킷 안의 비트 오류를 발견하는 데 사용된다.

그런데 메세지가 네트워크를 거쳐 전송되는 과정에서 loss가 발생할 수 있다. 그러므로 메세지가 전달되지
않았으니 ACK, NACK 모두 보내지 못하는 상황이 생긴다. 그래서 그렇게 가만히 기다리게 된다. 이런 상황을
방지하기 위해 일정시간이 지나도 전송이 되지 않았을 때 재전송 등을 결정하기 위해 사용하는 것이 Timer다.

## 타이머(Timer)

채널 안에서 패킷이 손실되었을 때 패킷의 타임아웃/재전송에 사용된다. 타임아웃은 패킷이 지연되었지만 손실되지는
않았을 때, 또는 패킷이 수신자에 의해 수신되었지만 수신자가 보내는 ACK이 손실되었을 때 발생한다.

그러므로 제대로 전송된 패킷이지만 불필요하게 재전송되는 상황이 발생할 수 있다. 이러한 상황을 방지하기 위해
순서 번호(sequence number)가 필요하다.

## 순서 번호(sequence number)

송신자에서 수신자로 가는 데이터의 패킷의 순서번호를 붙이기 위해서 사용된다. (패킷의 ID를 부여하는 셈이다.)
수신자 패킷의 순서번호의 격차가 생기면 수신자는 손실된 패킷을 검사하게 된다. 또한, 중복된 순서번호를 갖는
패킷은 수신자가 패킷의 중복 수신을 검사할 수 있게 한다.

이렇게 제대로 패킷들이 제대로 전송되더라도 여전히 문제는 있다. 지금까지의 방식을 생각해보면 송신자는 수신자가
현재의 패킷을 정확하게 수신했다는 것을 확인하기 전까지느 새로운 데이터를 전송하지 않은
'전송 후 대기(stop-and-wait)'방식이다. 이렇게 되면 송신자와 수신자 사이를 파이프라고 가정한다면 파이프 안에는
단 하나의 패킷만 존재할 수 있다. 그런데 효율적으로 파이프를 사용하려면 응답이 오지 않더라도 패킷을 파이프에
넣어서 보내야 한다. 즉 파이프라이닝을 해야 한다.

## 파이프라이닝(pipelining)

송신자는 주어진 범위에 있는 순서번호를 가진 패킷만 전송하도록 제한될 수 있다. 확인 응답은 아직 없지만,
허가된 다중 패킷이 전송될 수 있기 때문에, 송신자의 이용률은 전송 후 대기 모드보다 증가될 수 있다.
throughput이 높아지는 것이다.

## 윈도우

파이프라이닝을 할 때 윈도우 크기를 설정해 수신자의 메세지를 수신하고 버퍼링하는 능력, 네트워크의 혼잡 정도 등에
따라 ACK을 받지 않고도 보낼 수 있는 최대 데이터의 크기를 제어할 수 있다.

**이러한 원리들을 바탕으로 TCP를 살펴보도록 하자.**

# Connection-oriented Transport : TCP

가장 먼저 TCP 연결에 대해 살펴보자.

## TCP 연결

### handshaking

TCP 연결을위해서는 "핸드쉐이킹(handshaking)"이 선행되어야 한다. 즉, 데이터 전송을
보장하는 파라미터들을 각자 설정하기 위한 사전 세그먼트들을 보내야 한다.

### 전이중(full-duplex)

그리고 연결이 되면 이 연결은 full-duplex 방식이다. 동일한 connection 내에서 한쪽이 아니라
서로 데이터를 주고 받을 수 있다.

### point-to-point

TCP 연결은 또한 항상 단일 송신자와 단일 수신자 사이의 점대점 통신이다. 단일 송신 동작으로
한 송신자가 여러 수신자에게 데이터를 전송하는 "멀티캐스팅"은 TCP에서는 불가능하다.

### pipelined

TCP 연결에서의 데이터 송수신은 pipelined 되어 있다. 이유는 throughput을 향상시키기 위해서다.
pipelining을 하지 않으면 reliable transport를 위해 segment에 대한 ACK을 계속 기다려야 한다.
그렇게 되면 하나의 message만 pipe를 통해 송수신될 수 있기 때문에 효율이 향상시키기 위해
pipelining을 한다. 이를 통해 flow control과 congestion control에 의해 결정되는 window만큼
ACK을 받지 않은 상태로 pipe에 밀어넣을 수 있다.

### delivering bytestream

한 번 TCP 연결이 되면 TCP 연결을 통해 byte stream을 주고 받는다. 무슨 의미인가?
**"no message boundaries"**라는 뜻이다. 메세지간 경계가 없다.

UDP의 경우 여러 Client로부터 수신한 자료의 발신자를 구분하기 위해 메세지의 경계를
유지해야 한다. 반면 TCP는 user가 message를 내려보내면 일단 자신의 buffer에 받는다.
그래서 message boundaries에 대해 의식하지 않고 buffer에 있는 데이터를 byte stream으로
본다. 즉, byte의 연속으로 보는 것이다. 이 byte의 연속을 maximum segment size에 따라
byte를 잘라서 보내는 것이다.

참고) MSS(Maximum Segment Size)

네트워크마다 MSS가 있다. 네트워크 링크에서 물리적으로 실어나를 수 있는 최대 frame size가
있다. 이 frame에 집어넣을 수 있을 정도만 user message를 잘라 보내면 된다.

**다음은 이 TCP 연결을 통해 송수신되는 TCP Segment 구조에 대해 살펴보자.**

## TCP Segment 구조

![TCP_segment](/images/computer_networking/TCP_segment.png)

- Header + Payload의 구조다.
- UDP 헤더보다 TCP 헤더가 훨씬 복잡하다.
  - 왜? 헤더는 자신의 파트너 프로토콜에게 협력을 위해 보내는 편지다. TCP가
    하는 일이 훨씬 더 많으니 당연히 훨씬 복잡하다.
- Header는 Header의 길이가 고정적인가 가변적인가에 따라 다시 두 부분으로
  나누어진다.
  - 위 5줄이 고정 길이의 header다.(4byte \* 5 = 20byte) 즉, TCP 세그먼트가
    생성되면 default로 20byte는 붙는다는 말이다.
  - 위 그림에서 options 부분이 가변 길이의 헤더다. 따라서 총 헤더의 길이도
    가변적이다. 그래서 Header Length 부분이 필요하다.

각 필드를 하나씩 살펴보도록 하자.

- Source port# | Dest port#
  - TCP도 UDP 처럼 가장 핵심적인 기능은 Process to Process logical
    communication이다. 그래서 이 필드는 multiplexing과 demultiplexing을 위해
    필요하다.
- Sequence number
  - Sequence number는 reliable data transfer 원리에서 언급되었듯이
    중복 데이터 감지와 in-order delivery를 위해 필요하다.
- Receive window
  - Receive window는 flow control을 위해 필요한 필드다. 상대방이 나에게
    보내는 데이터에 대해 n 다음으로 m 만큼 ack 없이 보내라는 뜻이다.
    그러므로 이는 receiver의 buffer size에 의해 결정된다. (buffer가 full이면
    window size를 0으로 세팅한다.)
- Acknowledgement number
  - 상대방이 나에게 보낸 데이터에 대해 n-1까지 잘 받았다는 의미고, n부터 받기를
    기대한다는 의미다.
- PSH
  - push라는 뜻이다. TCP는 buffer에 저장하다가 push가 set된 패킷이 오면
    buffer data + push on segmemt를 즉시 내보낸다. 받는 쪽도 PSH가 Set되어 있으면
    수신 버퍼의 내용을 application 계층으로 올리고 buffer를 flush한다.
- URG
  - urgent라는 뜻이다. 이 경우 수신하는 쪽에서 수신 버퍼의 내용을 application 계층으로
    바로 올린다. 대신 PSH와의 차이점은 URG DATA POINTER로 URG DATA를 가리킨다는 것이다.
- ACK
  - Acknowledgement number와 관련된 필드다. ack 정보의 유무를 표시한다.

**이 세그먼트 구조에서 신뢰성 있는 데이터 전송의 핵심인 Sequence number와 ACK
정보를 조금 더 자세히 살펴보도록 하자.**

## Sequence number & ACK in detail

### Sequence number

sequence numbers : byte stream "number" of first byte in segment's data

주의할 점은 여기서 순서 번호는 세그먼트에 대한 순서번호가 아니라는 것이다. 전송된
바이트 스트림에 대한 순서 번호다. 그래서 sequence numbers는 byte stream 의 첫
번째 수와 같은 방식으로 정의되는 것이다.

### Acknowledgement number

acknowledgement number : seq# of next byte expeected from other side

상대방에게 다음에 몇 번 sequence number의 데이터를 받을 것을 기대하는지를
의미하는 번호다. 그 말은 acknowledgement number 전까지는 잘 받았다는 말이다.

TCP에서는 cumulative ACK을 사용하고 있다. 예를 들어 생각해보면 0 ~ 535 까지의
세그먼트와 900 ~ 1000의 바이트를 포함하는 세그먼트를 수신했다면 536과 899 의
바이트를 아직 수신하지 못했고 기다린다. 그래서 Acknowledgement number는
여전히 536이 되고 TCP는 cummulative acknowledgement 즉 누적 확인응답을 한다고
말한다.

여기서 처리하는 방법은 구현에 달렸다. 536 ~ 899 이 아니므로 900 ~ 1000의 바이트를
즉시 버리는 방법이 있고, 아니면 보유하고 있다가 빈 공간에 수신하지 못한 데이터를
채우는 방법도 있다. 후자가 네트워크의 대역폭 관점에서는 더 효율적이고 실제에서도
취하는 방법이다.

### Sequence number & acknowledgement number Scenario

그렇다면 TCP Sender 측의 sequence number space를 살펴보면서 sequence number와
acknowledgement number의 변화를 파악해보자.

![sequence_number_space](/images/computer_networking/sequence_number_space.png)

먼저 window size는 ACK과 상관없이 연속적으로 내보낼 수 있는 부분이다.

그리고 위 그림에서 A 부분은 송신하고 ACK까지 받은 부분이다.

B부분은 보냈지만 아직은 ACK 응답을 받지 못한 부분인다. 즉, 파이프 안에서 움직이고
있는 데이터를 의미한다.

C부분은 내보낼 수 있지만 아직 application 계층에서 내려받은 것이 없거나 segment를
꽉 채울만큼 데이터가 모이지 않았거나 하는 등의 이유로 사용할 수 없는 부분이다.

마지막으로 D는 Window가 허용하지 않는 범위여서 사용이 불가한 부분이다.

여기서 A 바로 다음에 오는 점이 바로 acknowledgement number다. 보냈고 ACK을 받은 부분이기
때문에 이 점 전까지는 잘 받았다는 의미고 이 점부터 ACK을 받기를 기대한다는 의미이기 때문이다.

그리고 B 바로 다음에 오는 점이 바로 sequence number다. 다음으로 송신할 byte stream의
처음 바이트이기 때문이다.

그러면 시나리오를 통해 sequence number와 acknowledgement number의 변화를 살펴보자.

![sequence_scenario](/images/computer_networking/sequence_scenario.png)

클라이언트의 초기 순서번호는 42이고, 서버는 79이다. 이 때 Client는 먼저 42번부터
송신하면서 79번부터의 데이터를 원한다고 서버에 알려준다.

서버는 초기 순서번호이며 클라이언트가 수신하기를 기대하는 번호인 79번부터 데이터를
송신해준다.

클라이언트가 마지막으로 송신할 때 보면 Seq#은 1이 늘었고, ACK#도 1이 증가한 것을 알
수 있다. 1byte의 char형 데이터를 하나 송신해서 순서 번호가 1 증가하고, ACK도 1byte
데이터를 받았기 때문에 1 증가한 것을 알 수 있다.

이 때 특징적인 것은 클아이언트와 서버는 데이터를 운반하는 세그먼트에 확인 응답이 같이
전달된다는 것이다.

**이 때까지 TCP의 세그먼트 구조에 대해 살펴봤다. 이 TCP의 세그먼트는 loss가
발생할 수 있었다. 그리고 이를 해결하기 위해 타임아웃/재전송 메커니즘을 사용했다.
타임아웃/재전송 메커니즘에 대해 살펴보도록 하자.**

## 타임아웃/재전송 메커니즘

타임아웃/재전송 메커니즘의 핵심은 결국 타임아웃 주기다. 그런데 타임아웃 주기는
어떻게 결정할 수 있을까? 분명한 건 타임아웃 주기는 왕복시간인 RTT(Roundtrip time)보다는
커야 한다는 것이다. (왕복시간 내에 오지 않으면 segement가 loss되었거나 ACK이 loss되었거나
문제가 발생한 것이다.)

그런데 문제가 있다. 왕복시간인 RTT를 정확히 알 수 없다는 것이다.

### RTT 예측

그래서 RTT를 예측하는 수 밖에 없다. 그런데 RTT를 너무 짧게 예측한다면 어떻게 될까?
제대로 전송이 된 뒤 ACK이 오고 있는데 재전송하게 된다.

반대로 RTT를 너무 길게 예측한다면? 실제로 loss가 발생했지만 너무 늦게 상황을 파악하게 된다.

그러면 어떻게 예측하는 것이 좋을까?

예측을 하려면 미래를 모르니 과거를 통해 예측하는 수 밖에 없다.

Sample RTT : measured time from segment transmission until ACK receipt

Sample RTT는 하나의 세그먼트가 전송되고 ACK 응답이 올 때까지의 시간이다. 그런데
이 Sample RTT는 라우터에서의 혼잡과 종단 시스템에서의 부하 변화 때문에 굉장히
불규칙적이다. 그래서 Sample RTT 하나로 예측하면 제대로 된 예측이 되지 않는다.
그렇기 때문에 평균을 구한다.

그런데 참고로 어떤 세그먼트가 재전송되었다면 이 땐 무시한다. 이유는 ACK이 어떤 세그먼트에 대한
ACK인지 알 수 없다. 처음 전송된 세그먼트에 대한 ACK인지 재전송된 ACK에 대해 오는 ACK인지
알 길이 없다. 그래서 둘 중 하나를 선택해서 통일해야 한다. 그래서 재전송된 세그먼트는
무시하는 것이다.
(결국 판단이다. 원래 세그먼트로 잡으면 RTT가 길어질 것이고 재전송되는 세그먼트에 대해서만
계산한다면 Sample RTT가 짧게 잡힐 것이다.)

그래서 RTT를 예측하는 공식은 다음과 같다.

```
Estimated RTT = (1-a) * Estimated RTT + a * SampleRTT
```

새로운 Estimated RTT는 이전까지의 Estimated RTT값과 새로운 SampleRTT의 조합으로 결정된다.
이 때 어디에 더 무게를 둘 것인지를 결정하는 a는 대개 0.125로 권장된다.(RFC 6298)

지수적 가중 이동 평균이기 때문에 최신의 SampleRTT를 더욱 반영하게 되는 방식이다. 그 이유는
최신의 SampleRTT가 최신의 네트워크 혼잡 상태를 더욱 잘 반영하기 때문이다.

![estimatedRTT](/images/computer_networking/estimatedRTT.png)

또 하나 의미가 있는 것은 RTT의 변화율이다. 그런데 이렇게 예측을 하더라도 SampleRTT 값과
Estimated RTT 값 사이에는 상당한 차이가 있을 수 있다. 위 그림처럼 말이다.
그래서 Estimated RTT에 safety margin을 더해준다.

결국 timeout interval은 EstimatedRTT + safety margin으로 결정된다.

safety margin을 구하기 위해 DeviationRTT를 구한다.

```
DevRTT = (1-b) * DevRTT + b * |SampleRTT - EstimatedRTT|
```

DevRTT가 크다는 말은 EstimatedRTT와 SampleRTT의 차이가 크다는 말이다. 그래서
변동이 클수록 더 큰 safety margin을 더해준다. 대개 b는 0.25 값으로 권장된다.
그리고 timeoutInterval은 다음과 같은 공식으로 구할 수 있다.

```
TimeoutInterval = EstimatedRTT + 4 * DevRTT
```

**앞서 타임아웃/재전송 메커니즘을 살펴본 것처럼 TCP 세그먼트에는 loss가
발생할 수 있다. 이런 문제를 TCP가 어떻게 해결하는지 신뢰성 있는 데이터 전송에
대해 알아보도록 하자.**

## TCP - 신뢰성 있는 데이터 전송(Reliable Data Transfer)

transport 계층의 아래 계층인 network 계층의 IP 서비스는 비신뢰적인 best effort
서비스를 제공한다. 이러한 환경은 트랜스포트 계층에 상당한 영향을 미치는데
TCP는 이러한 환경에서도 신뢰성 있는 데이터 전송을 제공한다.

어떻게 가능한지 살펴보도록 하자.

### 송신자 측의 이벤트

송신자측의 이벤트를 살펴보도록 하자. 크게 상황은 3가지가 있을 수 있다.

#### 세 가지 주요 이벤트

- Application layer에서 데이터를 수신하는 경우
- 타이머 타임아웃
- 수신자로부터 ACK을 수신하는 경우

#### Application layer에서 데이터를 수신하는 경우

- 1. message에 헤더를 붙여서 segment로 포장한다.
     (이 때 세그먼트에 Seq# 실고 checksum도 처리한다.)
- 2. Segment로 캡슐화한 다음 현재 timer가 실행 중이 아니면 timer를
     실행시킨다.

TCP는 하나의 연결마다 하나의 단일 타이머를 사용한다. timer는
아직 ACK을 받지 못한 Segment 중에 가장 오래된 세그먼트에 대해
세팅한다.

타이머가 running 상태라는 말은 해당 Segment보다 이미 먼저 나갔는데
아직 ACK을 받지 못한 세그먼트가 있다는 것이다. 그러므로 타이머를
세팅하지 않아도 된다.

"think of timer as for oldest unacked segment"

그리고 만료 주기는 TimeOutInterval로 세팅한다.

#### Timeout

타임아웃이 발생하면

- 1. 타임아웃을 유발한 세그먼트를 재전송한다.
- 2. 새로 세그먼트를 내보냈으니 타이머를 재시작한다.

#### 수신자로부터 ACK을 수신하는 경우

- 1. 수신 확인응답이 확인되지 않은 가장 오래된 바이트의 순서번호인 SendBase와
     수신한 ACK값인 y를 비교한다.

SendBase-1까지 정확하게 차례대로 수신이 되었다는 말이다.

- 2. TCP는 누적 확인 응답을 사용하므로 y는 y 바이트 이전의 모든 바이트들의 수신을
     확인한다.

따라서 SendBase 변수를 갱신한다. 또한 아직 확인 응답 안된 세그먼트들이 존재한다면
타이머를 다시 시작한다. (타이머는 아직 ACK을 받지 못한 가장 오래된 세그먼트에 대해
세팅된다.)

코드로 살펴보면 다음과 같다.

```c
NextSeqNum = InitialSeqNumber
SendBase = InitialSeqNumber

loop(forever){
  switch(event){
    event :data received from applicationn above
      create TCP segement with sequence number NextSeqNum
      if (timer currently not running){
        start timer
      }
      pass segment to IP
      NextSeqNum = NextSeqNum + length(data)
      break;

    event: timer timeout
      retransmit not-yet-acknowledged segment with smallest sequence number
      start timer
      break;

    event: ACK received, with ACK field value of y
      if (y > SendBase) {
        SendBase = y
        if (there are currently any not-yet-acknowledged segments) {
          start timer
        }
      }
      break;
  }
}
```

**위 상황을 바탕으로 송신자 측의 시나리오를 한 번 살펴보도록 하자.**

#### TCP Retransmission Scenario

- 1. 제대로 수신했지만 ACK 응답이 loss되는 경우

![retransmission_scenario1](/images/computer_networking/retransmission_scenario1.png)

위 그림을 살펴보면 99번까지 잘 받았기 때문에 ACK=100을 송신하지만 그 ACK이 loss되는 경우다.
결국 timeout이 발생하고 데이터를 재전송하게 된다.

- 2. 두 개의 연속된 세그먼트 중 첫 번째 세그먼트에 대한 ACK을 수신하지 못한 경우

![retransmission_scenario2](/images/computer_networking/retransmission_scenario2.png)

두 개의 연속된 세그먼트 중 첫 번째 세그먼트에 대해 ACK을 수신하지 못해 ACK이 발생한다.
이 경우 첫 번째 세그먼트를 재전송한다. (확인 응답이 되지 않은 가장 작은 순서번호를 가진
세그먼트를 재전송한다.) 이에 대한 응답으로는 120이 도착한다. TCP는 cummulative ACK이기
때문이다.

- 3. 첫 번째 세그먼트에 대한 ACK을 수신하지 못했지만 Timeout 전에 두 번째
     ACK을 수신하는 경우

![retransmission_scenario3](/images/computer_networking/retransmission_scenario3.png)

두 번째 시나리오와 같은 상황이지만 첫 번째 세그먼트에 대한 타임아웃이 발생하기 전에
두 번째 세그먼트에 대한 ACK이 도착하면 호스트 A는 두 세그먼트 모두 전송하지 않는다.

#### TCP 구현의 수정사항

- 1. 타임아웃 주기를 두 배로 설정

TCP는 재전송할 때마다 타임아웃의 주기를 이전 값의 두 배로 설정한다. 하지만 애플리케이션에서
데이터를 수신하거나 ACK을 수신하는 경우 TimeoutInterval은 EstimatedRTT와 DevRTT의 가장
최근 값에서 가져온다.

- 2. 빠른 재전송

재전송에서의 한 가지 문제점은 때때로 타임아웃의 길이가 비교적 길다는 것이다. 그래서
loss가 발생했을 때 time-out이 발생해 재전송되기까지 시간이 제법 걸린다.

이를 보완하기 위해 duplicative ACK이 계속 들어오면 timeout이 발생하기 전에
retransmit를 한다. 이를 "빠른 재전송(fast retransmit)"이라고 한다.

![fast_retransmit](/images/computer_networking/fast_retransmit.png)

왜 이렇게 할까? TCP는 파이프라이닝을 한다. 그러므로 윈도우가 허용하는 범위 내에서
세그먼트를 연속해서 내보낸다. 그러다가 loss가 발생해서 세그먼트간의 gap이 발생할 수
있다. 이 때 TCP는 NACK이 없고 cummulative ACK을 사용하기 때문에 duplicate ACK을
보낸다. 그렇게 세 번 duplicate ACK을 3번 보내면("triple duplicate ACKs") 거의 확실히
이전에 loss가 생겨 out-of-order이 된 것으로 판단해 time-out이 발생하지 않았음에도 재전송을
해준다. 이를 통해 비교적 긴 타임아웃 주기로 생기는 문제점을 해결할 수 있다.

참고) 왜 3번이나 기다릴까?
TCP는 불필요한 재전송을 최대한 피하려는 경향이 있기 때문이다.

코드로 살펴보면 다음과 같다.

```c
event : ACK received, with ACK field value of y
          if (y > SendBase) {
            SendBase = y
            if (there are currently any not yet acknowledged segments) {
              start timer
            }
            else { /* a duplicate ACK for already ACKed
              increasement number of duplicate ACKs received for y
              if (number of duplicate ACKs received for y == 3) {
                /* TCP fast retransmit */
                resend segment with sequence number y
              }
            }
          }
```

**이 때까지 IP 서비스의 비신뢰적인 데이터 전송을 보완하는 TCP의 신뢰성 있는
데이터 전송에 대해 살펴봤다. 이제 올바르게 바이트를 수신했을 때 TCP의 수신
버퍼에 버퍼 오버플로우가 발생해 loss가 발생할 수 있는데 이를 해결하기 위한
'흐름 제어(flow control)'에 대해 살펴보도록 하자.**

## TCP - 흐름 제어(flow control)

흐름 제어 : TCP가 수신자의 버퍼를 오버플로우 시키는 것을 방지하기 위해
애플리케이션이 읽는 속도와 송신자가 전송하는 속도를 일치시키는 서비스
(혼잡 제어와 다르다.)

그렇다면 어떻게 흐름 제어가 가능할까?

TCP는 송신자가 수신자에게 receive window라는 변수를 알려줌으로써 흐름 제어가
가능하도록 한다.

다음의 변수들을 정의함으로써 이것이 가능하다.

- LastByteRead : 호스트 B의 애플리케이션 프로세스에 의해서 버퍼로부터 읽힌
  데이터 스트림의 마지막 바이트 수
- LastByte Recd : 호스트 B에서 네트워크로부터 도착하여 수신 버퍼에 저장된 데이터
  스트림의 마지막 바이트 수

rwnd(receive window) = RcvBuffer(수신 버퍼) - [LastByteRcvd - LastByteRead]

반면, 이러한 정보를 알고 송신할 때는 두 변수를 통해 오버플로우가 발생하지 않도록 한다.

- LastByteSent
- LastByteAcked

이 두 변수의 차이(LastByteSent - LastByteAcked)는 호스트가 연결에서 전송 확인 응답이
되지 않은 데이터의 양을 말한다. rwnd 값보다 작은 확인응답이 안 된 데이터의 양을
유지함으로써 오버플로우가 발생하지 않도록 한다.

LastByteSent - LastByteAcked <= rwnd

참고로 사소한 기술적인 문제점이 있는데 버퍼가 가득찼다는 것을 알려준 다음 전송할 데이터가
없어 다시 버퍼의 상태를 알려주지 않으면 그 사이에 버퍼가 비더라도 상대방은 전송을 해주지
못하는 문제점이 생긴다.

이를 해결하기 위해 TCP 명세서는 1바이트 데이터로 계속해서 전송하여 문제를 해결한다.

**이 때까지 TCP의 수신 버퍼의 오버플로우로 loss가 발생하는 문제를 해결하는 flow control에
대해 살펴봤다. 이제 네트워크가 혼잡해 라우터의 버퍼들에 오버플로우가 발생하여 손실이 발생하는
경우를 해결하는 congestion control에 대해 살펴보자. 먼저 congestion control의 원리를
살펴보자.**

## TCP - 혼잡제어의 원리

### 혼잡의 원인과 비용

먼저 왜 congestion이 발생하고 그 비용은 어떻게 되는지 살펴보자. 먼저 congestion은
많은 source에서 너무 많은 데이터를 너무 빠르게 네트워크가 처리하기 힘들 정도로 송신하기
때문이다.

이로 인해 두 가지 상황이 발생할 수 있다.

- long delays
- lost packets

실제 데이터보다 네트워크에 유입되는 데이터는 더 많다. 다음 그림을 통해 자세히 살펴보자.

![congestion_control_principle](/images/computer_networking/congestion_control_principle.png)

세로축은 중복 데이터를 제외한 순수한 처리량을 의미한다. 그런데 일정 수준을 넘어서면 거의 처리가 되지
않는다. 다음의 비용 때문에 이러한 상황이 발생한다.

- 네트워크 혼잡으로 인한 큐잉 지연
- 라우터 버퍼 오버플로우로 인한 재전송
- 긴 지연으로 인한 송신자의 불필요한 재전송
- 경로 상에서 버려질 때 발생하는 버려지는 지점까지의 헛된 전송 용량

이러한 이유로 네트워크에 유입되는 데이터가 더 많아지는 것이다.

\*\*이러한 혼잡을 제어하기 위해 두 가지 접근법을 취할 수 있다.

### Congestion control에 대한 접근법

- 1. 종단간의 혼잡제어(end to end congestion control)

실제 혼잡이 발생하는 곳은 네트워크이지만 이에 대해 IP 계층은
어떠한 피드백도 제공하지 않는 구조다. 즉, 종단에서 혼잡을 처리해야
하는 접근이다. TCP가 채택하고 있는 방식이다.

이유는 무엇일까? TCP은 internet protocol 중 하나이고, internet
protocol은 모든 복잡성을 edge로 넘기는 철학을 가지고 있다.

- 2. 네트워크 지원 혼잡제어(network-assisted congestion control)

라우터가 실제로 네트워크에 피드백을 주는 구조다. 실제 혼잡이 발생하는
곳은 라우터가 있는 네트워크이고 가장 먼저 그 증상을 파악할 수 있다.
그래서 라우터가 피드백을 주는 방식이다. 방법은 아래의 2가지가 있다.

- 혼잡을 나타내는 하나의 비트 제공(single bit indicating congestion)
- 전송률을 송신자에게 전달(explicit rate for sender to send at)

**이러한 접근들을 바탕으로 혼잡 제어를 수행할 수 있다. 이제 혼잡 제어에
대해 살펴보도록 하자.**

## TCP - 혼잡 제어

TCP가 어떻게 혼잡 제어를 수행하는지 살펴보도록 하자. 먼저 TCP는 네트워크 지원
혼잡제어가 아닌 종단간의 혼잡제어를 사용한다.

이 때 TCP가 취한 접근방법은 네트워크 혼잡에 따라 연결에 트래픽을 보내는 전송률을
각 송신자가 제한하도록 하는 것이다. 만약 혼잡이 없으면 송신률을 높이고 혼잡을
감지하면 송신율을 줄이는 방식이다.

### 1. 어떻게 송신률을 제한하는가? - congestion window(cwnd)

TCP의 혼잡 제어 메커니즘은 congestion window 변수를 유지함으로 가능하다.
앞서 살펴본 아래 변수 2가지를 사용한다.

- LastByteSent
- LastByteAcked

LastByteSent - LastByteAcked("in-flight 상태의 데이터") <= min{cwnd, rwnd}

TCP는 위 제한을 유지한다. cwnd값을 조절함으로써 송신율을 제어하는 것이다.

RTT 시간이 지나면 ACK이 송신자에게 간다. 이 때 송신자는 cwnd만큼 내보냈을 것이다.
그러므로 송신율은 대략 다음과 같아진다.

```
rate ~ cwnd / rate(bytes/sec)
```

그렇다면 혼잡은 어떻게 감지할 수 있을까?

### 2. 어떻게 혼잡을 감지하는가?

- 1. packet loss

과도한 혼잡이 발생하면 하나 이상의 라우터의 버퍼들이 오버플로우되고, 그 결과
데이터그램이 버려질 수 있다. 이렇게 되면 타임아웃 또는 triple-duplicate-ACK이
발생할 수 있다. 따라서 손실된 세그먼트가 혼잡을 의미하며, TCP는 이 때 전송률을
줄인다.

- 2. ACK의 도착

확인응답이 도착한다는 말은 송신자로부터 수신자까지 성공적으로 전송되었고, 네트워크가
혼잡하지 않다는 표시로 받아들인다.

- 3. 전송률 탐색

TCP 송신자는 결국 혼잡이 발생하는 시점까지 전송률을 증가시키고, 손실 이벤트가 발생하는
시점에서 전송률을 줄이는 것이다.

### 3. 혼잡을 감지한 다음 송신률을 변화시키기 위해 어떤 알고리즘을 사용하는가?

#### Slow Start

연결이 시작되면 cwnd는 1MSS byte로 초기화된다. 즉, Segment를 한 개 내보낼 수
있는 것이다. 그리고 매 RTT마다 cwnd 값을 두배로 키운다. 이는 ACK이 들어올 때마다
cwnd 값을 키우는 방식으로 진행한다. 즉, 처음 rate는 slow하지만 지수적으로 빠르게
증가하므로 Slow Start라고 하는 것이다.

#### Congestion Avoidance

그렇다면 Slow Start에서 지수적으로 증가시키다가 언제 멈추어야 할까? ssthresh
(slow start threshold)라는 임계값에 도달하면 Congestion avoidance 모드에
들어간다. (여기서 ssthresh는 초기에 OS에 의해 결정된다. 초기에는 네트워크 상황을
알 수 없기 때문이다.)

Congestion avoidance에 들어가면 원래 2배씩 키워나가던 cwnd를 하나의 MSS만큼 키워나간다.
ACK을 받을 때마다 하나의 MSS만큼 키우는 것이다.

### loss가 발생했을 때

TCP는 cwnd를 꾸준히 늘리다가 loss가 발생하면 줄여주어야 한다. TCP는 어떻게 loss를
감지하는가? 손실 이벤트로 감지한다. 바로 타임아웃과 triple duplicate ACKs다.

그래서 TCP 버전에 따라 다음과 같이 진행된다.

- TCP RENO
  - time-out : cwnd -> 1MSS
  - 3 duplicate ACKs : cwnd/2
    (time-out은 cwnd를 1MSS로 뚝 떨어뜨리지만 3 duplicate ACKs는
    반으로만 줄이는 이유는 loss가 발생했지만 나머지 세그먼트들은 전송되고 있다는
    사실 때문이다. 이러한 단서로 상황을 조금 더 낙관적으로 보는 것이다.)
- TCP Tahoe
  - cwnd -> 1MSS (time-out, 3-duplicate ACKs 모두)

또, ssthresh 값은 위 두 버전 모두 현재 cwnd의 절반이 된다. (ssthresh = cwnd / 2)
이 ssthresh의 값에 따라 변화가 결정되는 것이다.

이 때 timeout이 발생한다면?

- cwnd -> 1MSS
  - 1MSS 이므로 슬로 스타트 상황대로 빠르게 지수적으로 증가시킨다.

3 duplicate ACKs가 발생한다면?

- ssthresh -> cwnd / 2
- cwnd -> cwnd / 2
  - 두 값이 같아지므로 1MSS씩 천천히 조심스럽게 증가시킨다.

**앞서 혼잡 상황에서의 비용에 대해 이야기했었다. 위의 내용을 고려하여 TCP 성능
지표에 대해 알아보자.**

### TCP throughput

먼저 처리율이다. RTT동안 K개를 보낼 수 있다면 throughput은 K/RTT가 된다.
그런데 TCP는 window 사이즈를 조금씩 계속 올려주다가 loss가 발생할 수 있는데
이 때의 값을 W라고 하자. 그러면 결국 window 사이즈는 W와 W/2 사이를 왔다갔다
하게 된다.

그래서 평균 TCP 처리율은 다음과 같다.

```
(w + w/2) * (1/2) = (3/4)W/RTT (bytes/sec)
```

그런데 TCP가 파이프라이닝을 하는 이유는 TCP의 throughput을 위한 것이다. 그런데
기술이 발달하면서 bandwidth가 커지고 pipe가 long하고 fat하게 된 것이다. 그래서
window size가 커져야 하게 된 것이다.
그렇다면 RTT는 무엇을 반영하는가? pipe의 길이다.

Mathis의 연구에 따르면 TCP의 처리율은 다음과 같다.

![TCP_throughput](/images/computer_networking/TCP_throughput.png)

여기서 L은 segment loss probability다. 즉, Loss가 커지면 원하는 throughput을
못 내게 되는 것이다.

다음 성능 지표는 공평성(fairness)다.

### TCP fairness

공평성은 처리율과 함께 가장 먼저 참고되는 성능 측정치다. 단위시간당 많은 일을 하면
좋지만 fair하지 못하다면? 어떤 연결은 starvation이 생긴다.

그래서 공평성의 목표는 K개의 세션이 병목 링크의 대역폭 R을 R/K로 공평하게 나누어
쓸 수 있도록 하는 것이다.

![TCP_throughput_fairness](/images/computer_networking/TCP_throughput_fairness.png)

위 그림에서 실선은 두 연결이 병목 링크의 전체 대역폭을 모두 사용하는 경우를 말한다. 그러다
이 선을 넘으면 loss가 발생해서 다시 줄이고 throughput을 위해 키우는 것을 반복하게 된다.
목적은 위 그림에서의 가운데 선에 가깝게 위치하게 되도록 하는 것이다. 하지만 쉽지 않다.
또 하나의 전제는 병목 링크로부터의 거리가 같을 때가 전제이기 때문이다. 하지만 쉽지 않고
가까운 쪽이 더 빠르게 키우게 된다.

참고로 TCP fairness에 영향을 미치는 몇 가지 요소들이 있다. 먼저, UDP가 있다. UDP 통신은
loss가 발생해도 반응하지 않는 방식이다. 그래서 TCP 관점에서 UDP 상에서 수행되는 애플리케이션은
공평하지 못하다.

또 하나는 병렬 TCP 연결이다. 여러 개의 TCP 연결 중 하나의 TCP 연결이 병렬 TCP 연결을 사용하면
불공정한 할당을 받게 되는 것이다.

**이 때까지 TCP의 제어 메커니즘에 대해 살펴봤다. 마지막은 TCP의 연결이 어떻게
설정되고 해제되는지 자세히 살펴보도록 하자.**

## TCP 연결 관리

### Why 3-way handshaking?

TCP는 연결을 설정할 때 3-way handshaking을 한다. 그렇다면 왜 3-way handshaking을
하는 것일까?

![two_way_handshaking_failure](/images/computer_networking/two_way_handshaking_failure.png)

사람은 2-way handshaking이면 되지만 TCP에서는 문제가 생긴다. 이유는 예를 들어 Client가
Server에 연결 요청을 보내고 그에 대한 ACK에 지연이 생겨 timeout이 발생했다고 생각해보자.
결국 ACK 응답이 도착해서 Client는 ESTABLISHED 상태가 되지만 그 전에 연결 요청을 재전송하게
된다. 그래서 서버는 2-way handshaking이니 이미 ESTABLISHED 상태이고 신경 쓰지 않는다.
그리고 마지막으로 Client가 연결을 종료하고 재전송되었던 연결 요청이 서버에 도착했다고
생각해보자. 이렇게 되면 서버는 half-open connection 상태가 된다. 여기에 재전송된
데이터가 half open connection 상태에 도착하면 더욱 문제가 생길 수 있는 것이다.

그래서 다음과 같이 3-way handshaking을 한다.

![three_way_handshaking](/images/computer_networking/three_way_handshaking.png)

SYN 메세지까지만 받았을 때는 client가 alive 상태라는 확신을 할 수 없다. 그러므로
SYNACK을 보낸 후 ACK 메세지를 받고 나서 두 호스트가 alive 상태라는 것을 확인하는 것이다.

### TCP 상태 전이도

유한 상태 머신으로 살펴보면 다음과 같다.

![tcp_finite_state_machine](/images/computer_networking/tcp_finite_state_machine.png)

다음은 TCP 상태 전이도다. 실제 사용하는 함수와 함께 일목 요연하게 정리되어 있다.

![TCP_state_diagram](/images/computer_networking/TCP_state_diagram.png)
(출처 : Advanced! 리눅스 시스템 네트워크 프로그래밍 - 김선영님 저자)

### TCP 연결 종료

TCP는 Full duplex connection이지만 close는 각 stream에 대해 독립적으로 이루어진다.

![TCP_connection_close](/images/computer_networking/TCP_connection_close.png)

종료에서도 3-way hand-shaking은 똑같이 적용된다. 그리고 관심 있게 볼 것은 마지막에
TIME_WAIT이 발생한다는 것이다. 이는 active close를 행한 측에서 발생하는 정상적인
상황이다.

이 TIEM_WAIT이 필요한 이유는 세그먼트의 유실이나 지연으로 인해 종료 후 비정상적인
패킷을 받을 경우를 대비하기 위해서다. 예를 들어 서버가 FIN을 보냈는데 ACK을 받지
못했다고 생각해보자. 그렇게 되면 client는 소켓을 닫았는데 서버는 제한된 수치까지
계속 보내게 된다.

그래서 TIME_WAIT 상태를 두는 것이고 재전송된 패킷은 버리거나 응답하는 것이다.

아래는 TIME_WAIT와 관련된 Advanced! 리눅스 시스템 프로그래밍 김선영 저자님의
글이다.

TCP의 TIME_WAIT을 없애는 법
http://sunyzero.tistory.com/198
