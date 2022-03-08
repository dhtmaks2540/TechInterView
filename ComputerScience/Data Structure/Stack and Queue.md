# Stack and Queue

스택과 큐는 선형 자료구조이다.

* 선형 : 선형적(linear)이란, **직선(line) 모양인** 을 뜻한다. 예를 들어 1차 함수의 그래프는 직선 모양이며 이 떄문에 1차 함수는 선형적이다라고 할 수 있다. 선형 구조란 하나의 자료 뒤에 하나의 자료가 존재하는 것으로, 자료들 간의 앞 뒤 관계가 1:1 선형관계이다. 배열과 리스트도 이에 포함되고 더 나아가 스택과 큐도 이에 해당한다.

![](https://goodgid.github.io/assets/img/data_structure/linear_and_nonlinear_1.png)

* 비선형 : 비선형적(nonlinear)이란, **직선 모양이 아닌** 을 뜻한다. 따라서 곡선 모양이면 일단 비선형적이라고 할 수 있다. 비선형 자료구조란 하나의 자료 뒤에 여러개의 자료가 존재할 수 있는 것으로, 자료들 간의 앞뒤 관계가 1:n 또는 n:n의 관계를 갖는다. 트리와 그래프가 대표적이며 계층적 구조를 나타내기에 적절하다.

![](https://goodgid.github.io/assets/img/data_structure/linear_and_nonlinear_2.png)

#### 전체적 자료 구조

![](https://goodgid.github.io/assets/img/data_structure/linear_and_nonlinear_4.png)

<br/>

## Stack

선형 자료구조의 일종으로 Last In First Out (LIFO). 즉, 나중에 들어간 원소가 먼저 나온다. 이것은 Stack 의 가장 큰 특징이다. 차곡차곡 쌓이는 구조로 먼저 Stack 에 들어가게 된 원소는 맨 바닥에 깔리게 된다. 그렇기 때문에 늦게 들어간 녀석들은 그 위에 쌓이게 되고 호출 시 가장 위에 있는 녀석이 호출되는 구조이다.

* 나중에 들어간 원소가 먼저 나오는 구조이다.
* LIFO(Last in First Out)의 구조.
* 안드로이드의 액티비티를 관리하는 데 스택이 사용된다.
* 시간 복잡도 : O(n)
* 공간 복잡도 : O(n)
* 언제 사용? 함수의 콜 스택, 문자열 역순 출력, 연산자 후위 표기법 등

<br/>

## Queue

선형 자료구조의 일종으로 `First In First Out (FIFO)`. 즉, 먼저 들어간 놈이 먼저 나온다. Stack 과는 반대로 먼저 들어간 놈이 맨 앞에서 대기하고 있다가 먼저 나오게 되는 구조이다. 참고로 Java Collection 에서 Queue 는 인터페이스이다. 이를 구현하고 있는 `Priority queue`등을 사용할 수 있다.

* 먼저 들어간 원소가 먼저 나오는 구조이다.
* FIFO(First in First out)의 구조.
* 큐는 활용 분야가 되게 많다. 안드로이드의 경우, 루퍼의 메시지 큐가 예시 중 하나이다.
* 시간 복잡도 : O(n)
* 공간 복잡도 : O(n)
* 언제 사용? 버퍼, 마구 입력된 것을 처리하지 못하고 있는 상황, bfs 탐색 등

<br/>

### Personal Recommendation

* Stack 을 사용하여 미로찾기 구현하기
* Queue 를 사용하여 Heap 자료구조 구현하기
* Stack 두 개로 Queue 자료구조 구현하기
* Stack 으로 괄호 유효성 체크 코드 구현하기

<br/>

**참조**

* [원본링크1](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#stack-and-queue)
* [원본링크2](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Data%20Structure/%5BData%20Structure%5D%20Stack%EA%B3%BC%20Queue.md)
* [선형과 비선형의 차이점](https://m.blog.naver.com/a2zygote/220922530433)
* [선형(Linear) / 비선형(NonLinear) 자료구조](https://goodgid.github.io/DS-Linear-and-NonLinear/)
