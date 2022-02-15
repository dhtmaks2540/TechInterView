# [TCP] 3 way handshake & 4 way handshape

연결을 성립하고 해제하는 과정을 말한다.

<br/>

## 3 way handshake - 연결 성립

TCP는 정확한 전송을 보장해야 한다. 따라서 통신하기에 앞서, 논리적인 접속을 성립하기 위해 3 way handshake 과정을 진행한다.

![](https://t1.daumcdn.net/cfile/tistory/2352F94A58D7287932)

1. 클라이언트는 서버에 접속을 요청하는 **SYN(a)** 패킷을 보낸다.
2. 서버는 클라이언트의 요청인 **SYN(a)** 를 받고, 클라이언트에게 요청을 수락한다는 **ACK(a+1)** 와 **SYN(b)** 이 설정된 패킷을 발송한다.
3. 클라이언트는 서버의 수락 응답인 **ACK(a+1)** 와 **SYN(b)** 패킷을 받고 **ACK(b+1)** 를 서버로 보내면 연결이 **성립(establish)** 된다.

<br/>

## 4 way handshake - 연결 해제

연결 성립 후, 모든 통신이 끝났다면 해제해야 한다.

![](https://t1.daumcdn.net/cfile/tistory/2336285058D7288E33)

1. 클라이언트가 연결을 종료하겠다는 **FIN 플래그** 를 전송한다.
2. 서버는 클라이언트의 요청(**FIN**)을 받고 알겠다는 확인 메세지로 **ACK** 를 보낸다.
   1. 그리고나서는 데이터를 모두 보낼 때까지 잠깐 **TIME_OUT** 이 된다.
3. 데이터를 모두 보내고 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 **FIN 플래그** 를 전송한다.
4. 클라이언트는 **FIN 메세지** 를 확인했다는 **메세지(ACK)** 를 보낸다.
5. 클라이언트의 **ACK 메세지** 를 받은 서버는 소켓 **연결을 close** 한다.
6. 클라이언트는 아직 서버로부터 받지 못한 데이터가 있을 것을 대비해 일정 시간 동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정을 거친다(**TIME_WAIT**).

* 4번까지의 통신이 완료되면 연결이 해제되는 것이다.

<br/>

## What is SYN packet? ACK packet?

두 용어는 다음의 약자이다.

* SYN : synchronize sequence number
* ACK : acknowledgement

**TCP Header** 에는 **Code Bit(Flag bit)** 라는 부분이 존재한다. 이 부분은 총 **6Bit** 로 이루어져 있으며 각각 한 bit들이 의미를 갖고 있다. **Urg-Ack-Psh-Rst-Syn-Fin** 순서로 되어 있으며 해당 위치의 비트가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다. SYN 패킷일 경우엔 000010이 되고 ACK 패킷일 경우에는 010000이 되는 것이다.

<br/>

## What two types of packets?

일단 연결을 성립하려면 서로 통신이 가능한지를 먼저 파악하기 위해 패킷을 주고받아야 한다는 것까지는 이해가 쉽다. 그런데 두 종류의 패킷을 주고 받는다. **요청** 과 **응답** 에 해당하는 패킷을 주고 받아야 하기 때문에 두 종류인 것이다.

<br/>

### Why 3-way? Is not enough 2-way?

비유를 들어보자. 일단 클라이언트가 자신의 목소리가 들리는지 물어본다(SYN). 서버는 클라이언트의 목소리가 들린다고 말한다(SYN + 1). 그리고 자신의 목소리가 들리는지 물어본다(ACK). 클라이언트는 서버의 목소리가 들린다고 말한다(ACK + 1). 이런 과정인 셈이다. TCP connection은 양방향성(bidirectional) connection 이다. 클라이언트에서 서버에게 존재를 알리고 패킷을 보낼 수 있다는 것을 알리듯, 서버에서도 클라이언트에게 존재를 알리고 패킷을 보낼 수 있다는 신호를 보내야 한다. 그렇기 때문에 2-way-handshake로는 부족하다.

<br/>

## Why randomized sequence number?

처음 클라이언트에서 SYN 패킷을 보낼 때 Sequence Number에는 랜덤한 숫자가 담겨진다. 초기 sequence number를 ISN이라고 한다. ISN이 0부터 시작하지 않고 난수를 생성해서 number를 설정하는 이유는 무엇인가? Connection을 맺을 때 사용하는 포트(port)는 유한 범위 내에서 사용하고 시간이 지남에 따라 재사용된다. 따라서 두 통신 호스트가 과거에 사용된 포트 번호 쌍을 사용하는 가능성이 존재한다. 서버 측에서는 패킷의 SYN을 보고 패킷을 구분하게 되는데 난수가 아닌 순차적인 number가 전송된다면 이전의 connection으로부터 오는 패킷으로 인식할 수 있다. 이러한 문제가 발생할 가능성을 줄이기 위해서 난수로 ISN을 설정하는 것이다.

<br/>

**참조**
* [원본링크1](https://asfirstalways.tistory.com/356)
* [원본링크2](https://gyoogle.dev/blog/computer-science/network/TCP%203%20way%20handshake%20&%204%20way%20handshake.html)