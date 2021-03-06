# Blocking I/O & Non-Blocking I/O

## I/O 작업

* I/O 작업은 Input/Output의 약자.
* 주로 파일 입출력을 다룰 때, 흔히 볼 수 있다.
* Ex) 두 대 이상의 컴퓨터끼리 서로 네트워크를 통해 통신을 한다고 가정할 때, 한 컴퓨터에서 출력(send)을 하고 다른 한 컴퓨터에서 입력(read)을 받는 과정을 통해 통신할 수 있다.
* I/O 작업은 User Level에서 직접 수행할 수 없고, 실제 I/O 작업을 수행하는 것은 Kernel Level에서만 가능하다.
* User Process, Thread는 커널에게 요청하고 작업 완료 후, 커널이 반환하는 결과를 기다릴 뿐이다.

<br/>

## Blocking Model

가장 기본적인 I/O 모델로, 리눅스에서 모든 소켓 통신은 기본 blocking으로 동작한다.

I/O 작업이 진행되는 동안 유저 프로세스는 자신의 작업을 중단한 채 대기하는 방식이다.

![](https://user-images.githubusercontent.com/41428527/51266321-4ade9700-19fe-11e9-9b23-30bca4faccfd.png)

위의 그림처럼

1. 유저는 커널에게 read 작업을 요청한다.

2. 데이터가 입력될 때까지 대기한다.

3. 데이터가 입력되면 유저(커널 -> 유저)에게 결과가 전달되어야만 유저 자신의 작업에 비로소 복귀할 수 있다.

* **말 그대로 block이 되고, 어플리케이션에서 다른 작업을 수행하지 못하고 대기하게 되므로 자원이 낭비된다.**

<br/>

### 예제

여러 Client 가 접속하는 서버를 Blocking 방식으로 구현하는 경우 I/O 작업을 진행하는 작업을 중지한다. 따라서 다른 Client가 진행중인 작업을 중지하면 안되므로, client 별로 별도의 Thread 를 생성해야 한다. 이로 인해 접속자 수가 매우 많아지고, 많아진 Threads로 컨텍스트 스위칭 횟수가 증가하기 때문에 비효율적인 동작 방식이 되는 것이다.

<br/>

## Non-Blocking Model

위와 같은 blocking 방식의 비효율성을 극복하고자 도입된 방식이다.

I/O 작업이 진행되는 동안 유저 프로세스의 작업을 중단시키지 않는 방식이다.

![](https://user-images.githubusercontent.com/41428527/51266324-4e721e00-19fe-11e9-900a-809ff39e40c1.png)

위의 그림처럼

1. 유저가 커널에게 read 작업을 요청.

2. 데이터가 입력되었든 입력되지 않았든 요청하는 그 순간, 바로 결과가 반환된다.

3. 이때, 입력 데이터가 없으면 입력 데이터가 없다는 결과 메시지를 반환한다.
   * 입력 데이터가 있을 때까지 1~2번을 반복.(2번에서 결과 메시지를 받은 유저는 다른 작업 진행이 가능하다.)

* 입력 데이터가 있으면 유저(커널->유저)에게 결과가 전달된다.

* **이 경우 I/O의 진행시간과 관계가 없기 때문에(대기X) 어플리케이션에서 작업을 오랜 시간 중지하지 않고도 I/O 작업을 진행할 수 있다.**

* **그러나 반복적으로 시스템 호출이 발생하기 때문에 이 경우 역시 자원이 낭비된다.**

<br/>

**참조**
* [원본링크](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Network/Blocking%20I:O%20%26%20Non-Blocking%20I:O.md)
* [참고자료](https://ju3un.github.io/network-basic-1/)