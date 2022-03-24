# Race Condition

Race Condition(경쟁 상태)은 **여러 프로세스들이 동시에 데이터에 접근하는 상황에서, 어떤 순서로 데이터에 접근하느냐에 따라 결과 값이 달라질 수 있는 상황** 을 말한다. 공유 데이터의 동시 접근(Concurrent access)은 **데이터의 불일치 문제** 를 발생시킬 수 있다. 따라서, Race condition을 막고 일관성을 유지하기 위해서는 협렵 프로세스 간의 실행 순서를 정해주는 메커니즘인 **동기화(Synchronization)** 가 필요하다.

대표적으로 다음 세 경우에서 Race Condition이 발생할 수 있다.

1. **커널 모드로 수행 중 인터럽트가 발생하는 경우**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FOcqh6%2FbtrfG14dG6S%2FUi7Kd1NRYDM0t02UKKVffk%2Fimg.png)

의도된 동작은 count++과 count--가 모두 반영되어 count가 초기값을 유지하는 것이지만, 만약 Load를 한 후에 인터럽트가 발생하는 경우 결과는 반영되지 않고 count++만 반영된다. 이는 커널 모드의 수행이 끝나기 전에는 인터럽트를 받지 않도록 하는 방법(disable/enable)으로 문제를 해결할 수 있다.

<br/>

2. **프로세스가 시스템 콜을 호출해서 커널 모드로 수행 중인데 Context switch가 발생하는 경우**

두 프로세스의 주소 공간에서는 데이터를 공유하지 않지만, 시스템 콜을 수행하는 동안에는 둘 다 커널 주소 공간의 데이터를 접근한다. 따라서 커널 주소 공간에서 작업을 수행하는 도중에 CPU를 빼앗으면 race condition이 발생한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb2HzMP%2Fbtrfw33k7qh%2F2sNwIKMlaknzor4TdpWrKK%2Fimg.png)

이는 커널 모드로 수행중일 땐 CPU가 preempt(선점) 되지 않도록 하고, 커널 모드에서 유저 모드로 돌아갈 때 preempt 되도록 함으로써 해결할 수 있다.

<br/>

3. **멀티 프로세서에서 공유 메모리 내의 커널 데이터에 접근하는 경우

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDoBj9%2FbtrfJVoYJJ9%2FP63cPFZ42aI9bfpzcFsUfK%2Fimg.png)

어떤 CPU가 마지막으로 Count를 저장했는지에 따라 결괏값이 달라진다. 싱글 프로세서인 경우 1번에서와 같이 인터럽트 disable/enable 방법으로 해결할 수 있지만 멀티 프로세서인 경우 인터럽트 제어로는 해결할 수 없다. 한 번에 한 CPU만 커널에 들어갈 수 있도록 하는 방법이 있으나 이는 비효율적이다. 만약 두 프로세서가 서로 다른 데이터에 접근하여 Race condition의 가능성이 없음에도 불구하고 한 번에 하나의 CPU만 커널에 들어갈 수 있기 때문이다. 따라서, 커널 내부에 있는 각 공유 데이터에 접근할 때마다 그 데이터에 대해서만 lock/unlock을 하는 방식으로 해결할 수 있다.

<br/>

# Critical Section

Critical Section(임계 구역)은 **코드 상에서 Race condition이 발생할 수 있는 특정 부분** 을 말한다. 즉, 공유 데이터를 접근하는 코드 부분을 말한다. Cirtical Section으로 발생하는 문제들을 해결하기 위해서는 다음 조건들을 만족해야 한다.

1. **Mutual Exclusion(상호배제)**

이미 한 프로세스가 Critical Section에서 작업 중이면 다른 모든 프로세스는 Ciritical Section에 진입하면 안된다.

2. **Progress(진행)**

Critical Section에서 작업 중인 프로세스가 없다면, Critical Section에 진입하고자 하는 프로세스가 존재하는 경우 진입할 수 있어야 한다.

3. **Bounded Waiting(한정 대기)**

프로세스가 Critical Section에 들어가기 위해 요청한 후부터 그 요청이 허용될 때까지 다른 프로세스들이 Critical Section에 들어가는 횟수에 한계가 있어야 한다. 쉽게 말해, Critical Section에 진입하려는 프로세스가 무한정 기다려서는 안 된다.

<br/>

# Synchronization Algorithms

Critical Section Probolem을 해결하기 위한 알고리즘 들을 살펴보자. 두 프로세스 Pi와 Pj가 있고, 공유하는 공통 변수가 존재한다고 가정하자.

<br/>

## Algorithm 1

**현재 Critical Section에 들어갈 프로세스가 어떤 프로세스인지를 한 변수로 나타내어 일치하는 프로세스만 진입하도록** 하는 단순한 방식이다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FccOkMk%2FbtrfIZSCSAo%2F5hFSuPhxSQSZ5SPTAYmUw1%2Fimg.png)

이 방식은 **Mutual Exclusion은 만족하지만 Progress는 만족하지 못한다.** 예를 들어 Pi의 remainder section 수행 시간이 매우 길어서 여전히 remainder section에서 수행 중이고, Pj가 수행을 한번 마치고 다시 Critical Section으로 진입하려고 한다면 turn == i 이기 때문에 진입할 수 없다. 따라서 Critical Section에 아무런 프로세스가 존재하지 않게 된다.

<br/>

## Algorithm 2

다른 방식으로는, **특정 프로세스가 Critical Section에 진입할 준비가 되었다는 것을 나타내는 변수를 두어, 다른 프로세스가 Critical Section 진입하려고 한다면 현재 프로세스는 기다리는 방법** 이다. 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbZuZTX%2FbtrfA2ibaFz%2FmQE6klkPEIRwy1jbMEK9aK%2Fimg.png)

이 방식도 마찬가지로 **Mutual Exclusion은 만족하지만 Progress는 만족하지 못한다.** 두 프로세스가 flag = true를 수행하고 나면 두 프로세스 모두 무한히 Critical Section에 진입하지 못하고 기다리는 상황이 발생하게 된다.

<br/>

## Algorithm 3 (Peterson's Algorithm)

**Peterson's Algorithm은 이전의 알고리즘 1과 2를 합쳐놓은 개념** 이다. turn 변수와 flag 변수를 동시에 사용한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FE6DL6%2FbtrfIYe7NMU%2F6viNsjUvtec2D4oyMbpA0K%2Fimg.png)

Pi 프로세스에 대해서, Pi는 flag[i] = true로 바꾸면서 Critical Section에 진입하려고 한다. 그리고 turn = j로 바꿔주면서 상대방이 들어가게 한다. 만약 상대방이 들어가고 싶고 (flag[j] == true), 현재 상대방이 Critical Section에 들어가 있으면 (turn == j) 기다린다. 그렇지 않으면 Pi가 들어간다. 그리고 나서 Critical Section을 빠져나오면 들어가고 싶지 않다고 flag[i] = false로 바꿔준다.

이 방식으로는 **Mutual Exclusion, Progress, Bounded waiting 모두 만족한다.** 하지만, **Critical Section 진입을 기다리면서 계속 CPU와 메모리를 사용하는 Busy Waiting** 의 문제점이 있다.

이제, 이 복잡한 코드 구현들이 하드웨어 / 소프트웨어 측면에서 어떤 지원들이 제공되는지 알아보자.

<br/>

# Synchronization Hardware

하드웨어적으로 현재 상태를 확인하고 변경하는 Test & modify를 **atomic** 하게 수행할 수 있도록 지원하면 Critical Section Problem은 간단히 해결할 수 있다. 많은 시스템들은 이러한 하드웨어적인 지원을 제공한다. 현대 기계들은 특별한 atomic hardware instruction을 제공한다.(Test_and_Set 등)

이전까지의 알고리즘들은 데이터를 읽고 쓰는 것을 하나의 명령어로 처리할 수 없기 때문에 복잡해졌다. 만약 메모리에서 데이터를 읽으면서 쓰는 것까지 하나의 명령어로 동시에 수행이 가능하다면 코드가 훨씬 간결해지고, 이는 **Test_and_Set** 을 이용하여 아래와 같이 구현한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FF640X%2FbtrfG1XEkxV%2FWr1qKejoHzyb6JsHWFx8t0%2Fimg.png)

하지만 이러한 하드웨어적인 명령어는 **bounded waiting 조건을 만족하지 못한다는 단점** 이 있다. 이 문제를 해결하기 위해서 waiting array라는 배열을 추가로 사용할 수 있는데, 자세한 설명은 생략...

<br/>

# Mutex Locks

Critical Section Problem을 해결하기 위한 소프트웨어 도구 중 가장 단순한 방법으로 Mutex(Mutual Exclusion) locks이 있다. Mutex locks은 **lock이 하나만 존재할 수 있는 locking 메커니즘을** 따른다. 즉, **이미 하나의 프로세스가 Critical Section에서 작업 중이면 다른 프로세스들은 Critical Section에 들어갈 수 없도록 한다.** Lock을 걸고 푸는 동작은 하드웨어 방식처럼 atomic 하게 수행된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5CzqR%2FbtrfuOZSuBX%2FNf0pXCWqCz0K9fxfPd9jIK%2Fimg.png)

이러한 방식은 앞의 Peterson's Algorithm 설명에서도 언급했듯이, **Busy Waiting의 단점** 이 있다. Critical Section에 프로세스가 존재할 때, 다른 프로세스들은 Critical Section에 계속해서 진입하려고 시도하기 때문에 CPU를 낭비하게 된다.

이렇게 **lock이 반환될 때까지 계속 확인하면서 프로세스가 기다리는 것** 을 다른 용어로 **Spin Lock** 이라고 한다. **Spin Lock은 Critical Section에 진입을 위한 대기 시간이 짧을 때, 즉 Context Switching 하는 비용보다 기다리는 게 더 효율적인 특수한 상황을 위해 고안된 것이다.** 단일 CPU 시스템에서는 어차피 Lock을 갖고 있는 스레드를 풀어주기 위해서 Context Switching이 일어나야 하기 때문에 유용하지 않고, 멀티 프로세서 시스템에서 종종 사용된다.

<br/>

# Semaphores

세마포어(Semaphore)는 **Busy Waiting이 필요 없는 동기화 도구이며, 여러 프로세스나 스레드가 Critical Section에 진입할 수 있는 Locking 메커니즘이다.**

세마포어는 카운터(Counter)를 이용하여 동시에 자원에 접근할 수 있는 프로세스를 제한한다. 주로 S라는 정수형 변수로 나타내며, 이는 **사용 가능한 자원의 개수** 를 의미한다. 또 세마포어 변수는 오직 두 개의 atomic 한 연산을 통해서 접근할 수 있다. 한 프로세스가 세마포어 변수를 수정할 때 다른 프로세스가 동시에 같은 세마포어 변수를 수정할 수 없다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHaGmL%2Fbtrfw4uBC7k%2FXg7uRwvt2vnchNYn3kQscK%2Fimg.png)

**P(S)는 공유 데이터를 획득하는 연산이고, V(S)는 반납하는 연산이다.**

세마포어에는 두 종류가 있다.

1. **Counting Semaphore** : 정수 값의 범위가 0 이상으로 제한이 없다. 주로 자원의 개수를 세는 데 사용한다.
2. **Binary Semaphore** : 정수 값이 오직 0 또는 1이다. Mutex lock과 동일한 역할을 갖는다.(Mutex lock과의 차이점)

세마포어는 다음과 같이 사용된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FACTmA%2Fbtrfw4VHTxS%2Fdfhm5JSdnGxIKLBSZ9d2S0%2Fimg.png)

하지만, 이 방식은 Busy Waiting이 발생하므로 비효율적이다. 따라서 **Block & Wakeup** 방식을 사용한다. Block & Wakeup 방식은 **Critical Section으로의 진입에 실패한 프로세스를 기다리게 하지 않고 Block 시킨 뒤 Critical Section에 자리가 나면 다시 깨워줌으로써** Busy waiting에서의 CPU 낭비 문제를 해결해준다. 일반적으로 Busy Waiting이 비효율적이지만, Critical Section이 매우 짧은 경우 Block & Wakeup의 오버헤드가 더 커질 수도 있다.

Block & Wakeup 구현을 위해 세마포어를 다음과 같이 정의한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlJWDM%2FbtrfHcSeRb2%2F9kbSE0DzDZyYTdOIxOZb5k%2Fimg.png)

value는 **세마포어 변수** 를 의미하고, L은 **block 된 프로세스들이 기다리는 Queue** 다. 따라서 wait와 signal 함수도 아래와 같이 구현된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FofmlK%2FbtrfzkjHMj2%2FO9g4jYIoo8cXPHtT7o2jIk%2Fimg.png)

**Block**을 수행하면, 커널은 block을 호출한 프로세스를 suspend 시키고 해당 프로세스의 PCB를 wait queue에 넣어준다. **Wakeup** 을 수행하면, block 된 프로세스 P를 꺠운 다음, 이 프로세스의 PCB를 Ready Queue로 이동시킨다. signal 함수에서 S.value가 0 '이하'인 이유는, 이전에 S.value++를 통해 자원을 내놓았는데 아직 0 이하라는 의미가 기다리고 있는 프로세스가 존재한다는 의미이기 때문이다.

wait나 signal 함수는 같은 세마포어에 대해 두 프로세스가 동시에 실행할 수 없으며, 만약 wait나 signal을 수행하는 도중에 preempt 된다면 Critical Section Problem에 빠지게 된다.

<br/>

# Classical Problems of Synchronization

고전적인 동기화 문제로 유명한 세 문제가 있다.

## Producer-Consumer Problem (Bounded-Buffer Problem)

생산자 스레드들과 소비자 스레드들이 있고, 사이에 공유 버퍼가 있다고 가정하자. 만약 둘 이상의 생산자가 비어있는 버퍼들을 동시에 보고 데이터를 만들어 넣는다면 문제가 발생할 수 있다. 마찬가지로 둘 이상의 소비자가 동시에 동일한 버퍼의 데이터를 사용한다면 문제가 발생할 수 있다. 따라서, **동시에 버퍼에 접근할 수 없도록 락을 걸어줘야 한다.** 또, 버퍼가 꽉 찬 상태에서 생산자는 반드시 소비자가 버퍼의 데이터를 사용하기를 기다려야 하고, 버퍼가 빈 상태에서 소비자는 생산자가 버퍼를 채우기를 기다려야 한다. 그러므로 **락을 걸고 푸는 용도, 자원의 개수를 카운팅 하는 용도로 세마포어 변수를 사용한다.**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbNMSzN%2FbtrfO5rC7Ef%2FFnEXbksNpMEsKgm2t2k8r1%2Fimg.png)

<br/>

## Readers-Writers Problem

Readers-Writers Problem은 **한 프로세스가 데이터를 수정하는 작업을 수행할 때 다른 프로세스가 접근하면 안 되고, 읽는 작업은 여러 프로세스가 동시에 수행 가능하도록 하는 문제** 이다. 만약 데이터에 대해 **하나의 lock을 사용하게 되면 데이터의 일관성은 지킬 수 있으나**, 여러 프로세스가 동시에 읽을 수 있음에도 불구하고 한 프로세스만 읽도록 강제하는 것이므로 **비효율적** 이다. 따라서 현재 수정하려고 하는 프로세스가 없다면 모든 대기 중인 Reader들의 접근을 허용하고, Writer는 대기 중인 Reader가 하나도 없는 경우 접근하도록 한다. 그리고 Writer가 접근 중이면 Reader들은 접근할 수 없도록 한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FXEUsx%2FbtrfG23RUyd%2FZzAeD0dJ3gvOMtQ3ijHkF0%2Fimg.png)

만약 n개의 reader가 기다리고 있다면, 제일 처음 reader만 wrt 세마포어에 넣고 나머지 n-1 개의 reader는 mutex 세마포어 큐에 넣어둠으로써 효율성을 높여준다. Readers-Writers Problem의 해결책은 **Starvation** 의 가능성이 있다. 계속해서 reader가 들어오거나 writer가 들어오는 경우 한쪽이 계속 수행되지 않을 수 있다. 이는 큐에 우선순위를 부여한다거나, 일정 시간이 지나면 자동으로 write or read가 되도록 하여 해결할 수 있다.

<br/>

## Dining-Philosophers Problem

5명의 철학자가 원탁에 둘러앉아있고, 젓가락도 5개가 놓여있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FblNMHL%2FbtrfPBRLIGR%2Fz7j0L5k1T0iHXyRROolCE0%2Fimg.png)

각 철학자는 식사를 하기를 원하고, 철학자가 식사를 하기 위해서는 양쪽 젓가락을 모두 집어야 한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FS8mIV%2FbtrfEmhca3P%2Fkd5UmbPyiDHh5p6kg75yvk%2Fimg.png)

위와 같이 구현하면 이웃한 두 철학자가 동시에 먹는 경우가 없다는 것이 보장된다. 하지만 이 방식은 만약 모든 철학자가 식사를 하기를 원해서 각자 왼쪽에 있는 젓가락을 들었다면, 더 이상 아무런 작업도 수행할 수 없는 Deadlock 상태에 빠져버리게 된다. Deadlock 상태를 방지하기 위한 여러 방법이 있는데, 4명의 철학자만이 테이블에 동시에 앉도록 하거나, 젓가락을 두 개 집을 수 있을 때만 젓가락을 집도록 하는 방법, 또는 짝수/홀수 철학자는 왼쪽/오른족 젓가락을 먼저 집도록 하는 방법 등이 있다.

ex) 각 철학자가 양쪽 젓가락을 다 들 수 있을 때만 들도록 하는 방법

V(self[i]) : 양쪽 젓가락을 들 수 있는 권한을 얻는 부분. 만약 얻지 못하면 P(self[i])에서 기다리게 된다. 양쪽 철학자가 밥을 다 먹은 경우에 기다림이 끝난다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb2j3XK%2FbtrfIYHfMsA%2FIsk9sG0IFykKcEVt2RkCO1%2Fimg.png)

<br/>

# Monitor

이전의 세마포어의 문제점은 **코딩하기가 힘들고 실수하기 쉬우며, 정확성을 입증하기가 어렵다.** V(mutex)와 P(mutex)의 순서에 따라 Deadlock이 생기거나 Mutual Exclusion이 깨질 수 있기 때문이다. 이러한 단점을 보완할 수 있는 구조로 Monitor가 있다.

Monitor(모니터)는 **동시 수행 중인 프로세스 사이에서 추상 데이터의 안전한 공유를 보장하기 위한 High-level 동기화 구조이다.** 공유 데이터를 접근하기 위해서는 **모니터의 내부 Procedure** 를 통해서만 접근할 수 있도록 한다.

세마포어와의 가장 큰 차이점은, 모니터는 락(Lock)을 걸 필요가 없다. 세마포어는 프로그래머가 직접 wait와 signal을 사용하여 Race condition을 해결해야 하지만 모니터는 자체적인 기능으로 해결할 수 있게 된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F96ZQY%2FbtrfQDV7EUc%2FSKcuDclXPoOikCIcKnoL1k%2Fimg.png)

위 그림은 Race condition이 발생할 수 있는 경우의 예시이다. P1이 B와 C를 사용하고 있을 때 P2가 C에 접근한다면 Race condition이 발생할 수 있다. 우리는 이를 방지하기 위해 여러 알고리즘이나 세마포어 같은 도구들을 배웠고, 이를 사용하면 다음과 같이 작용한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFT3HS%2FbtrfPBEvYjy%2FnCNAEpsW1t8mkT2uAoU4L1%2Fimg.png)

반면, 모니터는 아래와 같이 이루어져 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F6qPcA%2FbtrfFEJygqD%2F50pOIqkm5jUEdNEtRCB5U0%2Fimg.png)

**모니터는 공유 데이터 구조, 공유 데이터에 대한 연산을 제공하는 프로시저(Procedure), 현재 호출된 프로시저간의 동기화를 캡슐화한 모듈(module)이다.** 프로세스가 공유 데이터를 사용하기 위해서는 반드시 모니터 내의 Procedure를 통해야 한다. 그리고 동일한 시간엔 오직 한 프로세스나 스레드만 모니터에 들어갈 수 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbc7InW%2FbtrfG1YP29i%2FNeSIwXt7VYXBzY6oOkxDyK%2Fimg.png)

모니터에 진입하려고 했지만 내부에 프로세스가 들어가 있어 진입하지 못한 프로세스들은 **모니터 큐(Monitor Queue)** 에서 기다린다. 프로세스가 모니터 내에서 기다릴 수 있게 하도록 **조건 변수(Condition variable)** 가 사용된다.

조건 변수는 오직 wait와 signal 연산만으로 사용될 수 있다. 세마포어 변수와 유사하지만 **변수가 어떤 값을 가지진 않고, 자신의 큐에 프로세스를 매달아서 sleep 시키거나 큐에서 프로세스를 깨우는 역할** 만 한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlbD8w%2FbtrfPWPhkpl%2F01QJJpkRcSt9QaHyYErD7k%2Fimg.png)

<br/>

**참조**
* [원본링크](https://rebro.kr/176?category=504670)