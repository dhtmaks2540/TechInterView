# 데드락(Deadlock)

Deadlock(데드락)이란, **일련의 프로세스들이 서로가 가진 자원을 기다리며 block 되어 더 이상 진행이 될 수 없는 상태** 를 말한다. '교착상태' 라고도 부르며, 시스템적으로 한정된 자원을 여러 곳에서 사용하려고 할 때 발생한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsaxZo%2FbtrfRDcGGIW%2FggIpU9AjbKiZAj0rdqFGNK%2Fimg.png)

예를 들어 이처럼 각 차가 모두 앞에 있는 차가 지나가기를 기다리는 상황이 되어 모든 차가 더 이상 움직일 수 없는 상태를 말한다.

세마포어를 예로 들어보자. 세마포어 변수 A, B가 있고 프로세스 P1이 P(A), P(B)을 순서대로 호출하고 P2가 P(B), P(A)를 호출한다고 가정하면, P1과 P2가 서로 각각 B와 A를 영원히 가디리는 상태가 되어버린다. 이런 상태가 Deadlock이다.

프로세스가 자원을 사용하는 절차에는 Request, Allocate, Use, Release가 있다.

* **Request** : 자원을 요청하고, 만약 다른 프로세스가 자원을 사용하고 있어 받을 수 없다면 대기한다.
* **Allocate** : 자원을 받는다.
* **Use** : 프로세스가 받은 자원을 사용한다.
* **Release** : 프로세스가 자원을 놓아준다.

Deadlock은 모든 프로세스가 Request 상태가 되어있는 상황이다.

<br/>

## Deadlock Characterization

Deadlock이 발생하기 위해선 다음 4가지 조건을 만족해야 한다.

1. **Mutual exlusion (상호 배제)** : 매 순간 하나의 프로세스만이 자원을 사용할 수 있다.
2. **Hold and wait (보유 대기)** : 자원을 가진 프로세스가 다른 자원을 기다릴 때, 보유하고 있는 자원을 놓지 않고 계속 가지고 있다.
3. **No preemption (비선점)** : 프로세스는 OS에 의해 강제로 자우너을 빼앗기지 않는다.
4. **Circular wait (순환 대기)** : 자원을 기다리고 있는 프로세스 간에 사이클이 형성되어야 한다.(ex. 프로세스 p0, p1, ... pn이 있을 때 p0은 p1을 기다리고 p1은 p2를 기다리고, ... , pn은 p0을 기다린다.)

프로세스 간의 관계를 그래프로 도식화해 보면 데드락이 발생할지 예상할 수 있다. 이 그래프를 **Resource-Allocation Graph(자원 할당 그래프)** 라고 한다. 예시를 하나 살펴보자.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FH2XYm%2FbtrfWL1ukSu%2FTzA4phzq2YxvkVEKWQrUu0%2Fimg.png)

R은 자원이고 P는 프로세스를 의미한다. 자원 내의 동그라미는 자원(인스턴스)의 개수이다. **자원 -> 프로세스** 로 향하는 간선은 해당 자원을 프로세스가 보유 중(Allocate)이라는 의미이고, **프로세스 -> 자원** 으로 향하는 간선은 프로세스가 해당 자원을 요청(Request)했다는 의미이다.

만약 **그래프에 사이클(Cycle)이 없다면 Deadlock이 아니다.** 반면, **사이클이 있다면 Deadlock이 발생할 '수' 있다.** 정확히 말하자면, 자원당 하나의 인스턴스만 있는 경우엔 Deadlock이고, 여러 인스턴스가 존재하는 경우엔 Deadlock일 수도 있고 아닐 수도 있다. 위의 그림에서는 왼쪽 그래프는 Deadlock이지만, 오른쪽 그래프는 Deadlock이 아니다.

Deadlock 문제를 해결하기 위한 방법으로는 대표적으로 미리 예방하는 방법(Prevention)과 Deadlock이 발생하지 않도록 피하는 방법(Avoidance), 그리고 발생했을 때 처리하는 방법(Detection and Recovery), 무시하는 방법(Ignorance) 총 네 방법으로 나뉜다.

<br/>

## 데드락 문제 해결

### Deadlock Prevention

미리 예방하는 방식인 Deadlock Prevention은 **자원을 할당할 때 Deadlock의 4가지 필요조건 중 어느 하나가 만족되지 않도록 하는 방식** 이다.

1. **Mutual Exclusion(상호 배제)** : Critical Section Problem을 해결하기 위해서는 이 조건은 반드시 만족해야 하므로 공유자원이 존재한다면 이 조건은 만족시킬 수 밖에 없다.

2. **Hold and Wait(점유 대기)** : 프로세스가 자원을 요청할 때 어떤 자원도 가지고 있지 않도록 해야 한다. 따라서 프로세스를 시작할 때 모든 필요한 자원을 할당받게 하거나, 자원이 필요한 경우 보유하고 있던 자원을 모두 반납하고 다시 요청하는 방식을 이용할 수 있다.

3. **No preemption(비선점)** : 프로세스가 어떤 자원을 기다려야 하는 경우 보유하고 있던 자원이 선점된다. 그리고 모든 필요한 자원을 얻을 수 있을 때 그 프로세스는 다시 시작된다.

4. **Circular wait(순환 대기)** : 자원의 타입에 따라 프로세스마다 할당 순서를 정하여 정해진 순서대로만 자원을 할당한다.

하지만 이렇게 미리 Deadlock을 방지하는 방식은 **효율성과 처리량을 감소시키고, Starvation이 발생할 수 있다.**

<br/>

### Deadlock Avoidance

Deadlock Avoidance는 **Deadlock이 발생할 가능성이 있는 경우엔 아예 자원을 할당하지 않는 방식이다.** 가장 단순하고 일반적인 모델은 **프로세스들이 필요로 하는 각 자원별 최대 사용량을 미리 선언하도록 하는 방법** 이다.

먼저 다음 두 용어를 정의하자.

**Safe Sequence** : 프로세스의 sequence<P1, P2, ..., Pn>이 있을때, Pi의 자원 요청이 '가용자원 + 모든 Pj(j < i)의 보유 자원'에 의해 충족되는 경우 sequence를 safe 하다고 말한다.

**Safe state** : 시스템 내의 프로세스들에 대한 Safe Sequence가 존재하는 상태

만약 시스템이 Safe state에 있으면 Deadlock이 발생하지 않는다. 하지만 Unsafe state에 있으면 Deadlock이 발생할 수 있다. 따라서, Deadlock Avoidance는 **시스템이 Unsafe state에 들어가지 않는 것을 보장하는 것** 이다.

두 경우의 Avoidance 알고리즘이 있는데, 각 자원 타입마다 하나의 인스턴스가 존재하는 경우 자원 할당 그래프 알고리즘을 사용하고, 여러 인스턴스가 존재하는 경우 Banker's 알고리즘을 사용한다.

<br/>

1. **Resource Allocation Graph Algorithm(자원 할당 그래프 알고리즘)**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcMCznT%2FbtrfVqJ7pZW%2FtE8jDSofBmXCkTyKFRh5G0%2Fimg.png)

점선으로 표시된 간선(Claim edge)은 프로세스가 자원을 미래에 요청할 수 있음을 의미한다. 그리고 해당 자원을 요청하는 경우 실선(Request edge)으로 바뀌게 된다. 자원을 할당받으면 방향이 반대인 간선(Assignment edge)이 된다. 만약 자원을 다 쓰고 반납하게 되면 다시 Claim edge로 바뀐다.

Deadlock을 피하는 방법은, **Request edge가 Assignment edge로 변경될 때 점선을 포함하여 사이클이 생기지 않는 경우에만 요청된 자원을 할당한다.**

<br/>

2. **Banker's Algorithm**

여러 인스턴스가 존재하는 경우엔 사이클만으로 판단할 수는 없다. Banker's Algorithm은 dijkstra가 고안한 알고리즘이며, 이는 프로세스가 자원을 요청할 때마다 수행된다. 

<가정>
* 모든 프로세스는 자원의 최대 사용량을 미리 명시한다.
* 프로세스가 요청 자원을 모두 할당받은 경우 유한 시간 안에 자원들을 다시 반납한다.

기본적인 개념은, 자원을 요청할 때 safe 상태를 유지하는 경우에만 할당한다. 즉, **총 요청 자원의 수가 남은 자원의 수보다 적은 프로세스만 선택하여 수행한다.** 만약 그런 프로세스가 없다면 unsafe 상태인 것이다. 할당받은 프로세스가 종료되면 모든 자원을 반납하고, 모든 프로세스가 종료될 때까지 이 과정을 반복한다.

<br/>

### Deadlock Detection and Recovery

Deadlock이 발생했는지는 Deadlock을 미리 회피하는 방법과 유사하게 확인할 수 있다. 그래프를 통해서 사이클이 생겼는지를 확인하거나, 총 요청 자원의 수가 남은 자원의 수보다 적은 프로세스가 존재하지 않는다는 것을 이용하여 확인할 수 있다.

그렇다면 Deadlock을 해결하기 위해 어떤 프로세스를 종료시켜야 할까? 여기에는 몇 가지 판단 기준이 있다.

* 프로세스의 중요도
* 프로세스가 얼마나 오래 실행됐는가
* 얼마나 많은 자원을 사용했는가
* 프로세스가 작업을 마치기 위해 얼마나 많은 자원이 필요한가
* 프로세스가 종료되기 위해 얼마나 많은 자원이 필요한가
* 프로세스가 batch인가 interactive인가

해결하는 방법으로는 프로세스를 종료시키거나 자원을 선점하는 방법이 있다. 프로세스를 종료시킬 땐 Deadlock에 빠진 모든 프로세스를 종료하거나, Deadlock이 해결될 때까지 한 번에 한 프로세스씩 종료시킬 수 있다.

자원을 선점할 땐 어떤 프로세스를 종료시킬지 결정(Selecting a victim)하고, Deadlock이 발생하기 전 상태로 돌아가(Rollback) 프로세스를 재시작한다. 이때 동일한 프로세스가 계속해서 victim으로 선정되는 경우 Starvation이 발생할 수도 있다. 이는 Rollback 된 횟수를 저장함으로써 해결할 수 있다.

<br/>

### Deadlock Ignorance

**Deadlock이 일어나지 않는다고 생각하고 아무런 조치도 취하지 않는 방식이다.** 그 이유는, Deadlock이 매우 드물게 발생하기 때문에 Deadlock에 대한 조치 자체가 더 큰 오버헤드일 수 있기 때문이다. 따라서 만약 시스템에 Deadlock이 발생한 경우, 시스템이 비정상적으로 작동하는 것을 사람이 느낀 후 직접 프로세스를 죽이는 등의 방법으로 대춰한다. 이 방식은 **UNIX, Windows 등 대부분 범용 운영체제가 채택하는 방식이다.**


<br/>

## 주요 질문
1. 데드락(교착 상태)가 뭔가요? 발생 조건에 대해 말해보세요.

2. 회피 기법인 은행원 알고리즘이 뭔지 설명해보세요.

3. 기아상태를 설명하는 식사하는 철학자 문제에 대해 설명해보세요.
<br/>
교착 상태 해결책
   1. n명이 앉을 수 있는 테이블에서 철학자를 n-1명만 앉힌다.
   2. 한 철학자가 젓가락 두개를 모두 집을 수 있는 상황에서만 젓가락을 집도록 허용한다.
   3. 누군자는 왼쪽 젓가락을 먼저 집지 않고 오른쪽 젓가락을 먼저 집도록 허용한다.

<br/>

**[참조]**
* [원본링크](https://rebro.kr/177?category=504670)