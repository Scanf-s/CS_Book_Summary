# 1.1 인터넷이란?
## 1.1.1 구성요소로 본 인터넷

- **host, end-system** : 컴퓨터 네트워크에 연결되는 모든 장치

- 각 종단 시스템은 **통신 링크**와 **패킷 스위치**의 네트워크로 연결됨. **링크**는 동축 케이블, 구리선, 광케이블 등.. 을 포함한 다양한 물리 매체로 구성됨
각 링크들은 다양한 전송률을 이용하여 데이터를 송수신하고, 초당 비트 수를 의미하는 **bps**단위를 사용함

- **패킷** : 송신지에서 segment를 생성하고, header를 붙인 정보 패키지를 말함.
이러한 패킷을 전달해주는 패킷 스위치로는, Router와 Link-layer Switch가 존재함.
둘 모두 최종 목적지 방향으로 패킷을 전달하는 역할을 함.
패킷이 목적지까지 거쳐가는 경로를 **route 또는 path**라고 부름.

- **ISP**: 우리가 알고있는 SKT, KT, LG같이 인터넷 서비스를 제공해주는 업체.
종단 시스템이나, CP(content provider)에게 인터넷 접속을 제공함.

 ISP끼리도 물론 서로 연결되어있음. ISP도 상위계층과 하위계층으로 나뉘는데, **하위계층은 주로 국가 내부이고, 상위계층은 주로 국제적인 연결을 담당함**.
 
- 프로토콜 : end-system, packet switch 등.. 여러 인터넷의 구성요소에 대한 규칙

 **TCP/IP** : 인터넷에서 가장 중요한 프로토콜
 IP 프로토콜은 라우터 및 종단 시스템 사이에서 송수신되는 패킷의 형태를 정함.
 
 **IETF** : 인터넷 프로토콜(표준)을 정하는 기구
 **RFC** : IETF에서 정한 표준 문서. 주로 TCP, IP, HTTP, SMTP같은 프로토콜을 정의함
 
## 1.1.2 서비스 측면에서 본 인터넷

- **분산 어플리케이션**: 서로 데이터를 주고받는 수많은 종단 시스템을 포함하는 어플리케이션. 예를 들면, 실시간 스트리밍, 온라인 소셜 미디어, 멀티플레이 게임 등.. 이 있다.
 
 이때, 패킷 교환기는 종단 시스템간의 데이터 교환을 쉽게해주지만, 어플리케이션 자체에는 관심을 가지지 않음.
 
 어플리케이션끼리 데이터를 교환하려면, **Socket interface**를 통해야한다.
 > 소켓 인터페이스는, 하나의 host 어플리케이션에서 다른 host의 어플리케이션 목적지로 데이터를 전달하도록 요구하는 인터페이스이다.
 다시 말해, 송신 프로그램이 따라야하는 규칙의 집합이다.
 
  예를 들어, 우편 시스템에서 B에게 편지를 보내려면, 다음과 같은 규약을 따라야 한다.
  1. 편지내용(데이터)를 써야한다.
  2. B의 이름, 주소, 우편번호를 써야한다.
  3. 우편을 밀봉하여 우체통에 넣어야한다.
  
  이러한 규칙이 바로 Socket interface 이다.
  
## 1.1.3 프로토콜이란 무엇인가?
 
 ![](https://velog.velcdn.com/images/calzone0404/post/0dce13df-133f-408b-a179-ebc593d5817d/image.png)
위 그림을 보면 이해하기 쉽다.

사람과 대화를 하기위해서는, 먼저 자신을 소개하는 과정이 있어야하며, 이후 지금이 몇시인지 묻는 행위를 할 수 있다.

물론, "안녕" 인사에 대해서, 부정적인 반응이 있을 수 있는데, 이는 통신의 거부행위로 이해할 수 있다.

이러한 행위의 기반으로, 만약 다른 문화권 사람의 대화는 서로 다른 프로토콜(언어)을 가지고 있기 때문에, 의사소통(통신)이 원활하지 않게된다.

마찬가지로 컴퓨터 네트워크에서도, 사람처럼 메세지를 교환하고 행동을 취하는 장치들이 존재한다.

>**프로토콜**: 둘 이상의 통신 개체 간에 교환되는 메세지 포맷과 순서뿐만 아니라, 메세지의 송수신 및 다른 이벤트에 따른 행동들을 정의한다.

<hr>

# 1.2 네트워크의 가장자리
![](https://velog.velcdn.com/images/calzone0404/post/f5a953aa-afa6-47fd-aa7f-9ac9fd327822/image.png)

## 1.2.1 접속 네트워크 유형

### 가정용 접근 네트워크(Residential Access Networks):

- **DSL**(Digital Subscriber Line): 전화 회선을 사용하여 서비스를 제공하는 방식. 전화와 인터넷 서비스를 동시에 사용할 수 있으며, 거리에 따라 속도가 감소할 수 있다.
![](https://velog.velcdn.com/images/calzone0404/post/f16e2bf2-93e5-40d3-9878-f18e0832f915/image.png)

- **HFC**(Hybrid Fiber-Coaxial): 케이블 TV 네트워크를 통해 인터넷 서비스를 제공한다. 단점으로는, 다수의 사용자가 대역폭을 공유하기 때문에, 사용량이 많은 시간대에는 속도가 느려질 수 있다.
![](https://velog.velcdn.com/images/calzone0404/post/cf1c225d-150a-429c-be54-0a5d8b7abd18/image.png)

- **FTTH**(Fiber To The Home): 광섬유를 집까지 직접 연결하는 방식으로, 매우 높은 속도를 제공할 수 있다. (현재 주로 사용하는 방식)
![](https://velog.velcdn.com/images/calzone0404/post/7bc7b59a-208f-4df9-b04c-b85c3789b21a/image.png)


### 기업용 접근 네트워크(Enterprise Access Networks):

일반적으로 이더넷(Ethernet) 기술을 사용하며, 기업 내부의 LAN(Local Area Network)을 통해 연결된다. 기업용 네트워크는 보통 고속의 인터넷 접속과 보안을 강화한 네트워크 구조를 가진다.

### 광역 무선 접근 네트워크(Wide-Area Wireless Access Networks):

- 셀룰러 네트워크(Cellular Networks): 이동 통신사가 제공하는 네트워크로, 3G, 4G, LTE 등의 기술을 사용해 이동 중에도 인터넷 접속이 가능하다.

- 위성 인터넷(Satellite Internet): 특히 농촌이나 접근이 어려운 지역에서 사용될 수 있으며, 위성을 통해 인터넷 서비스를 제공하는 기술이다.

## 1.2.2 물리적 매체
- 유선 매체(Wired Media): 구리선, 동축 케이블, 광섬유 등이 있으며, 각각의 매체는 전송 속도, 설치 비용, 유지 보수 등에서 다른 특성을 보인다.

- 무선 매체(Wireless Media): 라디오 주파수, 마이크로웨이브, 적외선 등을 통해 데이터를 전송한다. 무선 매체는 설치가 간편하고 유연하지만, 물리적 장애물에 의해 신호가 약해질 수 있다.

<hr>

# 1.3 네트워크 코어