# CPU Scheduling

CPU가 하나의 프로세스 작업이 끝나면 다음 프로세스 작업을 수행해야 한다. 이때 어떤 프로세스를 다음에 처리할 지 선택하는 알고리즘을 CPU Scheduling 알고리즘이라고 한다. 따라서 상황에 맞게 CPU를 어떤 프로세스에 배정하여 효율적으로 처라하는가가 관건이다.

* 조건 : 오버헤드 ↓ / 사용률 ↑ / 기아 현상 ↓

* 목표
   1. `Batch System` : 가능하면 많은 일을 수행, 시간(time)보단 처리량(throughout)이 중요
   2. `Interactive System` : 빠른 응답 시간, 적은 대기 시간
   3. `Real-time System` : 기한(deadline) 맞추기

<br/>

## 프로세스 상태

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FX3xNu%2Fbtrec3xnZhm%2F3B6NJSOKC6PCodWbdD42tk%2Fimg.png)

1. New : 프로세스가 처음 생성된 상태
2. Ready : 프로세스가 CPU에 할당되기를 기다리는 상태(메모리 등 다른 조건을 모두 만족하고)
3. Running : 프로세스가 할당되어 CPU를 잡고 명령을 수행 중인 상태
4. Waiting : 프로세스가 어떠한 이벤트가 발생하기를 기다리는 상태, CPU를 할당해도 당장 명령을 수행할 수 없는 상태
5. Terminated : 프로세스가 실행을 마쳤을 때, 아직 완전히 프로세스가 제거되진 않은 상태

* 비선점 스케줄링 : `Interrupt`, `Scheduler Dispatch`
* 선점 스케줄링:  `I/O or Event Wait`

<br/>

### 프로세스 상태 전이

* 승인(Admitted): 프로세스 생성이 가능하여 승인됨

* 스케줄러 디스패치(Scheduler Dispatch): 준비 상태에 있는 프로세스 중 하나를 선택하여 실행시키는 것

* 인터럽트(Interrupt): 예외, 입출력, 이벤트 등이 발생하여 현재 실행 중인 프로세스를 준비 상태로 바꾸고, 해당 작업을 먼저 처리하는 것

* 입출력 또는 이벤트 대기(I/O or Event wait): 실행 중인 프로세스가 입출력이나 이벤트를 처리해야 하는 경우, 입출력/이벤트가 모두 끝날 떄까지 대기 상태로 만드는 것

* 입출력 또는 이벤트 완료(I/O or Event Completion): 입출력/이벤트가 끝난 프로세스를 준비 상태로 전환하여 스케줄러에 의해 선택될 수 있도록 만드는 것

<br/>

## Preemptive vs Non-Preemptive

1. Preemptive(선점)
   * 프로세스가 CPU를 점유하고 있는 동안 I/O나 인터럽트가 발생하지 않았음에도 다른 프로세스가 해당 CPU를 강제로 점유할 수 있다.
   * 즉, 프로세스가 정상적으로 수행중인 동안 다른 프로세스가 CPU를 강제로 점유하여 실행할 수 있다.

2. Non-Preemptive(비선점)
   * 한 프로세스가 CPU를 점유했다면 I/O나 인터럽트 발생 또는 프로세스가 종료될 때까지 다른 프로세스가 CPU를 점유하지 못하는 것이다.

<br/>

## 선점형 스케줄링

1. SRTF(Shortest Remaining Time First) 스케줄링

* 짧은 시간 순서대로 프로세스를 수행한다.
* 현재 CPU에서 실행 중인 프로세스의 남은 CPU 버스트 시간보다 더 짧은 CPU 버스트 시간을 가지는 프로세스가 도착하면 CPU가 선점된다.

<br/>

2. Round Robin 스케줄링

* 특징
   * 현대적인 CPU 스케줄링
   * 시분할 시스템의 성질을 활용한 방법이다.
   * 할당 시간(time quantum = time slice)을 정해서 이 시간이 지나면 프로세스는 선점당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다.
   * `RR`은 CPU 사용시간이 랜덤한 프로세스들이 섞여있을 경우에 효율적이다.
   * `RR`이 가능한 이유는 프로세스의 context 를 save 할 수 있기 때문이다.

* 장점
   * `Response time` 이 빨라진다. n 개의 프로세스가 ready queue에 있고 할당시간이 q(time quantum)인 경우 각 프로세스는 q 단위로 CPU 시간의 1/n을 얻는다. 즉, 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.
   * 프로세스가 기다리는 시간이 CPU를 사용할 만큼 증가한다. 따라서 공정한 스케줄링이라고 할 수 있다.

* 주의할 점
   * 설정한 `time quantum`이 너무 커지면 `FCFS`와 같아진다. 또 너무 작아지면 스케줄링 알고리즘의 목적에는 이상적이지만 잦은 context switch로 overhead 가 발생한다. 그렇기 때문에 적당한 `time quantum`을 설정하는 것이 중요하다.

<br/>

3. Multi-level Queue 스케줄링

   * 프로세스를 그룹으로 나누어, 각 그룹에 따라 Ready Queue(준비 큐)를 여러개 두며, 각 큐마다 다른 규칙을 지정할 수도 있다(Ex. 우선순위, CPU 시간 등).
   * 즉, 준비 큐를 여러 개로 분할해 관리하는 스케줄링 방법이다.
   * 프로세스들이 CPU를 기다리기 위해 한 줄로 서는게 아니라 여러 줄로 선다.
   * 다만, 단점으로 기아현상이 발생할 수 있다.

![](https://user-images.githubusercontent.com/34755287/53879673-5e979880-4052-11e9-9f9b-e8bfec7c9be6.png)

<br/>

4. Multi-level feedback Queue 스케줄링
   * 기본 개념은 Multi-level Queue와 동일하나, 프로세스가 하나의 큐에서 다른 큐로 이동 가능하다는 점이 다르다.

![](https://user-images.githubusercontent.com/34755287/53879675-5f302f00-4052-11e9-86a2-c02ee03bac64.png)

   * 위 그림에서 모든 프로세스는 가장 위의 큐에서 CPU의 점유를 대기한다. 이 상태로 진행하다가 이 큐에서 기다리는 시간이 너무 오래 걸린다면 **아래의 큐로 프로세스를 옮긴다.** 이와 같은 방식으로 대기 시간을 조정할 수 있다.

   * 만약, 우선순위 순으로 큐를 사용하는 상황에서 우선순위가 낮은 큐에 있는 프로세스에서 starvation 상태가 발생하면 이를 우선순위가 높은 큐로 옮길 수도 있다. 즉, 에이징 기법을 통해 Multi-level Queue에서 발생한 기아 현상을 해결할 수 있다.

   * 대부분 상용 운영체제는 여러 개의 큐를 사용하고 각 큐마다 다른 스케줄링 방식을 사용한다. 프로세스의 성격에 맞는 스케줄링 방식을 사용하여 최대한 효율을 높일 수 있는 방법을 선택한다.

<br/>

### 비선점형 스케줄링

1. FCFS(First Come First Served)

* 특징
   * 준비 큐에 먼저 도착한 프로세스가 먼저 CPU를 점유하는 방식이다.
   * CPU를 할당받으면 CPU 버스트가 완료될 떄까지 CPU를 반환하지 않으며, 할당되었던 CPU가 반환될 때만 스케줄링이 이루어진다.

<br/>

#### FCFS 예시

![](https://user-images.githubusercontent.com/33534771/89703489-500b8a00-d986-11ea-9b6c-27cca4ea1016.png)

![](https://user-images.githubusercontent.com/34755287/53879661-5d666b80-4052-11e9-8453-bad918a563ef.png)

표는 3개의 프로세스와 각 프로세스가 CPU를 사용하는 시간(Busrt Time)을 나타낸다.

이를 간트 차트로 표현하면 그림과 같다(도착 시간은 모두 0초라고 가정한다).

평균 대기 시간은 아래와 같다.

* Average Waiting Time = (0 + 24 + 27) / 3 = 17msec

만약, 프로세스가 들어온 순서가 P3, P2, P1이라면 간트 차트는 아래와 같이 바뀐다.

![](https://user-images.githubusercontent.com/34755287/53879665-5d666b80-4052-11e9-8ad5-8639b73b13ac.png)

* Average Waiting Time = (3 + 6 + 0) / 3 = 3msec

두 경우에서 모든 프로세스가 끝난 시간은 30msec로 같지만, 평균 대기 시간으로 봤을 때는 위의 예제는 17msec이고 아래는 3msec로 차이가 난다. **즉, 들어온 순서로 수행한다고 해서 반드시 효율적인 것은 아니다.**

위의 예제처럼 `P1, P2, P3` 순서로 등어온 경우를 **Convoy Effect**라고 한다.

이는 CPU 시간을 오래 사용하는 프로세스가 먼저 수행되는 동안 나머지 프로세스들은 그만큼 오래 기다리는 것을 뜻한다. P1이 수행되는 동안 P2, P3는 오래 기다리게 된다.

#### 단점

* Convoy Effect 발생 : 소요 시간이 긴 프로세스가 짧은 프로세스보다 먼저 도착해서 뒤에 프로세스들이 오래 기다려야 하는 현상

<br/>

2. SJF(Shortest-Job-First)

* 특징
   * 다른 프로세스보다 먼저 도착했더라도 CPU 버스트가 짧은 프로세스에게 CPU를 먼저 할당하는 방식이다.
   * 항상 주어진 프로세스에 대해 **최소의 평균 대기 시간을 보장한다**. 즉, 항상 최적(Optimal) 임이 보장된다.
   * 선점, 비선점 모두 가능하다. 선점형을 SRTF, 비선점을 SJF라고 부른다.

<br/>

#### SJF 예시

![](https://user-images.githubusercontent.com/33534771/89703540-b7293e80-d986-11ea-9b11-0dabd8e5488f.png)

![](https://user-images.githubusercontent.com/34755287/53879666-5d666b80-4052-11e9-93c2-86b725588403.png)

위의 간트 차트는 SJF를 사용했고, 평균 대기 시간은 아래와 같다.

* Average Waiting Time(AWT) = (3 + 9 + 16 + 0) / 4 = 7msec

이번에는 위의 표를 FCSF를 사용해 간트차트로 나타내고 평균 대기 시간을 구해보겠다.

![](https://user-images.githubusercontent.com/34755287/53879667-5d666b80-4052-11e9-8cd4-066aefcf3047.png)

* Average Waiting Time(AWT) = (0 + 6 + 14 + 21) / 4 = 10.25msec

SJF가 평균 대기 시간이 더 짧다. 수학적으로 증명되었으며, 어떠한 예제를 보더라도 SJF의 AWT가 짧다는 것을 알 수 있을 것이다.

#### 단점

* starvation : 효율성을 추구하는게 가장 중요하지만 특정 프로세스가 지나치게 차별받으면 안되는 것이다. 이 스케줄링은 극단적으로 CPU 사용이 짧은 Job을 선호한다. 그래서 사용 시간이 긴 프로세스는 거의 영원히 CPU를 할당받을 수 없다. 

* SJF가 가장 효율적인 CPU 스케줄링 방법 같지만, 매우 **비현실적**이다. 왜냐하면 컴퓨터 환경에서는 프로세스의 CPU 점유 시간(Burst time)을 알 수 없다. 한 프로세스가 실행 중에는 많은 변수가 존재하기 때문에 CPU 점유 시간을 알려면 실제로 수행하여 측정하는 수밖에 없다. 실제 측정한 시간으로 예측하여 SJF를 사용할 수도 있지만, 이는 오버헤드가 매우 큰 작업으로 잘 사용되지 않는다.

<br/>

3. Priority Scheduling

* 특징
   * 우선순위가 높은 프로세스가 먼저 선택되는 스케줄링 알고리즘이다.
   * 우선순위는 정수값으로 나타내면, 작은 값이 우선순위가 높다(Unix/Linux 기준).
   * 선점형 스케줄링 방식
      * 더 높은 우선순위의 프로세스가 도착하면 실행중인 프로세스를 멈추고 CPU를 선점
   * 비선점형 스케줄링 방식
      * 더 높은 우선순위이 프로세스가 도착하면 Ready Queue의 Head 에 넣는다.

<br/>

#### Priority 예시

![](https://user-images.githubusercontent.com/33534771/89703556-e344bf80-d986-11ea-87f1-74994cc47510.png)

![](https://user-images.githubusercontent.com/34755287/53879671-5e979880-4052-11e9-84d3-524270cdc920.png)

우선 순위가 낮은 순서대로 수행한 모습을 간트 차트로 나타냈다.

* Average Waiting Time(AWT): (0 + 6 + 16 + 18 + 1) / 5 = 8.2msec

우선순위를 정하는 방법은 크게 내부적인 요소와 외부적인 요소로 나뉜다.

* Internal : time limit, memory requirement, I/O to CPU burst(I/O 작업은 길고, CPU 작업은 짧은 프로세스 우선 등)

* External : amount of funds being paid, political factors 등

<br/>

#### 단점

* Starvation(기아) 현상
* 무기한 봉쇄(Indefinite blocking) : 실행 준비는 되어있으나 CPU를 사용못하는 프로세스를 CPU가 무기한 대기하는 상태

기아현상은 CPU의 점유를 오랜 시간 동안 하지 못하는 현상을 의미한다. Priority 스케줄링 방식에서 우선순위가 매우 낮은 프로세스가 ready queue에서 대기하고 있다고 가정해보자.

이 프로세스는 아무리 기다려도 CPU를 점유하지 못할 가능성이 매우 크다. 실제 컴퓨터 환경에서는 새로운 프로세스가 자주 ready queue에 들어온다. 이러한 프로세스가 모두 우선순위가 높은 상태라면 이미 기다리고 있던 우선순위가 낮은 프로세스는 하염없이 기다리고만 있는 상태로 남아있을 수 있다.

이를 해결하는 방법 중 하나는 **aging**이 있다. ready queue에서 기다리는 동안 일정 시간이 지나면 우선순위를 일정량 높여주는 것이다. 그러면 우선순위가 매우 낮은 프로세스라 하더라도, 기다리는 시간이 길어질수록 우선순위도 계속 높아지므로 수행될 가능성이 커진다.

<br/>

## CPU 스케줄링 척도

CPU 스케줄링 알고리즘은 여러 종류가 있는데, 각 알고리즘의 성능을 평가하는 기준(Performance measure, 성능 척도)이 있다. 

1. 시스템 입장에서의 성능 척도
   * CPU 이용률 (CPU Utilization) : 전체 시간 중 CPU가 쉬지 않고 일한 시간
   * 처리량 (Throughput) : 단위 시간당 수행 완료한 프로세스의 수

2. 프로그램 입장에서의 성능 척도
   * 소요 시간 (Turnaround Time) : 프로세스가 Ready queue에서 대기한 시간부터 작업을 완료하는데 걸리는 시간
   * 대기 시간 (Waiting Time) : 프로세스가 Ready queue에서 대기한 시간
   * 응답 시간 (Response Time) : 프로세스가 처음으로 CPU를 할당받기까지 걸린 시간

<br/>

**[참조]**
* [원본링크1](https://gyoogle.dev/blog/computer-science/operating-system/CPU%20Scheduling.html)
* [원본링크2](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Operating%20System/CPU%20%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81.md)
* [프로세스 스케줄링](https://rebro.kr/175?category=504670)