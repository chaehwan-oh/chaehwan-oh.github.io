---
layout: post
subclass: post
title: "컴퓨터 네트워크 Chapter2. 애플리케이션 계층"
date: 2018-10-06 00:00:02
tags: [dudaji, computer-science, computer_networking]
excerpt: "컴퓨터 네트워크 Chapter2. 애플리케이션 계층에 대한 정리입니다."
disqus: True
---

## 2장 애플리케이션 계층 단원의 개념 구조도

![network_ch2_recap](/images/computer_networking/network_ch2_recap.jpg)

**Overview는 끝났고 2단원부터 각 프로토콜 계층에 대해 살펴보도록 하자.**

## 네트워크 애플리케이션의 동작 원리

### 네트워크 애플리케이션의 구조

네트워크 애플리케이션이 동작하는데 있어 구조는 2가지 방식이 가능하다.

- Client-Server
- Peer-to-Peer

하나는 'Client-Server'구조이고 하나는 'Peer-to-Peer' 구조다.
네트워크 애플리케이션 프로그램이 두 가지 구조 중 하나이기도 하고,
프로그램을 지원하는 프로토콜도 이 2가지 구조 중 하나의 구조를 취하게 된다.

#### Client-Server

먼저 Client-Server 구조를 살펴보면 Client와 Server로 구성된다.

Client와 Server의 특성을 살펴보면 다음과 같다.

Server

- always-on host : 항상 켜져 있는 호스트다.
- permanent IP address : 영구적인 IP 주소를 가진다.
- data centers for scaling : 규모에 따라 데이터 센터의 형태일 수 있다.

Client

- do not communicate directly with each other : Client간에는 통신하지 않는다.
- may be intermittently connected : 서버처럼 네트워크에 항상 접속해 있는 것이 아니라
  간간이 접속할 수도 있다.
- may have dynamic IP addresses : 동적인 IP 주소를 가진다.

#### Peer-to-Peer

- Client-Server 구조와 다르게 통신하는 process가 모두 user host에 있다.
  즉, 항상 켜져 있는 서버가 없다.(no always on server)
- 각각의 end system들이 직접 통신하는 방식이다. 여기서 각각의 통신하는 end system을
  peer라고 한다.
- peer는 service를 요청하기도 하고 제공하기도 한다.
- Peer-to-Peer 구조의 대표적인 장점은 Self-Scalability다.
- peer 각각이 user host이므로 항상 접속해 있는 것이 아니라 간간이 접속해서 네트워크에
  참여하는 방식이다. 그래서 관리가 어렵다.

**이 2가지 구조는 host간의 관계를 통해 2가지 구조로 나누어봤다. 하지만 통신의 주체는
호스트가 아니라 프로세스다. 결국 서로 다른 호스트에서 프로세스가 메세지를 교환하는 것이다.
그래서 살펴볼 것이 프로세스간 통신이다.**

### 프로세스간 통신

여기서 통신하는 프로세스는 2가지 종류로 나누어진다.

- Client Process : process that initiates communication
- Server Process : process that waits to be contacted

앞서 살펴본 애플리케이션 구조와 함께 살펴보면 Client-Server 구조에서는 Client 프로세스는
Client 호스트에서 동작하고 Server 프로세스는 Server 호스트에서 동작한다.

반면, Peer-to-Peer 구조에서는 하나의 Peer가 Client Process와 Server Process를 동시에 실행할
수 있다.

이 프로세스들이 통신하기 위해서는 크게 3가지가 필요하다.

#### 소켓(Socket)

앞서 살펴본 protocol layer에서 application layer에서는 message를 만들어서 transport layer에
process to process delivery를 부탁했다.

그런데 Application Process는 application developer가 control하는 영역이고, transport 계층 이하는
운영체제가 control하는 영역이다. 그래서 결국 application 계층과 transport 계층간의 인터페이스가
필요한데 그 역할을 하는 것이 소켓이다.

![socket](/images/computer_networking/socket.png)

#### 프로세스 주소 배정

두번째로 필요한 것은 어떻게 목적지를 식별할 것인가이다. 메세지가 전달되어야 하는 궁극적인 목적지는
host가 아니라 process다. 그러므로 프로세스를 식별할 수 있어야 한다.

일상생활에서 우편물을 보낼 때 우리의 최종 목적지는 그 집이 아니라 그 집에 있는 특정 수신인이다.
이 수신인을 식별하는 역할을 하는 것이 포트 번호다.

```
주소 -> IP주소
사람이름 -> port number
```

참고로 포트 번호 중 Well-known port number가 있다. 예를 들어 HTTP Server의 port 번호는
언제나 80으로 고정되어 있다. 또, mail server의 경우 25번으로 고정되어 있다.

이유는 여러 클라이언트가 접근하는데 최종 목적지의 port number를 명시해야 쉽게 접근이 가능하기
때문이다. 그래서 IP주소는 다 다르지만 port number를 같게 해서 쉽게 찾을 수 있도록 하는 것이다.

#### Which transport service?

application에서는 transport 계층에 process to process delivery를 맡기는데 어떤 transport
서비스가 필요할까? 이는 서비스의 요구사항(service requirement)에 따라 결정된다.

이러한 서비스(소프트웨어)의 요구사항은 다음과 같다.

- data integrity(데이터 무결성)
- timing
- throughput
- security

즉, application마다 중요한 기준이 있는 것이다.

예를 들어 다음과 같은 서비스는 data integrity가 굉장히 중요하다.

- file transfer
- e-mail
- web document
- text messaging

파일을 다운로드받는다고 했을 때 그 파일이 온전히 다운로드되어야 하지만
엄청난 속도로 바로 다운로드가 되어야 하는 것은 아니다. 물론 그렇게 되면
좋겠지만 그보다 데이터 무결성이 더욱 중요한 것이다. 텍스트 메세지를 보낼 때도
마찬가지다. 내용이 제대로 전달되어야 소통에 문제가 없다.

다음과 같은 서비스는 time sensitivity가 굉장히 중요하다.

- real-time audio
- video
- stored audio/video
- interactive games

interactive game의 경우 순간순간 바로 피드백이 와야 한다. video도 마찬가지다.
게다가 재생속도에 맞추어 영상이 진행되려면 minimum throughput도 만족시켜야 한다.

반면, loss-tolerant하다. loss는 어느 정도 수용이 가능하다. 사람의 눈으로는 영상을
보는데 어느 정도 loss가 있더라도 인지하는데 지장이 없기 때문이다.

이러한 기준들을 바탕으로 어떤 Transport 서비스를 사용할 것인지를 결정해야 한다.

transport 서비스에는 TCP 프로토콜과 UDP 프로토콜이 있다.

#### TCP VS UDP

하나씩 살펴보도록 하자.

##### TCP

- reliable transport 제공
- connection-oriented한 동작을 통해 flow control과 congestion control이 가능

##### flow control

flow control을 잠시 살펴보면 두 TCP가 있을 때 두 TCP는 buffer를 가진다.

sending TCP와 receiving TCP라고 했을 때 sending TCP는 먼저 전송할 데이터를
받으면 일단 buffer에 담는다. 그 다음 receiving TCP로 내보낸다. receiving
TCP는 받으면 buffer에 담고 그 다음 application layer에 올린다.

이 때 receiving TCP의 buffer가 유한하기 때문에 sending TCP가 왕창 계속 보내면
receiving TCP의 buffer가 꽉 찰 수 있다.
(receiving TCP가 읽어들이는 속도보다 Sending TCP가 내보내는 속도가 더 빠르면
이런 상황이 생긴다.)

그러니 connection을 통해 receving TCP에서 너무 빠르니까 slow down을 해달라고
Sending TCP에서 전달하는 것이다.

##### congestion control

congestion control은 Sending TCP와 Receving TCP 사이의 네트워크에 있는 router들을
거쳐서 데이터들이 도달하는데 그 사이의 Router의 buffer가 꽉 차는 경우가 생길 수 있다.
congestion 때문에 말이다. 이 때 connection을 통해 네트워크 상황을 알림으로써 내보내는
패킷의 전송률을 줄이도록 한다. 이를 'congestion control'이라고 한다.

##### flow control vs congestion control

flow control은 receiver의 buffer를 overwhelming하지 않도록 하는 control이고,
congestion control은 network의 congestion을 피하기 위한 control이다.

##### UDP

- unreliable data transfer 제공

이렇게만 봤을 때는 왜 UDP를 사용할까? 라는 생각이 들 수 있다. 하지만 상황에 따라
UDP를 사용할 때가 있다.

#### Why UDP?

##### protocol control overhead

TCP의 경우 connection을 setup하고 관리하는 오버헤드가 발생한다. 이를
'protocol control overhead'라고 한다.

예를 들어 one time transaction이라고 생각해보자. 이 경우 한 번만 전송하면
되는데 connection을 setup하고 관리하는데 이는 배보다 배꼽이 더 커지는 것이다.
그래서 이런 상황에는 UDP가 더 나을 수 있다.

##### critical data integrity

반대로 data integrity가 굉장히 중요한 상황에는 오히려 UDP를 사용할 수 있다.
왜냐하면 data integrity가 굉장히 중요하다면 transport 계층에만 data integrity를
의존하지 않을 것이기 때문이다. application layer에서 data integrity check를
할 수 있다. 그렇게 되면 application layer와 transport layer가 반복적인 작업을
하게 되는 것이다. 이 때는 오히려 simple한 UDP를 사용할 수 있다.

##### the problem of flow control and congestion control

또 다른 경우는 TCP의 flow control과 congestion control이 문제가 될 수 있다. 예를 들어
멀티미디어 스트리밍의 경우 minimum throughput gurantee가 필요하다. 그런데 TCP의 경우
congestion이 심하다면 메세지를 내보내도 hold하고 있을 수 있다. 그렇기 때문에 minimum
throughput gurantee를 transport 계층에서 막을 수 있다. 그래서 이런 TCP를 사용하는 것은
좋지 않다. 이 때 UDP를 사용할 수 있다.

TCP와 UDP를 살펴봤는데 timing과 minimum throughput gurantee, security를 둘다 제공해주지는
않는다.

현재 위 기능을 제공해주는 프로토콜은 없다. 연구가 없었던 것은 아니지만 실제 사용되는 것은 거의
없다. 아니면 TCP와 UDP를 보완하는 방식으로 사용된다.

현재 data integrity를 보장해주는가 그렇지 않은가로 나누어질 뿐이다. 왜냐하면 두 프로토콜 모두
인터넷 초기에 만들어졌고 그 당시는 인터넷에서 멀티미디어가 발달하지 않았을 때이기 때문이다.

**이 때까지 프로세스간에 어떻게 통신이 가능한가를 살펴봤다. 이 때 프로세스는 소켓을 통해
메세지를 주고 받는데 이 메세지는 어떻게 구성되는지 살펴보도록 하자.**

### application layer protocol

application layer protocol에서 정의해야 하는 부분은 다음과 같다.

- types of messages
  : 교환하는 메세지의 종류
  ex) request, response

- message syntax
  : 어떤 필드들이 있고 어떻게 구분되는가?
  (what fields in messages & how fields are delineated)

- message semantics
  : meaning of information in fields

- rules
  : rules for when and how processes send & respond to messages

참고) application layer에는 두 종류가 있다.

- open protocols
  : defined in RFC, allows for interoperability
  ex) HTTP, SMTP

- proprietary protocols ex) Skype
  : 어떻게 동작하는지 모른다.

**이제부터는 5가지 정도의 실질적인 protocol을 살펴볼 것이다. 하나씩 살펴보자.**

## HTTP(HyperText Transfer Protocol)

### HTTP의 특징

- HTTP는 Web을 위해 만들어진 프로토콜이다.
- Client-Server 구조를 따르고 있다.
- transport 서비스로 TCP를 사용한다.
  - 왜? Web을 지원하기 만들어졌기 때문에 Web 자체가 data integrity가 중요하기
    때문에 TCP를 사용한다.
  - TCP가 사용되는 간략한 순서
    - TCP를 사용하기 떄문에 HTTP request message를 송신하기 전에 Client 측에서
      TCP connection을 초기화한다.
    - HTTP Server는 항상 80번 port이기 때문에 이 port에 접촉한다.
    - Server가 받아들이면 연결이 맺어진다.
    - connection을 통해 HTTP message를 주고 받는다.
    - TCP connection이 close되고 통신이 종료된다.
- HTTP 프로토콜은 stateless하다. 즉, 이전에 보낸 요청을 전혀 기억하지 않는다.
  - 왜? stateful하려면 request history를 어딘가 저장하는 오버헤드, 그리고
    crash(충돌)이 발생하면 이를 해결하는 데에 오버헤드가 발생한다. 그러므로
    이를 피하기 위해 stateless하게 통신한다.

### HTTP의 동작 방식 2가지

- Non-persistent HTTP
- Persitent HTTP

#### Non-persistent HTTP

초기 HTTP는 non-persistent HTTP였다. 이 말은 server에 object를 하나 보내주고 나면
바로 TCP connection을 close해버린다는 말이다.

그런데 web page는 하나의 object로 구성되지 않는다. 그래서 10개의 object라면 TCP
connection을 10번 맺어야 한다.

간략한 동작방식을 살펴보도록 하자.

```
1) HTTP Client initiates TCP connection to HTTP Server
(browser에서 URL 입력 후 엔터)
2) HTTP Server(always-on) 의 연결 승낙
3) Client : HTTP TCP 소켓을 통해 request message 송신
4) Server : TCP socket을 통해 전송됨.
5) Server : response message를 작성해서 Client로 전달
(이 때 requested object를 실고 있음)
6) Server: TCP connection closed
7) Client : response message를 받고 추가로 필요한 object 발견
(받은 object를 display하고 필요한 object를 파악해서 다시 요청)

(1번에서 7번까지의 과정을 반복)
```

참고) 위 과정에서 TCP connection이 맺어졌다는 말의 의미는 무엇일까?

여기서의 소켓이 생성되었다는 말이다. 그래서 message를 주고 받는다는
말은 이 socket을 통해 message를 주고 받는다는 말이다.

그렇다면 응답시간은 어떻게 될까?

응답시간은 user가 요청을 보내고 웹 페이지가 보일 때까지의 시간이다.

RTT(Round-trip Time)

작은 패킷을 Server로 보내고 다시 Client로 돌아오는 데까지 걸리는 시간

먼저 TCP connection에 1RTT가 필요하다. 그리고 요청을 보내고 응답하는데
1RTT가 필요하다. 그리고 요청을 보내고 응답하는데 1RTT가 걸린다.
마지막으로 file transmission time을 추가해야 한다.

그래서 2RTT + file transmission time 만큼 걸린다.

그런데 Non-persistent이므로 이미 필요한 object를 다 받은 것이 아니다.
base HTML까지만 받았을 뿐이다.

그러니 웹 페이지 전체를 받는데 걸리는 시간을 생각해보아야 한다.

Non-persistent HTTP는 매번 TCP 연결을 해주어야 하므로 매 object마다
2번의 RTT가 필요하다.

(참고로 이러한 비효율성 때문에 병렬로 TCP 연결을 동시에 수행하는 방법도
있다. 소켓이 동시에 여러 개 열리는 것이다. 하지만 운영체제 입장에서는
TCP 연결로 인한 오버헤드가 클 수 있다. 소켓도 운영체제의 자원이고 TCP마다
buffer도 필요하다.)

#### Persistent HTTP

그래서 초기는 non-persistent HTTP였지만 persistent HTTP가 나오게 됐다.
persistent HTTP는 Server가 object 하나를 내보내고도 TCP 연결을 끊지 않고
놓아둔다. 그래서 추가적으로 들어오는 request 요청을 받고 응답해줄 수 있다.

그렇다면 어떻게 동작할까?

맨 처음에는 TCP connection을 맺어야 한다. 그리고 base HTML file을 받는
데에도 RTT가 한 번 더 필요하다. 하지만 이 때 base HTML file을 받으면
추가적으로 필요로 하는 object들을 파악할 수 있다.

예를 들어 필요한 object 수가 5개라면 5개를 요청하고 받을 때까지 TCP
connection을 끊지 않는다. 이어지는 요청 간 interval이 거의 없다면
거의 1RTT만에 object들을 받는다.

**HTTP의 동작 방식을 살펴봤으니 동작할 때 주고 받는 메세지에 대해
살펴보자.**

### HTTP의 메세지

message에는 두 종류가 있다. request와 response가 있다.

#### Request message

먼저 HTTP request message를 살펴보자.

- HTTP request message는 ASCII로 표현되어 있다.
- HTTP reqeust의 첫 줄은 request line이라고 한다.

```
GET /index.html HTTP/1.1\r\n
```

- 그 다음에는 header line이 나온다.

```
Host : www.cs.umass.edu\r\n
(Server의 Host를 말한다.)
User-agent : Firefox/3.6.10\r\n
(Client의 browser를 말한다.)
Accept-Language : en-us, en; q = 0.5\r\n
(Web page마다 각 언어의 version을 두고 있을 수도 있다.)
Connection : keep-alive\r\n
(TCP connection을 계속 유지하라는 말이다. persistent방식)
```

다음은 HTTP은 HTTP request 메세지의 일반적인 형식이다.

![http_request_message](/images/computer_networking/http_request_message.png)

위에서 Version은 Client가 사용하는 HTTP version을 말하고, cr(carriage return)과
lf(line feed)를 통해 각 내용을 구분해준다.

#### HTTP method

그런데 경우에 따라 form에 input을 받는 경우도 있다. 예를 들어 검색 엔진에서
검색창에 입력을 하는 것도 form에 input을 받는 것이다. 이 input도 Server에 전달되어야
한다.

이 때 방법들이 있는데 첫 번째 방법은 Post method를 사용하는 방법이다.

##### Post method

- web page often includes form input
- input is uploaded to server in entity body
  (body에 내용을 실어서 나른다.)

##### URL method

- uses get method
- input is uploaded in URL field of request line

(www.~.com?monkeys&bannana 와 같이 사용되는 방식이다.)

그 외의 method들도 있다.

#### method types

HTTP/1.0 : HTTP version

- GET
- POST
- HEAD
  - asks server to leave requested object out of response
  - 실제 파일은 보내지 말고 응답만 요청하는 경우다. 테스트 목적으로 사용된다.

HTTP/1.1 :

- GET, POST, HEAD
- PUT
  - upload file in entity body to path specified in URL field
- DELETE
  - deletes file specified in the URL field

PUT과 DELETE를 통해 원격에서 서버의 내용 수정이 가능해진다. 그러므로 사용자의
접근이 일부로 제한된다.

#### Response message

가장 위에는 Status line이 온다.

```
HTTP/1.1 200 OK\r\n
```

![http_response_message](/images/computer_networking/http_response_message.png)

그렇다면 앞서 살펴본 response message에 담기는 status code를 살펴보도록 하자.

```
200 Ok
301 Moved Permanently(다른 곳에 저장되었다는 말이다.)
400 Bad Request
404 Not Found
505 HTTP Version Not Supported
```

**이 때까지 웹 애플리케이션을 위한 HTTP 프로토콜의 동작방식과 메세지를 살펴봤다.
앞서 HTTP는 stateless하다는 것을 살펴봤는데 그로 인해 필요한 것이 Cookie다.
Cookie에 대해 살펴보도록 하자.**

### Cookie

Cookie를 사용하는 목적은 HTTP가 stateless protocol인데 HTTP가 stateless지만
Server-side에서 request의 history를 저장하고 있기 위해 사용된다. HTTP가
stateless여서 좋은 점은 protocol overhead가 줄어든다는 점이었다. 대신 transaction의
history를 저장함으로써 얻을 수 있는 유용한 일을 할 수 없게 된다. 이를 보완하기 위해
Cookie가 사용된다.

Cookie를 사용하려면 HTTP response message에 Cookie의 header line을 넣어서 보내주어야 한다.

이렇게 Cookie header line이 포함된 HTTP response message를 받은 다음에는 Client 쪽에서도
HTTP request message에 cookie header line을 넣어서 보내주어야 한다.

user's browser에서는 cookie file을 유지하고 web site에서는 back-end database를 유지한다.

![cookie](/images/computer_networking/cookie.png)

최초 접속 시 unique ID를 할당해준다. 그리고 그 다음부터는 request를 보낼 때마다 그
unique ID를 달고 request를 보내게 된다. 그리고 site에서는 ID에 대한 transaction
data를 backend DB에 저장한다.

Cookie를 사용하면 어떤 일들이 가능할까?

사용자 입장에서 가장 큰 것은 user session state를 유지할 수 있다는 것이다.
예를 들어 웹 이메일을 보낼 때 log in 상태를 유지한다거나 웹 쇼핑몰에서
장바구니 기능을 사용하는 것이 그 예다. 이를 통해 Server 입장에서는 사용자 정보를
계속 수집해서 이러한 정보들을 기반으로 추천도 가능하다.

**cookie에 대해 알아봤다. 그 다음은 HTTP Server의 역할을 대신 할 수 있는
Web cache다.**

### Web cache

![web_cache](/images/computer_networking/web_cache.png)

Web cache가 동작하는 방식은 먼저 Web cache를 통해 Web에 접근한다. 브라우저는
HTTP 요청을 모두 Web cache로 보내는 것이다. Web cache(proxy server)에 있으면
바로 응답하고, 없으면 원래 서버에 가서 받아서 응답한다.

그렇다면 Web cache는 HTTP 프로토콜에서 Client일까? Server일까? 정답은 두 가지
역할을 모두 해주어야 한다.

그 이유는 proxy server는 origin server에 대해서는 client로 동작한다. Client에
대해서는 Server로 동작한다.

그렇다면 누가 web cache를 사서 local site에 두게 될까?
대학, 회사, regional ISP들이다.

왜 그렇게 하는 것일까?

당장 생각해볼 수 있는 것은 사용자의 요청에 대한 응답시간이 상당히 줄어든다.
local site의 proxy server로부터 응답을 받을 수 있기 때문이다.

그런데 이것보다 더 큰 이유가 있다. 예를 들어 학교라고 가정해보자.

학교는 ISP에 바로 연결된다. 그런데 internet access link를 통해 다운로드되는 양이
많다면 internet access link의 bandwidth는 커야 한다.

이 access link는 ISP에서 사는데 매달 정기적으로 돈을 낸다. 만약 Web cache를 사면
이 link를 통해 왔다갔다 하는 트래픽을 줄일 수 있다.

만약 poor content provider라면 data center를 설치하는 것이 쉽지 않다. 그러므로
edge쪽에 Web cache를 쭉 설치해두면 Server가 한 귀퉁이에 있더라도 효과를 볼 수
있게 되는 것이다.

그렇다면 웹 캐시를 사용하는 상황을 한 번 생각해보자.

![web_cache_scenario](/images/computer_networking/web_cache_scenario.png)

평균 object size : 1Mbits
평균 request amount : 15 object/sec
access link rate : 15.4 Mbps(bandwidth)

이 때 traffic intensity를 계산해보자.

traffic intensity(link utilization):
traffic 유입속도 / bandwidth

traffic 유입속도 : 15Mbps
bandwidth : 15.4Mbps

이렇게 되면 99% 문제다. 앞서 traffic intensity가 0.7 정도만 넘어도
delay가 매우 커지는 것을 봤다. 그러므로 위와 같은 상황이 되면
매우 인터넷이 매우 느려진다.

이를 해결하는 방법은 크게 2가지다.

첫번째 방법은 access link의 대역폭을 키우는 방법이다. 하지만 이는 Cost가
비싸다.

두번째 방법은 local web cache를 두는 방법이다. 이를 통해 traffic intensity를
낮출 수 있다. 첫번째 방법에 비해 저렴하고 ISP에 정기적으로 지불하지 않아도 되므로
고정비로 해결할 수 있는 방법이다.

그렇다면 delay는 얼마나 줄일 수 있을까?

cache hit rate이 0.4라고 생각해보자.

이 때 40%는 cache에서 해결되고, 60%는 Server에서 해결된다.

total delay
= 0.6 _ (from server) + 0.4 _ (from cache)

이 경우 서버에서는 2.01 그리고 cache는 milliseconds 정도로 계산된다.

= 1.2 secs

local에 cache를 둔 경우가 더 저렴하면서 성능이 더 좋다.

#### Conditional GET

하지만 local에 cache를 두는 경우에도 문제점은 있다. 바로 original server에서
내용이 수정된 경우다. web cache는 이를 모르고 계속 과거 정보를 주게 된다. 그래서
무작정 주지 않고 최신 정보인지를 확인한다. 이를 위해 사용하는 것이
'Conditional GET method'이다.

Conditional GET method는 HTTP request header로 다음을 넣어준다.

```
'If-modified-since: <date>'
```

object가 수정되지 않으면 응답은 하는데 object는 실어보내지 않는다.
오버헤드를 크게 만들지 않기 위해서다. 그리고 수정이 되었다면 200 ok와
data를 실어서 보내준다.

만약 request할 때마다 매번 확인한다면 out-of-date인 page를 줄 일이 없다. 하지만
매번 Server를 거쳐야 하는 오버헤드가 발생할 수 있다.

**다음으로 살펴볼 프로토콜은 이메일과 관련된 프로토콜들이다.**

## e-mail protocol

크게 3가지 주요부분으로 나누어진다.

- user agents : 메일을 읽고 쓰게 해주는 프로그램을 말한다.
- mail servers
  : 나가고 들어오는 메세지들이 저장된다.
  - mailbox : 유저들을 위해 들어오는 메세지들이 저장된다.
  - message queue : 송신될 메세지들이 저장된다.(사용자의 구분이 없다.)
- mail protocols
  - simple mail transfer protocol(SMTP)
  - POP3, IMAP

다음은 메일 시스템의 구성이다.

![email_system](/images/computer_networking/email_system.png)

그리고 다음 메일 시스템에서의 시나리오다.

![email_scenario](/images/computer_networking/email_scenario.png)

### SMTP

그렇다면 SMTP protocol의 Client와 Server는 누가 될까?
이 경우 Alice의 메일 서버의 서버 프로세스가 Client가 된다.

헷갈릴 수 있지만 먼저 접촉하는 쪽이 Client이므로 Alice의 메일 서버의
서버 프로세스다.

SMTP는 기본적으로 TCP 위에서 동작한다. 이 역시도 data integrity를
위해서다. TCP를 사용하기 때문에 TCP connection을 맺어야 한다.
참고로 SMTP는 port25번을 사용한다. (well known port number)

3단계로 진행된다.

- handshaking(greeting)
- transfer of messages
- closure

command(client)/response(server) 구조를 가진다.

Commands는 ASCII text이고, response는 Status code 와 phase를 나타낸다.
(message는 7bit ASCII이어야 한다.)

이런 점에서 HTTP와 굉장히 유사하다. 하지만 차이점이 있다.

HTTP : pull protocol (Server에서 정보를 가져온다.)
SMTP : push protocol (Server에 정보를 전달한다.)

### mail access protocol : POP3, IMAP

다음은 이메일 프로토콜 중 POP3와 IMAP이다. 다시 다음 그림을 살펴보면

![email_scenario](/images/computer_networking/email_scenario.png)

SMTP 프로토콜을 이용해 push하지만 마지막 receiver는 정보를 받아와야 한다.
즉, mail에 접근해야 한다. 이 때 사용하는 것이 mail access protocol이다.
mail access protocol에는 POP3, IMAP, HTTP가 있다.

하나씩 살펴보도록 하자.

#### POP 프로토콜

POP3 프로토콜이라고 한다. 크게 2개의 phrase로 구성된다.

- authorization phrase
  - user : username
  - pass : password
- transaction phrase
  - list : list message numbers
  - retr : retrieve message by number
  - dele : delete
  - quit

통신이 시작되면 먼저 authorization을 한 다음 transaction을 한다. SMTP와 유사하게
Client는 command를 보내고, Server는 response를 한다.

POP3에 있는 2가지 방식이 있다.

- download and delete : 다른 Client에서 읽는 것이 불가하다.
- download and keep : 다른 Client에서 읽는 수 있지만 새 mail처럼 보인다.

두 방식 모두 문제가 있다. 그래서 IMAP이 나온 것이다.

#### IMAP 프로토콜

POP3는 Session간 stateless하게 동작했다. 그래서 client가 Server에 있는 mail box에서
message들을 local machine으로 가져와 local에서 delete 등의 작업을 했다.
반면, IMAP는 client가 하는 일이 mail Server에서 반영되도록 한다.
Server에서 작업하는 것이다.

- keep all messages in one place : at server
- allow user to organize messages in folders
- keeps user state across sessions

POP3와 IMAP 중 무엇이 더 복잡할까? IMAP이 훨씬 복잡하다. 그리고 더 무겁다.

**다음으로 살펴볼 프로토콜은 DNS다.**

## DNS

우리가 웹 사이트에 접속할 때는 www.yahoo.com 과 같은 방식으로 접속한다.
그런데 실제로 client process에서 server process를 contact할 때는 hostname이
아닌 hostname에 해당하는 32bit IP address를 사용한다.

그래서 결국은 모든 network application의 경우 hostname을 IP주소로 mapping해주는
작업이 필요하다.

이런 mapping service를 제공해주는 application이 Domain Name System이다.

DNS는 distributed database를 통해 mapping 정보를 저장해두고 있는다.

그런데 이런 mapping service는 모든 network application이 필요로 하는 Service이므로
네트워크에서 제공하는 것이 합리적이다. 하지만 DNS는 application layer protocol로
구현되었다. 그 이유는 "네트워크 내에서는 가장 간단한 역할 즉 패킷을 전달하는 역할만
하고 다른 일은 가급적이면 하지 말자. 모든 복잡성은 edge로 몰아내자"는 인터넷의 철학
때문이다.

### DNS Services

가장 기본적인 DNS의 기능은 'hostname to IP addres translation'이다. 어떠한
network application이 실행되려면 user가 자신이 원하는 server 주소를 typing했다면
그 server의 hostname에 해당하는 IP 주소를 알아내야 진행이 가능하다.

- host aliasing
  hostname 입력 시 domain 이름을 바꾸는 경험이 있을 것이다. 그것이 실제 hostname이고,
  외부에는 의미 있는 이름을 제공하는 것이다.

- mail server
  domain을 담당하는 mail server를 알려주는 service

- load distribution
  request가 많을 때 replicate를 두는 경우 있는데 Service 자체를 alias를 하나 두고
  web server1, web server2 와 같은 replicate 각각에 분산해서 요청을 전달해 load를
  분산시킨다.

### Why not centralized DNS?

그렇다면 왜 Centralized DNS를 사용할 수 없을까?

DNS는 network application에 있어 굉장히 중요한 서비스다. DNS가 제대로 동작하지 않으면
실행이 불가능하다. 그래서 하나의 중앙 DNS가 잘못되는 치명적인 문제가 생긴다. 또,
Centralized DNS를 두면 traffic이 집중되어 처리가 곤란해진다. 그리고 DNS translation 요청을
처리하는데 centralized가 하나 있으면 distant로 인해 응답시간이 길어지는 문제도 발생한다.
마지막으로 DB가 커지는데 이를 관리하는 것이 어려워진다는 문제점도 있다.

### DNS hierarchy

DNS는 distributed이기도 하지만 hierarchical DB이기도 하다.

```
Root DNS server : Top level DNS server information
- Top level DNS server : 기능별 도메인을 담당 ex) .com, .edu
  - Authorititive DNS server : hostname to IP address mapping information
```

이제 계층을 하나씩 살펴보도록 하자.

#### Root DNS Servers

.edu, .com 등을 담당하는 TLB 서버 정보를 유지하고 있다.

#### Top-level domain Servers(TLD)

ex) .com, .edu

와 같은 기능별 도메인 정보를 담당한다. 그리고 이를 관리하는 기관들이 있다.

ex) Network Solutions -> .com 도메인 관리
ex2) Educause -> .edu 정보 관리
ex3) 한국인터넷정보센터 -> .kr 정보 관리

각 기능별로 담당하는 authoritative DNS servers의 정보를 유지하고 있다.

#### authoritative DNS Servers

각 기관이 보유하는 DNS Server다. 예를 들어 대학교의 정보전산원 등 말이다.
실제 hostname to IP address 정보를 유지하고 있다.

#### Local DNS Server

위 계층에 속하지 않는 DNS Server도 있다. Local DNS Server라고 한다. 또
Default name server라고도 하고, proxy name server라고도 한다.

이 서버를 설치하는 주체는 residential ISP, 회사, 학교 등이 있다.
웹 캐시를 두는 주체와 유사하다.

일단 DNS query는 모두 local DNS server로 간다. 있으면 응답, 없으면 root DNS Server로
가서 정보를 요청한다. (web cache처럼 말이다.)

정리하면 root DNS Server는 TLD 서버의 정보를 유지하고 있고, TLD 서버는 authoritative
DNS 서버의 정보를 보유하고 있다. 결국 authoritative DNS 정보에서 hostname to IP address
mapping 정보를 얻을 수 있다.

**이렇게 DNS Server에서 mapping 정보를 획득하는 방법을 조금 더 살펴보자. 방식은
2가지다.**

### DNS query

- iterated query
- recurisve query

#### iterated query

![iterated_query](/images/computer_networking/iterated_query.png)

첫 번째 방법은 iterated query 방식이다. mapping 정보 자체를 알려주는 것이 아니라
어떤 Server에 contact해야 하는지를 알려준다.

Root DNS Server는 TLD DNS Server의 정보를 알려주고, TLD DNS Server는
authoritative DNS Server의 정보를 제공하고, authoritative DNS Server를 통해 결국은
hostname to IP address mapping 정보를 획득하는 방식이다.

#### recursive query

![recursive_query](/images/computer_networking/recursive_query.png)

해당 서버가 어떻게든 필요한 mapping 정보를 구해서 query를 보낸 호스트에 알려주는 방식이다.
iterated query와 비교했을 때 recursive query 방식은 root와 root TLD에 load가 커진다.

root와 TLD 중 어디가 load가 더 클까? 바로 root다. TLD는 해당 .edu와 같은 기능별 도메인에
대한 처리만 하지만 root는 모든 도메인에 대한 요청을 받는다.

그래서 이 경우 Caching을 사용하면 응답시간을 줄이고 traffic도 줄일 수 있다.

local DNS Server는 TLD DNS Server를 유지한다. 이렇게 되면 root에 query를 보내지 않아도
된다. root가 부하가 집중되는 곳이니 이렇게 처리해주는 것이다.

그래서 결국 local DNS Server는 Caching하고 있는 hostname이면 바로 요청에 대해 응답하고,
모르는 호스트의 경우 TLD DNS Server 정보를 가지고 있으니 바로 query를 보내서 처리한다.

그런데 앞서 살펴봤듯이 Cache의 문제점은 out-of-date 정보를 제공할 수 있다는 점이다.
그래서 mapping 정보에는 TTL(Time to leave) 정보가 들어간다.

TTL이 만료되면 정보를 버리고 그 사이에 갱신되면 out-of-date 정보를 제공하게 되는 방식이다.
이러한 문제 때문에 IETF 기관에서는 hostname 정보 변경 시 update/notify하는
mechanism이 제안되었다.

### DNS message

지금 살펴볼 것은 DNS message다. DNS에서 주고 받는 message의 형식이다.
query와 reply 둘다 동일한 message format을 가지고 있다.

msg header에서는 identification과 flags가 들어간다.

- identification : 보낼 때 identification을 copy해서 똑같은 identification으로
  reply한다. 이를 통해 짝을 지을 수 있다.(16bit)
- flags(2bytes[16bits])
  - query or reply
  - recursion desired
  - recursion available
  - reply is authoritative

그 밑으로는 가변 길이의 field들이 붙게 된다. (가변 길이면 길이를 알려줘야
parsing이 가능하다.)

- questions 필드 : name, type
  - query가 실린다. 여러 개가 실릴 수 있다.
- answers 필드
  - 응답을 실어나른다.
- authority 필드 : records of authorization servers
- additional info 필드 : additional "helpful" info that may be used

### DNS Resource Record

DNS의 정보는 분산 DB에 저장되는데 4개의 정보(field)를 가지고 있다.

Resource Record format : (name[ex:hostname], value, type, ttl[time to leave])

여기서 name과 value는 가장 기본적인 정보다. type 값은 다음과 같이 정해질 수 있다.

- hostname - IP address pair : 'A'
- alias - canonical name : 'CNAME'
  ex) www.ibm.com - servereast.backup2.ibm.com
- domainname - mailservername : 'MX'
- domainname - authoritative name server : 'NS'

### DNS Scenario

예를 들어 새로운 창업기업 Network utopia 라는 기업이 생겨났다고 생각해보자.
그러면 가장 먼저 authoritative server를 구비하고 DB에 hostname to IP address 정보를
유지한다.

- type A record : "www.networkuptopia.com"
- type MX and A record for mail server at networkutopia.com

그리고 .com 도메인을 관리하는 Network Solutions에 DNS 등록을 한다.
이 때 authoritative name server의 이름과 IP 주소를 제공해야 한다.

그러면 network solutions 에서 .com TLD Server에 두 개의 resource record를
삽입한다.

- (networkutopia.com, dns1.networkutopia.com, NS)
- (dns1.networkutopia.com, 212.212.212.1, A)

이렇게 되면 어디에서도 접근이 가능하다.

**이 때까지 application 프로토콜에 대해 이야기했다. 그런데 이 application 프로토콜의
역할은 application program을 지원하는 것이다. 이 network application은 socket을
사용한다. socket은 application program과 transport layer 계층 사이의 interface다.
지금 예제 프로그램을 통해 소켓 프로그래밍에 대해 살펴보자.**

## Socket Programming

살펴볼 예제 프로그램은 Client가 키보드 입력을 받아서 Server로 전달하면 Server는
입력을 대문자로 변환해서 다시 Client로 전달해 Client가 마지막으로 화면에 출력하는
프로그램이다.

transport 서비스로 TCP를 사용할 수도 있고, UDP를 사용할 수도 있다.

먼저 UDP를 사용한 경우를 살펴보도록 하자.

### Socket Programming with UDP

- UDP : no 'Connection' between client & Server
- Sender : IP destination address + port# 를 패킷에 붙여야 한다.
  (Server는 port 번호가 알려져 있지만 Client는 Port 번호가 알려져 있지 않다.
  그럼 어떻게 알 수 있을까? 요청받은 Segment에서 추출해낸다.)
- Receiver : extract 'Client's ID address and Port #'

### Client/Server Socket Interaction : UDP

![UDP_interaction](/images/computer_networking/UDP_interaction.png)

전체적인 흐름은 위와 같다. Python 코드를 통해 진행을 살펴보자.

```python
# UDP Client
from socket import * # include Python's socket library

serverName = 'hostname' # ex) 'mm.ewha.ac.kr'
serverPort = 12000
clientSocket = socket(socket.AF_INET, socket.SOCK_DGRAM)
# AF_INET -> IPv4 / SOCK_DGRAM -> UDP
# 명시해주는 이유는 운영체제가 주소 그리고 포트 번호를 할당하는데
# IPv4 주소라는 것을 알려주는 것이다. 또 transport 서비스를 명시해주는 것이다.
message = raw_input('Input lowercase sentence:')
clientSocket.sendto(message(serverName, serverPort))
# Attach server name, port to message; send into socket
modifiedMessage, serverAddress = clientSocket.recvfrom(2048)
# recvfrom은 2가지 값을 return한다. 2048은 받는 메세지의 최대 길이를 의미한다.
print modifiedMessage
clientSocket.close()

# UDP Server
from socket import *
serverPort = 12000
serverSocket = socket(AF_INET, SOCK_DGRAM)
serverSocket.bind(('', serverPort))
# client에서는 없던 명령이다. Client는 운영체제가 바인딩하고 Server는
# 명시적으로 binding을 해주어야 한다. Server도 운영체제가 default로
# 지정하지만 override하는 것이다.
print "The server is ready to receive"
while 1:
    message, clientAddress = serverSocket.recvfrom(2048)
    # 위 명령은 extract하는 작업이다.
    modifiedMessage = message.upper()
    serverSocket.sendto(modifiedMessage, clientAddress)
```

다음은 TCP 소켓 프로그래밍이다.

### Socket Programming with TCP

- server process must first be running
- server must have created socket(door) that welcomes client's contact
  (Client의 contact를 받아들이기만 하는 용도 그래서 welcome socket이라고 한다.)

그 다음은 Client가 Contact한다.

- Creating TCP Socket specifying IP address, port number of server
  process for handshaking
  (이미 Socket이 있지만 해당 Client를 위한 Socket를 다시 생성한다.)
- when client creates socket : client TCP estabilishes connection to server TCP
  (socket을 계속 생성하기 때문이다.)
- when contacted by client, server TCP creates new socket for server process to
  communicate with that particular client
  - allows server to talk with multiple clients
    (왜 소켓을 계속 생성해주어야 할까? byte stream을 교환하기 때문이다.)
  - source port numbers used to distinguish clients

Client Socket으로부터 내려보내지는 것은 전부 목적지를 명시하지 않아도 새로 생성한
Server가 새로 생성한 Socket으로 간다. 그래서 TCP로 데이터를 주고 받을 때는 목적지를
별도로 명시하지 않는다. Connection을 Setup한 다음에는 목적지를 명시하지 않는다.
굳이 명시하지 않아도 TCP 계층에서는 이미 알고 있기 때문이다. (차이점)

그래서 Client별로 Socket를 두는 것이다. 반면, TCP는 한 번 Connection을 맺으면 임의의 길이의
byte stream을 주고 받게 된다. 그런데 그동안 다른 Client의 Contact를 못 받으면 안 된다.
UDP의 경우 각 메세지가 독립적이다. 그렇기 때문에 UDP에서는 메세지 하나를 받으면 그에 대한 답을
보내주고 끝이다.

### Client/Server Socket interaction : TCP

![TCP_interaction](/images/computer_networking/TCP_interaction.png)

TCP 소켓 프로그래밍의 전체적인 흐름은 다음과 같다. 코드 진행은 다음과 같다.

```python
# TCP Client
from socket import *

serverName = 'servername'
serverPort = 12000
clientSocket = socket(socket.AF_INET, socket.SOCK_STREAM)
# AF_INET -> IPv4 / SOCK_STREAM -> TCP
clientSocket.connect((serverName, serverPort))
sentence = raw_input('Input lowercase sentence:')
clientSocket.send(sentence)
# 위 명령을 보면 목적지를 명시하지 않는다.
modifiedSentence = clientSocket.recv(1024)
print 'From Server: ', modifiedSetence
clientSocket.close()

# TCP Server
from socket import *
serverPort = 12000
serverSocket = socket(AF_INET, SOCK_STREAM)
serverSocket.bind(('', serverPort))
serverSocket.listen(1) # request가 올 때까지 대기
print "The server is ready to receive"
while 1:
    clientSocket, addr = serverSocket.accept()
    # 위 명령은 connection 요청이 들어오면 실행된다.
    sentence = connectionSocket.recv(1024)
    capitalizedSentence = sentence.upper()
    connectionSocket.send(capitalizedSentence)
    # 위 명령 역시 목적지를 명시하지 않는다.
    connectionSocket.close()
```
