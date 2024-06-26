# 3.1 Introduction and Transport-Layer Services

![](https://velog.velcdn.com/images/calzone0404/post/47d0ae3b-42f3-4030-81fb-2d8295e06c36/image.png)

>전송계층 프로토콜은 각기다른 호스트에서 실행되고 있는 프로세스들간의 논리적인 커뮤니케이션을 제공한다. 

**논리적인 커뮤니케이션**을 통해서 각자 호스트의 어플리케이션의 관점에 있어서 마치 호스트가 지구 정반대편에 서로 존재하더라도, 직접적으로 연결되어있는 것 같은 착각을 불러일으킨다.

전송계층 프로토콜은 **네트워크 라우터**에서 구현되는 것이 아닌, **말단장치**에서 구현된다.

>송신측에서의 전송계층은 응용계층으로부터 전달받은 메세지를 변환하여 **전송계층의 패킷**으로 변환한다. 이 전송계층의 패킷을 **SEGMENT**라고 부른다.

어플리케이션 계층의 메세지를 작은 단위로 쪼개서, 전송계층의 헤더에 각각의 청크를 넣어서 전송계층의 세그먼트를 생성한다. 그리고 네트워크 계층에서 Packet을 Datagram 단위로 캡슐화 작업 후, 목적지로 보내버린다. 수신측에서는 이와 정반대의 과정이 일어난다.

## Relationship Between Transport and Network Layers

> 전송계층은 각기다른 호스트에서 실행되고 있는 **Process**간의 논리적인 커뮤니케이션을 지원하는 계층이다.
- 즉, 전송계층 프로토콜은 종단 시스템에서 구현되며, 응용계층과 네트워크계층 사이에서 메세지를 운반하는 역할을 수행한다.

> 네트워크 계층은 호스트끼리의 논리적인 커뮤니케이션을 지원하는 계층이다

전송 프로토콜이 제공할 수 있는 서비스는 **네트워크 계층 프로토콜의 서비스 모델에 의해 제약을 받는다**
- 만약 네트워크 계층이 안전한 전송을 위한 delay 또는 충분한 대역폭을 제공하지 못한다면 전송계층 프로토콜은 응용계층에서 다른 호스트의 프로세스로 전달하려는 메세지에 대해  delay 또는 충분한 대역폭을 보장할 수 없다.

하지만, 네트워크 프로토콜이 네트워크 계층에서 이러한 delay나 충분한 대역폭을 보장하지 않아도 전송 프로토콜은 특정 서비스를 제공할수는 있다.
- 예를 들어서, 네트워크 계층이 비신뢰적인 상태라면 전송계층이 신뢰성있는 데이터 전송을 보장할 수 있다는 의미이다.
- 즉, 네트워크 프로토콜이 패킷을 손실, 왜곡 또는 패킷을 복제하는 경우에도 전송 프로토콜은 암호화를 사용하여 패킷의 보안을 지킬 수 있다.

## Overview of the Transport Layer in the Internet

인터넷은 애플리케이션 계층에 **UDP(User Datagram Protocol, 사용자 데이터그램 프로토콜)**와 **TCP(Transmission Control Protocol, 전송 제어 프로토콜)** 두 가지 서로 다른 전송 계층 프로토콜을 제공합니다. 

UDP는 **신뢰할 수 없는** 비연결 서비스를, TCP는 **신뢰할 수 있는** 연결 지향 서비스를 제공한다. 네트워크 애플리케이션을 설계할 때, 개발자는 이 두 전송 프로토콜 중 하나를 선택해야 한다.

전송 계층 패킷을 간략히 'Segment'라고 하며, TCP의 경우는 'Segment', UDP의 경우는 일반적으로 'Datagram'으로 불린다.

> 하지만,네트워크 계층 패킷에도 같은 용어를 사용하여 혼동을 방지하기 위해 **TCP와 UDP 패킷 모두를 'SEGMENT'**라고 칭하고, 'Datagram' 용어는 네트워크 계층 패킷에만 사용한다.

인터넷의 네트워크 계층은 **IP(Internet Protocol, 인터넷 프로토콜)**라는 프로토콜을 사용하여 호스트 간의 논리적 통신을 제공한다.

IP는 ***'최선 노력 배달 서비스'***를 제공하는데, 이는 IP가 통신 호스트 간에 세그먼트를 배달하기 위해 최선을 다하지만 배달, 순서대로 배달, 데이터 무결성을 보장하지 않기 떄문에 **IP는 신뢰할 수 없는 서비스로 간주**된다. 모든 호스트는 최소한 하나의 네트워크 계층 주소인 IP 주소를 가지고 있다.

UDP와 TCP의 가장 기본적인 동작 규칙은 IP의 두 end-system 간의 배달 서비스를 **두 process 간의 배달 서비스로 확장**하는 것 (전송계층이 하는 작업)이다. 이를 위해 전송 계층의 **멀티플렉싱과 디멀티플렉싱**을 수행한다.

UDP와 TCP는 또한 세그먼트 header에 "오류 검출 필드"를 포함함으로써 무결성 검사를 제공한다. UDP는 프로세스 간 데이터 배달과 오류 검사만을 제공하며, 신뢰할 수 없는 서비스인 반면, TCP는 신뢰할 수 있는 데이터 전송, 흐름 제어, 순서 번호, 승인, 타이머 등을 사용하여 데이터가 올바르고 순서대로 전달되도록 보장한다. 

TCP는 전체 인터넷을 위한 서비스로서, 연결의 공정한 대역폭 공유를 위해 각 연결의 전송 속도를 조절함으로써 네트워크의 혼잡을 관리하는 **혼잡 제어 기능**을 제공한다.

# 3.2 Multiplexing and Demultiplexing

이 섹션에서는 전송계층의 multiplexing과 demultiplexing에 대해서 소개한다. 

목적지 호스트에서, 전송계층은 네트워크 계층으로부터 세그먼트를 전달받는다. 전송계층은 전달받은 세그먼트들을 현재 실행중인 적절한 응용 프로세스들에게 전달해야만 한다.

예를 들어, 당신이 FTP 세션과 두개의 Telnet 세션을 실행중인 동시에, Web 페이지를 다운로드하는 중이라고 가정해보자. 만약 전송계층에서 하나의 세그먼트를 받았을 때, 이 세그먼트를 HTTP 프로토콜로 보내야할지, FTP로 보내야할지, Telent으로 보내야할지 결정해야 한다.

프로세스는 하나 또는 그 이상의 **소켓**들을 가질 수 있다. 이 **소켓**은 데이터가 네트워크에서 프로세스로, 프로세스에서 네트워크로 전달되는 **문**이 된다. 
![](https://velog.velcdn.com/images/calzone0404/post/7295bf2f-9732-41c7-93d1-2214af266951/image.png)

위 사진에서, 전달받는 측의 전송계층은 실제로 프로세스에게 직접적으로 전달하지 않음을 볼 수 있다. 대신, 중간에 존재하는 소켓을 통해서 전달한다. 따라서 수신측 호스트에는 하나 또는 그 이상의 소켓이 존재한다. 이 소켓은 UDP인지 TCP인지로만 식별된다.

각 전송계층에 전달된 세그먼트는 목적에따른 필드들의 집합을 가지고 있다. 전송계층은 이 필드를 확인하여, 이 세그먼트의 목적지 소켓을 식별한 뒤, 해당 소켓으로 전달한다.

> 위 과정을 **Demultiplexing**이라고 한다.

송신측에서는 전송계층에서 다양한 소켓들로부터 데이터 청크를 모으고, 세그먼트를 생성하기 위해 헤더를 달고, 각 데이터 청크를 encapsulating 한뒤, 네트워크 계층으로 전달하게 된다.

> 위 과정을 **multiplexing**이라고 한다.

![](https://velog.velcdn.com/images/calzone0404/post/74ea9bac-8435-4efa-bb6c-02b76505e95e/image.png)

## Connectionless Multiplexing and Demultiplexing

파이썬 프로그램에서는 다음과 같이 **UDP socket**을 생성할 수 있다.

```python
clientSocket = socket(AF_INET, SOCK_DGRAM)
```
이 방법으로 UDP socket을 생성하게 되면, 전송계층은 자동으로 소켓에 **port number**을 할당하게 된다. 특히, 전송계층은 포트번호를 할당할 때, 다른 UDP가 주로 사용하지 않는 범위인 1024 ~ 65535 사이의 번호를 할당해준다. 물론, 우리가 직접 포트번호를 **bind()**함수를 통해 지정해줄수 있다.

```python
clientSocket = bind(('', 19157))
```

그런데, 만약 개발자가 "Well known protocol"코드를 작성하였다면, "Well know protocol"에 해당하는 "Well known port number"을 할당해주어야만 한다.

일반적으로, 클라이언트측의 어플리케이션은 전송계층이 포트번호를 자동으로 할당하도록 두지만, 서버측의 어플리케이션은 구체적인 포트번호를 지정해야한다.

## 구체적인 UDP 통신 과정

UDP port 19157을 가지는 Host A가 있다고 가정해보자. A는 UDP prot 46428을 사용하고 있는 Host B에게 프로그램 데이터를 송신하고 싶다.

이때, 송신자 A의 전송계층에서 세그먼트를 생성하는데, 세그먼트 안에는 송신자 A의 포트 번호(19157), 수신지 B의 포트번호(46428), 보내려는 어플리케이션 데이터, 또 추가적인 헤더 정보가 담긴다.

이 세그먼트 뿐만이 아니라 여러 세그먼트를 묶어서 (Multiplex) 네트워크 계층에게 넘기면 네트워크 계층에서 IP datagram에 캡슐화 후, 수신지 호스트에게 best-effort로 전달을 시도한다.

이제 B가 이 Datagram을 받게 되면 전송계층에서 목적지 port 번호를 확인하여 해당 포트번호의 소켓(42428)으로 넘긴다 (Demultiplex). 

> 중요한점 : UDP Socket은 목적지 IP주소와 목적지 port number을 통해 식별한다.

그럼 송신자, Source port number가 필요한 이유는 무엇일까? 바로, **Return address**로 작동하기 때문에 필요하다. B가 만약에 다시 A로 데이터를 전송하고 싶다면 A로부터 받은 Source Port number을 통해 다시 전송하면 매우 효율적으로 통신이 가능해진다.

<hr>

## 구체적인 TCP 통신 과정

TCP Demultiplexing을 이해하기 이전에, TCP Socket과 TCP 연결 수립 과정을 알아야만 한다. TCP는 Source ip address와 destination ip, source port, destination port number가 필요하다.

<hr>

### TCP 연결 수립 과정

1. TCP 서버는, 포트번호 12000에서 TCP 클라이언트로부터 연결 요청을 기다리는 "Welcoming socket"을 가진다.

2. TCP 클라이언트에서 소켓을 생성하고, 아래 명령을 통해 연결을 수립하는 요청을 보낸다.

```python
clientSocket = socket(AF_INET, SOCK_STREAM)
clientSocket.connect((serverName, 12000))
```
이때, TCP 헤더에 목적지 포트번호 12000과 특별한 연결 설정 비트를 담고, 클라이언트에서 선택한 출발지 포트번호에서 TCP segment를 전송하게 된다.

3. 서버에서 목적지 포트번호 12000인 세그먼트를 수신하게 되면, 이 세그먼트를 포트 번호 12000을 사용하고 있는 서버 프로세스로 전달한다.

<hr>

## Web Servers and TCP
![](https://velog.velcdn.com/images/calzone0404/post/3b9e03f5-09b7-4fcd-96a5-4838872872b6/image.png)

위 그림의 상황을 한번 살펴보자. 호스트 C가 두 HTTP 연결을 TCP로 B에게 전달하고 있고, 호스트 A가 하나의 HTTP 연결을 TCP로 B에게 전달하고 있다. 이때, 호스트 A의 source port 번호와 C의 source port 번호가 같은 세그먼트가 존재한다.

> 그러나, B는 이 두 세그먼트를 정확히 구별이 가능하다. 그 이유는 두 클라이언트 A와 C가 서로 다른 IP주소를 사용하고 있기 때문이다.

아파치 웹 서버와 같이 80번 포트에서 작동하는 웹 서버를 운영하는 호스트의 예를 들면, 클라이언트(예: 브라우저)가 서버로 세그먼트를 보낼 때 모든 세그먼트의 목적지 포트는 80이다.

이는 초기 연결 설정 세그먼트와 HTTP 요청 메시지를 담은 세그먼트 모두에 해당하며, 서버는 소스 IP 주소와 소스 포트 번호를 사용하여 위 사진처럼 다른 클라이언트의 세그먼트를 구분하게 된다.

클라이언트와 서버가 persistent HTTP를 사용하는 경우, 지속적인 연결 동안 클라이언트와 서버는 동일한 서버 소켓을 통해 HTTP 메시지를 교환하게 된다. 

반면에, non-persistent HTTP를 사용하는 경우, 각 요청/응답마다 **새로운 TCP 연결이 생성되고 종료**되며, 따라서 각 **요청/응답마다 새로운 소켓이 생성되고 나중에 닫힌다.** 

이러한 빈번한 소켓 생성 및 종료는 바쁜 웹 서버의 성능에 심각한 영향을 미칠 수 있다(비록 운영 체제의 여러 가지 기술을 사용하여 이 문제를 완화할 수 있음).

# 3.3 Connectionless Transport : UDP

### UDP 동작 개요

> UDP(User Datagram Protocol)는 비연결형 트랜스포트 계층 프로토콜로, TCP와 달리 연결 설정 과정이 없다.

- UDP는 간단한 **Multi/Demultiplexing 기능과 오류 검사 기능**만 제공하며, 송신자와 수신자 간의 데이터 전송에 있어 최소한의 작업만 수행하는 전송계층 프로토콜 이다.

### UDP 동작 방식

1. UDP는 송신 측에서 애플리케이션 프로세스로부터 메시지를 받아 이를 **네트워크 계층에 전달**한다.

2. UDP는 데이터를 전송하기 위해 송신 측에서 출발지 포트 번호와 목적지 포트 번호를 설정하고, 이를 UDP 헤더에 포함시켜 세그먼트를 생성한 후 네트워크 계층으로 전달한다.

3. 수신 측에서 네트워크 계층으로부터 받은 메시지를 애플리케이션 프로세스로 전달한다.

### UDP 특징

- `비연결형`: 송신자와 수신자 간에 데이터 전송을 위한 사전 연결 설정 과정(*Handshake*)가 없다. 즉, 연결 설정에 따른 지연이 발생하지 않는다.

- `오류 검사 기능`: UDP는 오류 검출을 위한 체크섬 필드를 제공하여, 데이터 전송 중 발생할 수 있는 비트 오류를 검사한다. 수신 측에서는 체크섬을 통해 세그먼트에 오류가 있는지를 확인한다. 오류 검사 기능에 대한 자세한 설명은, 3.3.2절에서 설명한다!

- `간단한 헤더 구조`: UDP 헤더는 **8바이트**로 매우 간단하다. 각 2바이트로 이루어진 `출발지 포트 번호`, `목적지 포트 번호`, `길이`, `체크섬`의 4개 필드로 구성되어 있다.

### UDP 사용 사례
![](https://velog.velcdn.com/images/calzone0404/post/4a55e633-9dc7-4bfe-a723-9310c392d765/image.png)

UDP는 실시간 애플리케이션, 멀티미디어 스트리밍, DNS, 네트워크 관리 프로토콜 등에서 주로 사용된다. 어플리케이션 계층에서 UDP를 사용하는 이유는 UDP의 간단한 특징 때문이다.

1. `데이터 전송의 정교한 제어`: 애플리케이션이 데이터를 보낼 때, TCP의 혼잡 제어에 방해받지 않고 원하는 대로 전송할 수 있다.

2. `연결 설정 지연 없음`: TCP의 연결 설정 과정 없이 즉시 데이터를 전송할 수 있다.

3. `연결 상태 유지 없음`: UDP는 연결 상태를 유지하지 않기 때문에 몇몇 프로토콜에서는 더 많은 클라이언트를 동시에 처리할 수 있다.

4. `헤더 크기`: UDP 헤더는 8바이트인데, TCP는 20바이트를 사용한다.

### UDP와 TCP의 비교

> `TCP`는 **신뢰적인 데이터 전송**을 보장하고, 데이터의 **순서 보장**, **흐름 제어** 및 **혼잡 제어**를 제공한다.

> 반면`UDP`는 **비신뢰적인 데이터 전송**을 제공하며, **최소한의 오류 검사 기능**만 제공한다.

따라서, UDP는 간단하고 빠르며, 특정 상황에서 TCP보다 더 적합할 수 있다. 특히, 실시간 애플리케이션이나 멀티미디어 스트리밍과 같은 어플리케이션에서 유리하다. (Chrome의 QUIC 프로토콜)

## 3.3.1 UDP 세그먼트 구조

### UDP 세그먼트의 구성
![](https://velog.velcdn.com/images/calzone0404/post/bfd6046d-a6f9-4b3d-b5c1-89d0752772ae/image.png)

UDP(User Datagram Protocol) 세그먼트는 위 그림처럼 간단한 구조를 가지고 있으며, 다음과 같은 필드로 구성된다.

1. `출발지 포트 번호` (Source Port Number): 16비트 길이로, 송신자의 포트를 식별하는데 필요하다. 출발지 포트 번호는 수신자가 송신자에게 응답을 보낼 때 사용한다.

2. `목적지 포트 번호` (Destination Port Number): 16비트 길이로, 수신자의 포트를 식별하는데 필요하다. 이 필드는 **어떤 수신지의 어플리케이션 소켓으로 보낼 지 결정**할 때 필요하다.

3. `길이` (Length): 16비트 길이로, UDP 헤더와 데이터의 **총 길이**를 바이트 단위로 기록한다. 최소 값은 8바이트 (헤더의 길이)이다.

4. `체크섬` (Checksum): 16비트 길이로, **오류 검출**을 위해 사용된다. 3.3.2절에서 자세한 설명을 하도록 하겠다.

5. `데이터 필드` : 어플리케이션 데이터가 위치하는 곳이다. 예를 들면, DNS의 경우 데이터 필드에 질의 메세지 또는 응답 메세지가 들어있다. 스트리밍 비디오 어플리케이션 같은 경우에는 영상 정보가 데이터 필드에 들어간다.

## 3.3.2 UDP 체크섬의 역할

> 체크섬은 UDP 세그먼트가 전송 중 손상되었는지 확인하기 위해 사용된다. 

송신 측에서 UDP는 세그먼트 내의 모든 **16비트 워드(word)를 더한 후**, 다시 **1의 보수**를 취하여 체크섬 필드에 삽입한다.

> 수신 측에서는 이 체크섬을 검증하여 오류를 감지할 수 있다.

- 오류가 없는 경우, 수신 측에서 계산한 값이 0xFFFF**(모든 숫자가 1)**이다. 

- 그렇지 않으면, **즉, 하나이상 0이 발생한다면** 세그먼트에 **오류가 발생**한 것으로 판단할 수 있다.

### 체크섬 계산 예시

```text
16비트 워드 세 개: 
	0110011001100000, 
    0101010101010101, 
    1000111100001100
    
첫 두 워드의 합: 
	0110011001100000 + 
    0101010101010101 
    = 1011101110110101
    
이 합에 세 번째 워드를 더하면: 
	1011101110110101 + 
    1000111100001100 
    = 0100101011000010 (오버플로 발생) -> 자리올림
    
세 워드의 합 연산 결과에 1의 보수를 취하면:
0100101011000010의 1의 보수는 1011010100111101
이 값이 바로 체크섬이다.
```

> 이제, 수신측에서도 3개의 워드와 체크섬 값을 받게 되는데,
세 워드의 합 + 체크섬 비트를 연산해서 0xFFFF가 되면 `오류없음`으로 판단하게 되고, 하나라도 0이 존재하면 `오류발생`으로 판단한다.

# 3.4 Principles of Reliable Data Transfer

![](https://velog.velcdn.com/images/calzone0404/post/7169bdd1-7e10-4bf8-b0c9-6bfbc74d5fc3/image.png)

> 이 챕터에서는 TCP, 즉 신뢰적인 데이터 전송이 어떻게 동작하는지에 대해 서술하는 챕터이다.

#### 용어 설명
`rdt` : Reliable data transfer
`udt` : Unreliable data transfer

먼저 동작 과정을 설명하기 전에, 몇가지 가정을 하고 시작한다.

1. `신뢰적인 채널`에서는 전송된 데이터가 손상되거나, 손실되지 않는다고 가정한다.
   - 이러한 서비스 추상화를 구현하는 것이 **신뢰적인 데이터 전송 프로토콜**의 의무이다.
하지만, 이 작업은 신뢰적인 데이터 전송 프로토콜의 하위 계층이 **신뢰적이지 않을 가능성이 존재**하여 어려워진다.  

2. 하위 채널에서 비트가 손상되거나 전체 패킷을 손실하는 경우, 어떠한 프로토콜 매커니즘이 필요할지 생각해야한다.
   - 이때, 보내진 패킷은 일부 손실될 수 있지만, **보내진 순서대로 잘 전달될 것이라고** 가정한다.

3. 이 챕터에서는 `단방향 데이터 전송`의 경우인 송신 측으로부터 수신 측까지의 데이터 전송만을 고려한다.

## 3.4.1 신뢰적인 데이터 전송 프로토콜 구축 방법

### RDT 1.0
![](https://velog.velcdn.com/images/calzone0404/post/ac7bd59e-cb43-4dcd-a75c-0f005bfafee2/image.png)

> RDT 1.0은 완벽하게 신뢰적인 채널상에서의 신뢰적인 데이터 전송에 대해서만 따진다.

#### 송신측
1. 상위 계층 (응용계층)이 `rdt_send()` 함수를 호출하여 데이터를 전송 계층으로 전달한다.
2. 전송 계층에서는 데이터를 `packet`으로 만든다
3. 패킷을 채널로 송신(`udt_send()`)한다.

#### 수신측
1. `rdt_rcv()` 이벤트에 의해 하위 채널 (네트워크 계층)으로부터 패킷을 수신한다.
2. 전달받은 패킷를 `extract(packet, data)`를 통해 데이터를 추출한다.
3. 데이터를 `deliver(data)`를 통해 응용 계층으로 전달한다.

### RDT 2.0

> RDT 2.0은 비트 오류가 있는 채널상에서 신뢰적인 데이터 전송에 대해서만 따진다. 즉, 전송되는 비트가 1에서 0으로 바뀌거나 0에서 1로 바뀌는 오류에 대해서만 생각하자.

RDT 2.0에서는 `Automatic Repeat reQuest, ARQ (자동 재전송 요구)`라는 프로토콜을 사용한다.

#### ARQ 기능
- `오류 검출` : 비트 오류가 발생했을 때 수신자가 검출할 수 있는 기능
- `수신자 피드백` : 송신자가 수신자의 상태를 알아내는 방법
수신자가 송신자에게 ACK 또는 NAK 피드백을 제공해서 메세지가 잘 도착했는지 송신자에게 피드백을 준다.
이때 ACK는 긍정적 메세지로, 메세지를 잘 받았다는 의미이며, `1`로 표현한다.
NAK은 부정적 메세지로, 메세지를 전송받지 못했다는 의미이며, `0`으로 표현한다.

![](https://velog.velcdn.com/images/calzone0404/post/0b16b7a6-d300-4ff9-8ec5-ada787c2017c/image.png)

#### 송신측
1. 응용 프로그램에서 `rdt_send(data)`를 호출해서 데이터를 전송계층으로 전달한다.
2. 보낼패킷 `sndpkt`변수에 `make_pkt(data, checksum)` 보낼 데이터와, 체크섬(3.3.2절에서 설명한 UDP 세그먼트에서 사용하는 오류검출 기술임)을 함께 묶어서 패킷을 만듬
3. 패킷을 udt_send()로 전송함
4. 전송한 후에, 송신측은 `ACK 또는 NAK 응답을 기다림`
5. 만약, 수신지에서 rdt_rcv(rcvpkt)으로 피드백이 왔을 때, 만약 이 패킷안에 들어있는 피드백 값이 `NAK`라면, 다시 보냈던 패킷을 `udt_send(sndpkt)`을 통해 보낸다.
6. 만약 피드백이 `ACK`라면, rdt_rcv(rcvpkt)를 수신지로 보내서 해당 피드백을 **확인했음**이라고 알려준다. 따라서 송신자는 방금 보낸 패킷이 **정확하게 수신되었음**을 확정하게 된다.

#### 수신측
1. 하위 계층에서 패킷을 전달받는다 (`rdt_rcv(rcvpkt)`). 그리고, 해당 패킷에 오류가 있는지 확인한다 (`corrupt(rcvpkt)`)
2. 만약 corrupt()가 **TRUE**이면, `NAK` 피드백을 송신측에 보내기 위해 패킷을 생성하여, 송신측으로 보낸다
3. 만약 corrupt()가 **FALSE**이면, `ACK` 피드백을 송신측에 보내기 위해 패킷을 생성(`make_pkt`)하여 보낸다.(`udt_send(sndpkt)`)
4. 수신한 패킷은 extract하여 응용계층으로 올려보낸다.

#### 문제점

RDT 2.0도 치명적인 문제가 있는데, ACK 또는 NAK패킷이 손상될 수 있다는 가능성이 있다는 점이다. 만약 피드백 패킷이 손상된다면, 송신자는 수신자가 제대로 받았는지 안받았는지 확인할 수 없다.

### RDT 2.1

> RDT 2.1은 데이터 패킷에 순서 번호를 추가하여 데이터 전송의 신뢰성을 더욱 높인 방법이다.

`Sequence number`
- 수신자는 순서 번호를 확인해서 수신한 패킷이 재전송인지 새로운 패킷인지 확인할 수 있다.

#### 송신측
![](https://velog.velcdn.com/images/calzone0404/post/813085f3-aaf7-4b89-ba6f-1f3e8137a549/image.png)
기존과 설명이 중복되므로 달라진 점만 설명하겠다.

- make_pkt()을 사용할 때, 0번 순서 번호를 붙여서 패킷을 생성한다.
- 이후, 0번 순서 번호에 대한 ACK 또는 NAK를 기다린다.
- 만약 응용 계층에서 또 다른 데이터를 전송하려고 하면, 1번 순서 번호를 붙여서 송신한다.
- 1번 순서번호를 붙인 패킷을 전송하면 1번 패킷에 대한 ACK 또는 NAK를 기다린다.

#### 수신측
![](https://velog.velcdn.com/images/calzone0404/post/a87c1692-2a16-44fe-a68f-21fcd5558b8b/image.png)

- 0번 패킷을 받았다면, 풀어서 응용계층으로 넘기고 1번 패킷 수신대기를 한다.
- 그런데 만약 또 다시 0번 패킷을 받았다면, **제대로된 피드백 전송이 이루어지 않았다는 의미이므로** 피드백 패킷을 다시 전송한다.
- 1번 패킷을 받았다면, 풀어서 응용계층으로 넘기고 다시 0번 패킷 수신 대기를 한다.
- 만약 또 다시 1번 패킷을 받았다면, 다시 피드백을 전송한다.

### RDT 2.2

> RDT 2.2는 피드백 패킷에 대해서도 순서 번호를 붙여서 통신하는 방식이다. 그림을 통해 이해하면 될 것 같다.

#### 송신측
![](https://velog.velcdn.com/images/calzone0404/post/beeb3c90-3b5a-4511-9e4b-81f6a8caf98a/image.png)

#### 수신측
![](https://velog.velcdn.com/images/calzone0404/post/718c7c97-aed1-4050-9317-045387e87497/image.png)

### RDT 3.0
> 비트가 손상되는것 이외에도, 패킷이 손실이 일어나는 경우에 대해서 고려한 방법이다.

RDT 2.2처럼 순서 번호를 추가하여 정확한 순서의 피드백 및 패킷 수신이 가능하도록 보장하였다.

또한, `타이머`기능이 추가되었는데, 송신자가 각 데이터 패킷을 전송할 때 마다 타이머를 시작해서, **일정 시간이 지나도 ACK를 받지 못한다면, 해당 패킷을 재전송** 하도록 설계되었다.

#### 작동 방식 요약

##### 송신자
![](https://velog.velcdn.com/images/calzone0404/post/0f44d102-7671-400b-bdc9-9908e9ca9156/image.png)

##### 수신자
> 출처 : https://ddongwon.tistory.com/80

![](https://velog.velcdn.com/images/calzone0404/post/83fcaa69-ea0e-43b6-8b91-8bd90060e7ff/image.png)


1. 송신자가 데이터 패킷을 전송한 후, 타이머를 시작한다. 타이머가 일정 시간이 지나면 인터럽트가 발생한다.

2. 송신자가 수신자로부터 알맞은 순서의 ACK를 수신하면 타이머를 멈추고, 다음 패킷을 전송한다. ACK에는 당연히 수신된 패킷의 순서 번호가 함께 포함되어 있기 때문에, 송신자는 해당 ACK가 어떤 패킷에 대한것인지 파악할 수 있다.

3. 만약 **타임아웃이 발생하였다면**, 송신자는 패킷이 손실되었다고 가정하여 다시 패킷을 전송해준다.

4. 수신자는 패킷을 수신할때마다 순서 번호를 확인해서 해당 패킷에 대한 ACK를 전송하고, 중복된 데이터는 무시한다.

## 3.4.2 파이프라이닝 된 신뢰적인 데이터 전송 프로토콜

하지만 RDT 3.0도 현대의 인터넷에서는 문제가 존재한다. 바로 지연시간이 너무 길다는 것이다. 그림을 보면 바로 이해가 될 것이다.
![](https://velog.velcdn.com/images/calzone0404/post/ed3de528-1850-4077-9691-f42b11659835/image.png)

하나의 패킷을 보내고 RTT만큼 기다리면, 보내려는 패킷이 매우 많은 경우 많은 지연시간을 초래한다.

따라서 현대의 인터넷은 **파이프라이닝**기법을 통해 RTT를 최소화하였다.
![](https://velog.velcdn.com/images/calzone0404/post/bb08a2b7-1d5a-44fb-b479-fe21d9508bdc/image.png)

즉, 패킷을 여러개를 한번에 파이프라인에 채워 넣어서 보내버리면 이러한 지연시간을 줄일 수 있다.

파이프라이닝 방식은 다음과 같은 중요성을 가진다.

- 순서 번호의 범위가 커져야 한다. 왜냐하면 여러개를 한번에 전송하기 때문이다. 즉 여러개의 패킷에 대해 각각 개별된 순서 번호를 가져야 한다.

- 프로토콜의 송신 측과 수신 측은 패킷 하나 이상을 버퍼링해야한다. 최소한 송신자는 전송되었으나 아직 확인응답되지 않은 패킷을 대기시켜야 한다(버퍼링해야 한다).

이러한 방식을 결정하는 접근 방법으로 `GBN`과, `SR`이 있다.

## 3.4.3 Go-Back-N (GBN)

> GBN 프로토콜에서 송신자는 확인 응답을 기다리지 않고, 여러 패킷을 전송할 수 있다.

하지만, 파이프라인에서 확인응답이 안된 패킷의 최대 허용 개수는 `N`개로 제한된다.

![](https://velog.velcdn.com/images/calzone0404/post/b68be080-93a4-428d-86c9-d348e2fbb92a/image.png)

여기서, 확인응답이 안 된 가장 오래된 패킷의 순서 번호를 `base`로 정의한다. 또한 전송 후 아직 확인 응답이 되지 않은 패킷 중 가장 작은 순서 번호(가장 오래된 패킷)를 `nextseqnum`이라고 한다.

- 따라서 **[0, base-1]** 순서번호는 이미 전송되고, 확인응답이 된 패킷이다.
- **[base, nextseqnum-1]** 순서번호는 송신은 했는데 아직 확인 응답을 받지 못한 패킷이다.
- **[nextseqnum, base+N-1]** 상위 계층에서 데이터가 전달되면 바로 사용이 가능한 공간이다.
- **base+N** 이상의 순서번호는 아직 사용할 수 없는 상태라고 생각하자.

> 그림처럼 전송되었지만 아직 확인 응답이 되지 않은 패킷과 현재 사용 가능한 크기를 합친 공간의 크기 `N`을 `Window Size`라고 부른다.

> GBN 프로토콜은 Sliding Window Protocol이라고 부를 수 있다.

- Window는 순서 번호 공간에서 패킷에 대한 응답이 도착하면 **오른쪽으로 한 칸씩 슬라이드한다**.

### 동작 방식 FSM

#### 송신측
![](https://velog.velcdn.com/images/calzone0404/post/b4919491-eb98-4c6b-8fbe-d828fd1ff212/image.png)

#### 수신측
![](https://velog.velcdn.com/images/calzone0404/post/483221b2-0e8e-4661-94c7-31d7e91be6a9/image.png)

### 송신 데이터의 손실이 발생하는 경우

![](https://velog.velcdn.com/images/calzone0404/post/2012c1f3-8c30-441f-8fa9-7fa16d384a57/image.png)

그림과 같이 $N=4$인 Window size에 패킷 4개를 보냈는데, packet 2번이 손실되었다면,
- 수신측 입장에서는 2번에 대한 순서의 패킷을 받아야 하는데, 3번 패킷이 전달되었으므로 이 3번 패킷을 버리고, **1번 패킷까지 다 받았다는 신호를 보낸다 (ACK 1)**

- 이후 4번, 5번도 같은 이유로 버려버린다.

- 송신측에서는 pkt2를 분명 보냈는데, 응답패킷에 대한 timeout이 발생하였으므로 다시 pkt2부터 수신지로 보내게 된다.

## 3.4.4 Selective Repeat (SR)

GBN의 단점으로는 하나의 패킷에 Loss가 발생하면 다시 그 패킷부터 순서대로 보내야 하는 불필요한 패킷 전송이 발생한다.

> 따라서 **선택적 반복** 프로토콜은 수신자에서 오류가 발생한 패킷을 수신했다고 의심되는 패킷만을 송신자가 다시 전송하므로 GBN처럼 불필요한 패킷 전송이 일어나지 않는다.

![](https://velog.velcdn.com/images/calzone0404/post/0e98f156-a19b-4ee9-a371-9bcefedd950d/image.png)

SR에서는 각 전송된 패킷에 대해 개별적인 확인응답을 보내도록 한다. 따라서 위 그림과 같이 연속적으로 응답된 패킷이 존재하는것이 아니라, **선택적으로 존재**하게 된다.

SR 수신자는 패킷의 순서와는 무관하게 손상 없이 전달된 패킷에 대해 확인응답을 한다.

순서가 바뀐 패킷은 버퍼에 저장해놓고, 이전에 빠진 패킷이 모두 전달되어 **완전한 순서**를 이룬다면 상위 계층으로 전달하게 된다.

![](https://velog.velcdn.com/images/calzone0404/post/b2d99f62-3318-4031-8146-6a5b56dc9ce5/image.png)

### 송신측
1. 상위 계층에서 데이터가 수신될 때, 송신자는 패킷의다음 순서 번호를 검사한다.

2. 순서 번호가 송신자 Window 범위에 포함되면 패킷을 생성해서 송신한다.

3. Window 범위에 포함되지 않으면 GBN처럼 되돌려보낸다.
4. 각 송신하는 패킷마다 각자의 타이머를 설정해놓고, 어떤 패킷이 타임아웃이 발생하는지 알아내도록 한다.

5. ACK가 수신되었을 때, ACK가 Window 범위 안에 존재하는 경우 해당 패킷이 송신되었다고 표시한다.**(진한 파란색)**

6. 만약 send_base 패킷에 대한 응답이 도착하였다면 send_base 번호는 **아직 응답받지 않은 패킷중 가장 작은 순서번호를 가진 패킷**이 된다.

### 수신측
 
1. 패킷을 순서대로 수신하지 않더라도 각 패킷을 개별적으로 확인하고 ACK를 보낸다.

2. 순서가 맞지 않는 패킷도 버퍼에 저장하고, 누락된 패킷이 도착하면 순서대로 재정렬하여 상위 계층에 전달한다.

## 요약
![](https://velog.velcdn.com/images/calzone0404/post/28fb4594-7633-4033-8114-a77a11cbbc94/image.png)

# 3.5 연결 지향형 프로토콜 TCP

## TCP Connection

> Connection-Oriented(연결 지향형) :
> TCP는 어떤 데이터를 다른 프로세스에 보내기 전에, 두 프로세스가 서로 Handshake 과정을 거쳐야 한다.
> 즉, 데이터 전송을 시작하기 전에 전송을 보장하는 사전 정보를 교환합니다.

TCP 연결은 Full-Duplex를 제공하며, 항상 Point to Point 연결을 통해 통신한다. 따라서 Multicasting은 불가능하다.

## TCP 연결 과정
- Client Process: 연결을 초기화하는 프로세스
- Server Process: 연결 요청을 수락하는 프로세스

## 3 Way Handshake (후에 자세히 다룬다)

1. Client Process는 Server Process와 연결을 설정하려고 시도하는 SYN을 날린다.
2. Server Process는 SYN에 대해 답장 신호 ACK를 날린다.
3. Client Process는 ACK 신호에 대해 ACK 신호를 날린다.
4. 연결이 수립된다.

이러한 과정을 통해 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장할 수 있게 된다.

## TCP Segment 구조

TCP Segment는 TCP Header와 Client가 보내려는 Data로 구성된다.

### Header 구성 요소
- 출발지와 목적지 포트 번호
- 체크섬 필드
- 32비트 Sequence 번호 필드
- 32비트 ACK 번호 필드
- 16비트 Window 크기
	- Window는 수신자가 받아들이려는 바이트의 크기를 나타냅니다.
- 4비트의 TCP Header 길이 필드
- Option 필드
- Flag 필드
- ACK: ACK 필드의 값이 유효한지 확인하는 Flag
- RST, SYN, FIN: 연결 설정 및 해제에 사용
- PSH: 수신자가 데이터를 상위 계층에 즉시 전달해야 함을 알리는 Flag
- URG: 긴급 데이터 표시 Flag

  ![](https://velog.velcdn.com/images/calzone0404/post/42cc3376-6620-4189-9582-8c937efd7cad/image.png)

## Sequence 번호와 ACK 번호

실제 통신을 시작할 때, 수많은 패킷이 송수신 된다. 그런데, SYN과 ACK 번호가 존재하지 않는다면, 수신한 ACK 패킷이 어떤 패킷에 대한 ACK인지 모른다. 이를 위해서 Sequence 번호가 필요하다.

- **Sequence 번호**는 TCP Segment의 연속된 데이터 번호이다. 이때, Sequence 번호는 전송되는 Segment의 가장 앞에 있는 숫자를 표기한다.

> 예를 들어, 1129 ~ 1181까지의 Byte Stream을 전송한다면 Sequence Number는 **1129**이다.
  
- **ACK 번호**는 상대방으로부터 받아야 하는 다음 TCP Segment 데이터 번호이다. 즉, 해당 번호 앞까지 데이터 처리를 완료하여 해당 번호 TCP Segment를 전송해달라고 요청하는 의미이다.
  
> 예를 들어, 1181까지 처리한 경우, 다음 번호인 1182부터 TCP Segment 데이터를 요청하기 위해 ACK 번호는 1182이다.
  
### Telnet 통신 예시
![telnet](https://velog.velcdn.com/images/calzone0404/post/f85bd5c0-6f46-48eb-800e-040c0a3e8235/image.png)

1.
Seq 42 : A에서 송신한 첫 번째 Segment는 순서 번호 42를 가진다.
ACK 79 : A는 78까지 데이터 처리를 완료했으므로, 다음 순서인 79번째 데이터를 요청한다.

2.
Seq 79 : 79번째 데이터를 A로 송신한다.
ACK 43 : B는 42번째 데이터를 수신했으므로, 43번째에 해당하는 데이터를 보내달라고 **ACK 번호로 43**을 가진다.

3.
Seq 43 : 43번째 데이터를 B로 송신한다.
ACK 80 : A는 79에 대한 Segment 데이터를 받았으므로, 80번째 Segment를 보내달라고 요청한다.

## 왕복시간 Rount Trip Time (RTT) & Timeout

- RTT : 세그먼트가 전송된 시간부터 확인 응답 될 때 까지의 시간

> TCP는 손실된 Segment를 발견하기 위해 **Timeout/재전송 방법**을 사용한다.
이때, Timeout 시간은 연결의 **RTT** 시간보다 당연하게도 더 커야한다.

### 왕복 시간 예측

- SampleRTT : Segment가 송신된 시간으로부터, 그 Segment에 대한 ACK신호가 도착한 시간까지의 시간 길이

> 대부분의 TCP는 한 번에 하나의 SampleRTT 측정을 시행한다.
  
- SampleRTT 값은 라우터에서의 혼잡, 종단 시스템에서의 부하 변화 때문에 Segment마다 시간이 전부 다르므로 **평균적인 RTT 시간(EstimatedRTT)**을구한다.

![estRTT](https://velog.velcdn.com/images/calzone0404/post/f2706a40-046f-4579-a0a8-f105250e4e9a/image.png)

### 재전송 타임아웃 주기 설정

- DevRTT : SampleRTT가 EstimatedRTT와 얼마나 벗어나 있는지에 대해서 정의한다.

![devRTT](https://velog.velcdn.com/images/calzone0404/post/0445d061-d670-40c8-963d-5b889eddee76/image.png)

- Timeout값은 EstimatedRTT에 약간의 여윳값을 더하여 구한다.

![TimeoutInterval](https://velog.velcdn.com/images/calzone0404/post/4ef67e52-9a77-4d0d-8860-46d871fcdd63/image.png)

## 신뢰성 있는 데이터 전송

> 1. TCP는 IP의 비신뢰적인 최선형 서비스에서 rdt를 생성한다.
> 2. Timeout, 중복 ACK 발생 시, 재전송을 수행한다.

### TCP 송신자 이벤트

- App으로부터 Data 수신 시
  - Sequence 번호를 붙여서 Segment를 생성한다.
  - 아직 다른 Segment에 대해 실행중이 아니라면, Timer를 시작한다.

---

- Timer Timeout
  - Timeout 발생 시 Segment를 재전송한다.
  - Timer를 초기화하고, 다시 시작한다.

---

- ACK 수신 시
  일단 TCP는 변수 SendBase와 ACK값 $y$를 비교한다.
  
  > SendBase : ACK가 확인 되지 않은 / 가장 오래된 바이트의 순서 번호
  > SendBase - 1 : 수신자에게서 정확하게 차례대로 수신되었음을 알리는 마지막 바이트의 순서번호
  
  TCP는 누적된 ACK를 사용하고, $y$는 $y$바이트 이전의 모든 바이트의 수신을 확인한다.
  
  이때, $y > SendBase$이면, ACK는 이전에 ACK응답이 안 된 하나 이상의 Segment들을 확인한다.
    1. 송신자는 자신의 SendBase 변수를 갱신
    2. 아직 ACK응답이 안 된 Segment가 존재한다면, Timer를 다시 시작
    
### 예시
![](https://velog.velcdn.com/images/calzone0404/post/173f8eca-18f9-4d39-baef-4d03af6da232/image.png)

**오른쪽 그림 설명**

1. 92 ~ 100까지 데이터를 송신
2. 100 ~ 120까지 데이터를 송신
3. 100까지 받았다고 ACK
4. 120까지 받았다고 ACK
5. ACK 100에 대해 A는 92~100에 대해 응답이 아직 안되어있던 신호이므로 SendBase를 100으로 갱신
6. ACK 120에 대해 A는 100~120에 대해 응답이 아직 안되어있던 신호이므로 SendBase를 120으로 갱신
7. 92~100까지 데이터를 송신
8. 이미 받은 데이터이므로 120까지 받았다고 ACK 송신
9. A는 92~100에 대해 이미 응답을 받았으므로 SendBase를 갱신하지 않음

![examp](https://velog.velcdn.com/images/calzone0404/post/ade317a3-8f90-4281-94eb-53733c3a79e2/image.png)

1. A는 92~100에 대한 데이터 송신
2. A는 100~120에 대한 데이터 송신
3. B는 92~100에 대한 데이터를 송신받았기 때문에 ACK번호 100을 송신했으나, 모종의 이유로 데이터가 날라감
4. B는 100~120에 대한 데이터를 송신받았기 때문에 ACK번호 120을 송신
5. A는 92~100에 대한 ACK응답을 아직 받지 못하여 **Timeout**이 발생함
따라서 호스트 A는 92~100을 **재전송**
6. B는 이미 92~100에 대한 데이터를 받았으므로, 수신된 데이터를 버리고 다시 ACK 100을 송신함

![example2](https://velog.velcdn.com/images/calzone0404/post/18dddea2-5e7b-450f-be25-8d1d5a677ce9/image.png)
1. A는 92~100에 대한 데이터 송신
2. A는 100~120에 대한 데이터 송신
3. B는 92~100에 대한 데이터를 송신받았기 때문에 ACK번호 100을 송신했으나, 모종의 이유로 데이터가 날라감
4. B는 100~120에 대한 데이터를 송신받았기 때문에 ACK번호 120을 송신
5. ACK 120이 먼저 도착하여 SendBase = 120으로 갱신되고, 120 ~ 135에 대한 데이터 송신
6. 그런데, A는 92~100에 대한 ACK응답을 아직 받지 못하여 **Timeout**이 발생함

### TCP 빠른 재전송

- **Timeout주기가 때때로 김**
  긴 Timeout 주기는 Host간의 지연을 증가시킴.
  그러나, 송신자는 중복 ACK에 의한 Timeout이 일어나기 전에 패킷 손실 발견 가능
  
---

- **중복 ACK**
  송신자가 이미 이전에 받은 ACK에 대한 재확인응답 ACK

---

- **빠른 재전송**
  만약 TCP 송신자가 같은 데이터에 대해 3개의 중복 ACK를 수신했다면,
  ACK된 Segment Data의 다음 3개의 Segment가 분실되었음을 의미한다.
  
  ![fastretrans](https://velog.velcdn.com/images/calzone0404/post/34eea288-0400-417d-9e01-8dfbf4b9459b/image.png)
  
위 그림을 보면 seg 100 전송이 실패하게 되어, ACK 100이 중복으로 전송되는 모습을 확인할 수 있다. 
따라서 A는 Timeout이 일어나기 전에, 100이후로 데이터 전송이 실패했다고 판단하여 100~120에 대한 데이터를 다시 전송해준다.


## TCP 흐름 제어

> TCP는 송신자가 수신자의 버퍼를 Overflow시키는 것을 방지하기 위해 흐름 제어 서비스를 제공함

따라서 TCP 송신자는 네트워크에서 **혼잡 제어**에 의해 송신이 억제될 수 있다.

---

### 흐름 제어 과정
> TCP는 Receive Window라는 변수를 통해 흐름 제어를 수행한다

 Receive Window는 수신자가 가용한 버퍼 공간이 얼마나 되는지 알려주는데 사용한다.
 이때 TCP는 Full-Duplex 이므로 각 송신자는 별개의 Receive Window를 갖는다.
 
- 예시
  A가 B에게 대용량 파일을 전송한다고 가정하자.
  
  이때 B는 이 연결에 대해 수신 버퍼를 할당한다.
  할당된 수신 버퍼의 크기 : **RcvBuffer**
  
  B의 Process는 Buffer로부터 데이터를 읽으며 변수를 정의한다.
    1. LastByteRead : B의 Process에 의해 Buffer로부터 읽힌 Data Stream의 마지막 Byte 번호
      - LastByteRcvd : B에게 도착하여 Receive Buffer에 저장된 Data Stream의 마지막 Byte 번호
      - **RcvWindow(rwnd)** : 버퍼의 여유 공간 = RcvBuffer $-$ $($ LastByteRcvd $-$ LastByteRead $)$

![](https://velog.velcdn.com/images/calzone0404/post/474bc390-1af5-45b2-ba0b-2ab815c6ba76/image.png)

## TCP 연결 관리

> TCP는 어떤 데이터를 다른 프로세스에 보내기 전에, 두 프로세스가 서로 Handshake과정을 거쳐야 한다.

### 3-way Handshake - 연결 수립

![](https://velog.velcdn.com/images/calzone0404/post/9110e0ad-a620-48b1-befd-e9d5e4a02247/image.png)

1. Client TCP는 Servver에게 SYN을 송신한다.
   - 이때, SYN bit을 1로 설정한다 (SYN = 1)
   - Client는 최소 순서 번호(client_isn)을 임의로 선정해서 SYN을 보낼 때 같이 보낸다.

---

2. SYN Segment가 Server로 도착하면
   - Server는 SYN 번호를 추출한다.
   - 연결에 필요한 TCP Buffer와 변수를 할당한다.
   - Client TCP에게 SYNACK를 송신한다
     - 이때, SYNACK bit은 1로 설정된다 (SYN = 1)
     - client_isn을 순서 번호로 받았으므로, client_isn + 1을 SYNACK와 함께 송신한다

---

3. Client가 연결 승인 Segment 수신시
   - Client는 연결에 필요한 TCP Buffer와 변수를 할당한다.
   - Client는 Server에게 SYNACK에 대한 확인 Segment를 송신한다
     - 클라이언트는 client_isn + 1을 수신하였으므로, 이 값을 받았다는 신호로 **ACK = client_isn + 1**과, 연결이 수립되었으므로 **SYN = 0** 신호를 보낸다


### 3-way Handshake - 연결 해제

> TCP 연결에 참여하는 두 Host중 하나가 연결을 끊을 수 있다.
연결이 끊어지면, 각각 할당했던 Buffer와 변수가 회수된다.

![](https://velog.velcdn.com/images/calzone0404/post/ab7fc695-fc43-402a-8497-e443d96b32b4/image.png)

1. Client가 Process 종료 명령을 내리고, 연결 해제를 위해 **FIN = 1** 신호를 보낸다.
2. Server가 FIN을 수신하면, Client에게 **ACK = 1**신호를 보낸다.
3. 또, Server는 연결 해제를 위해 Client에게 **FIN = 1** 신호를 보낸다.
4. Client는 FIN에 대해 **ACK = 1**를 보낸다.
5. 연결이 종료된다.


### TCP Handshake 상태 변이 - Client

![](https://velog.velcdn.com/images/calzone0404/post/3a2881da-ff06-451a-a9aa-21301117b923/image.png)


### TCP Handshake 상태 변이 - Server

![](https://velog.velcdn.com/images/calzone0404/post/72c3b5b0-61a9-4e16-98f6-7ceeb7e78871/image.png)

### 수정사항 및 추가사항

수정할 점과 추가해야 할 점을 정리하여 반영하겠습니다.

# 3.6 혼잡 제어의 원리

> **혼잡(Congestion)**: 너무 많은 송신자가 너무 높은 속도로 데이터를 보내려고 시도할 때 발생

## 혼잡의 원인과 비용 - 예시 1

- 두 Host A, B가 각각 출발지와 목적지 사이에서 단일 Router를 공유합니다.
- A와 B의 프로세스가 전송률 $λ_{in}$으로 데이터를 전송합니다.
- Output Link의 수용량: $R$
- Router Buffer는 무한하다고 가정합니다.

![](https://velog.velcdn.com/images/calzone0404/post/8785dc16-82bc-4739-bea3-3592ebd8cd33/image.png)

### 1. 연결 당 처리량

![](https://velog.velcdn.com/images/calzone0404/post/297f5ea7-4d2b-46c3-9865-a67ab95bcf27/image.png)

0 ~ $R/2$ 사이의 전송 속도에서는 수신 측의 처리량은 송신자의 전송률과 같습니다. 그러나 전송 속도가 $R/2$를 초과하면 처리량은 $R/2$로 제한됩니다. 즉, A와 B가 전송률을 아무리 높여도 각자의 처리량은 $R/2$보다 높아질 수 없습니다.

### 2. 평균 지연

![](https://velog.velcdn.com/images/calzone0404/post/8fac3ee9-866e-4266-9da9-c2beca2520e5/image.png)

전송률이 $R/2$에 근접할 경우 평균 지연은 점점 커집니다. 전송률이 $R/2$를 초과하면 평균 지연은 무한대가 됩니다.

## 혼잡의 원인과 비용 - 예시 2

- 송신자 A와 B
- 유한한 크기의 Buffer를 가진 라우터 1개
- A와 B의 프로세스가 전송률 $λ_{in}$으로 데이터를 전송합니다.
- Output Link의 수용량: $R$

![](https://velog.velcdn.com/images/calzone0404/post/7126c959-670e-43c2-a4d1-692f648c6a6a/image.png)

라우터의 버퍼 크기가 유한하므로 버퍼가 가득 차면 도착하는 패킷은 버려집니다. 패킷이 버려지면 송신자가 해당 패킷들을 재전송하게 됩니다.

### 다양한 시나리오

![](https://velog.velcdn.com/images/calzone0404/post/394d60cd-fc14-43e9-bf48-085557e8cfe6/image.png)

A. **어떠한 손실도 발생하지 않음**:
   - 연결의 처리량: $λ_{in}$
   - 송신률은 $R/2$를 초과할 수 없습니다.

B. **패킷 손실 및 재전송 발생**:
   - 제공된 부하 $λ_{in}$이 $R/2$일 경우 데이터의 전송률은 $R/3$
   - 전송된 데이터 중 $R/2$는 원래 데이터이고, $0.166R$은 재전송 데이터입니다.

C. **타임아웃으로 인한 불필요한 재전송**:
   - 원래 데이터와 재전송 데이터가 모두 수신자에게 도착합니다.
   - 제공된 부하가 $R/2$일 때의 처리량은 $R/4$입니다.
   - 지연으로 인해 불필요한 재전송이 발생하고, 라우터는 불필요한 복사본들을 전송합니다.

## 혼잡 제어에 대한 접근법

### 종단 간 혼잡 제어

> 네트워크 계층은 Congestion Control을 위해 Transport Layer에게 직접적인 지원을 받지 않습니다.

- TCP Segment 손실 및 증가하는 RTT 지연값을 네트워크 혼잡 발생의 신호로 간주합니다.
- TCP는 그에 따라 Window size를 줄입니다.

### 네트워크 지원 혼잡 제어

> 라우터들이 네트워크 내에서 Congestion과 관련된 Feedback을 송신자, 수신자 또는 Network 참여자 모두에게 전달합니다.

#### ATM ABR(Available Bit Rate) 혼잡 제어

- 라우터는 자신의 Output Link에 제공할 수 있는 전송률을 송신자에게 명확히 알릴 수 있습니다.
- 송신자의 경로가 여유롭다면 이용 가능한 대역폭 내에서 송신을 수행합니다.
- 송신자의 경로가 혼잡하다면 송신자는 최소 보장 속도로 송신하도록 제어됩니다.

**RM (Resource Management)**
- 송신자가 보낸 데이터 Cell들을 분산하여 전송한다.
- RM Cell의 비트들은 Switch에서 설정된다. 

---

# 3.7 혼잡 제어

## 3.7.1 전통적인 혼잡 제어 방식

- **일단 전송률을 올리고**, 패킷 손실이 발생하기 전까지 사용 가능한 대역폭을 탐색한다.

- **Additive Increase** : 혼잡 윈도 크기(CongWin)을 RTT마다 1 MSS씩 손실이 발생하기 전까지 증가시킨다.

- **Multiplicative Decrease** : 손실 발생 시 CongWin을 절반으로 줄인다.

### TCP 송신자가 자신의 송신률을 제한하는 방법
> 송신 측에서 동작하는 TCP 혼잡 제어 메커니즘은 추가적인 변수인 **혼잡 윈도(congestion window, cwnd)**를 추적한다.

이때, 송신하는 쪽에서 ACK가 응답을 받지 못한 데이터의 양은 CongWin과 Receive Window(수신 윈도 = 버퍼의 여유 공간)의 최솟값을 초과하지 않을 것이다.

> LastByteSent - LastByteAcked ≤ min{cwnd, rwnd}

따라서 CongWin의 값을 조절하여 송신자는 링크에 데이터를 전송하는 속도를 조절할 수 있다.

### TCP 송신자가 혼잡을 감지하는 방법
- 손실 발생 = 3개의 중복된 ACK 신호를 받거나, Timeout이 된 경우. 이때 송신자는 손실 발생 이후 CongWin을 줄인다.

#### 1. TCP Slow Start
1. TCP 연결이 시작되면, CongWin = 1MSS로 초기화 한다. 이때 초기 전송률은 MSS/RTT이다.

2. 처음 손실 이벤트가 발생하기 전까지는 지수적으로 TCP 전송률을 늘려나간다.
   - 즉, RTT마다 CongWin값을 2배씩 증가시킨다.

> 처음 전송률은 느리지만, RTT가 지날수록 지수적으로 빠르게 전송률이 증가하게 된다.

#### 2. 혼잡 회피 (Congestion Avoidance)
- 3개의 중복된 ACK가 전달되었다면, **CongWin**은 절반으로 줄고, 선형적으로 증가한다.

- Timeout이 발생하였다면, **CongWin**은 1MSS로 초기화되고, 지수적으로 증가한다. 이후, 임계값 근처에서는 선형적으로 증가한다.
  - 정확히 언제 지수에서 선형으로 변환되는가?
  > CongWin값이 Timeout 이전에 절반으로 줄어들었을 때 선형으로 변환된다.
  - 따라서 손실 발생 시 임계값은 손실 발생 직전 CongWin/2 값으로 설정된다.

#### 3. Fast Recovery (빠른 회복)

빠른 회복은 추천되지만 필수는 아니다. 빠른 회복 단계에서는 3개의 중복 ACK가 수신되었을 때 혼잡 윈도를 절반으로 줄이고, 이후 새로운 데이터가 전송될 수 있도록 한다. 
이는 혼잡 회피 단계와 유사하게 작동하지만, 손실이 발생한 후 전송률을 더 빨리 회복할 수 있도록 한다.

### 결론

1. CongWin이 임계값보다 낮으면 송신자는 Slow Start 단계에서 윈도 크기를 기하급수적으로 증가시킨다.
2. CongWin이 임계값을 초과하면 송신자는 혼잡 회피 단계에 들어가고, 윈도 크기는 선형적으로 증가한다.
3. 3중 중복 ACK가 발생하면 임계값이 CongWin의 절반 값으로 설정되고 CongWin이 임계값으로 설정된다.
4. Timeout이 발생하면 임계값은 CongWin의 절반 값으로 설정되고, CongWin은 1 MSS로 설정된다.

## 3.7.2 명시적 혼잡 알림 (Explicit Congestion Notification, ECN)
![image](https://github.com/Scanf-s/CS_Book_Summary/assets/105439069/f9f6c67b-5658-4202-843c-065727325735)

- ECN은 네트워크가 TCP 송신자와 수신자에게 혼잡 상태를 명시적으로 알리는 방법이다.
- 네트워크 계층에서 IP 데이터그램 헤더의 두 비트가 ECN을 위해 사용된다.
- 혼잡 상태를 나타내는 비트가 설정되면, 이 정보는 목적지 호스트로 전달되고, 목적지 호스트는 송신 호스트에 혼잡 상태를 알린다.

### 지연 기반 혼잡 제어 (Delay-Based Congestion Control)

- 지연 기반 혼잡 제어는 패킷 손실이 발생하기 전에 혼잡 상태를 탐지하기 위해 지연 시간을 측정하는 접근법이다.
- TCP Vegas는 송신자가 모든 승인된 패킷의 RTT를 측정하고, 이 값을 기반으로 혼잡을 감지하여 송신률을 조절한다.

## 3.7.3 TCP 공정성

![image](https://github.com/Scanf-s/CS_Book_Summary/assets/105439069/d13971cc-dedf-43db-a344-e3196938ea37)

각기 다른 종단 간의 경로를 갖지만, 모두 **R bps**의 전송률인 병목 링크를 지나는 **K개의 TCP 연결**을 생각해보자. 
각 연결이 큰 파일을 전송하고 있고, 병목 링크를 통과하는 UDP 트래픽이 없다고 가정했을 때, 각 연결의 평균 전송률이 **R/K**에 가깝다면 혼잡 제어 메커니즘이 **공평**하다고 한다. 
즉, 각 연결은 링크 대역폭을 동등하게 공유한다.

### 공평성과 UDP

UDP는 혼잡 제어를 갖고 있지 않다. TCP의 관점에서 보면 UDP 상에서 수행되는 멀티미디어 애플리케이션은 공평하지 못하다. 즉, 다른 연결들과 협력하지도 않으며, 그들의 전송률을 적당하게 조절하지도 않는다.

TCP 혼잡 제어는 혼잡(손실) 증가에 대해 전송률을 감소시키므로, 그럴 필요가 없는 UDP 송신자들이 TCP 트래픽을 밀어낼 가능성이 있다.

> UDP 트래픽으로 인해 인터넷이 마비되는 것을 방지하기 위한 혼잡 제어 방식의 개발이 필요하다.

### 공평성과 병렬 TCP 연결

UDP 트래픽이 공평하게 행동하도록 강요하더라도, TCP 기반 애플리케이션의 다중 병렬 연결의 사용을 막을 방법이 없기 때문에 공평성 문제는 여전히 완전하게 해결되지 않는다. 애플리케이션이 다중 병렬 연결을 사용할 때는 혼잡한 링크 대역폭의 더 많은 부분을 차지한다.
