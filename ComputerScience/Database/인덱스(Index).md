# Index

## 인덱스란?

인덱스는 **데이터베이스의 테이블에 대한 검색 속도를 향상시켜주는 자료구조** 이다. 테이블의 특정 컬럼(Column)에 인덱스를 생성하면, 해당 컬럼의 데이터를 정렬한 후 별도의 메모리 공간에 데이터의 물리적 주소와 함께 저장된다. 컬럼의 값과 물리적 주소를 (key, value)의 한 쌍으로 저장한다.

인덱스는 책의 **목차** 혹은 **색인** 이라고 생각하면 된다. 책에서 원하는 내용을 찾을 때 목차나 색인을 이용하면 훨씬 빠르게 찾을 수 있는데, 마찬가지로 테이블에서 원하는 데이터를 찾기 위해 인덱스를 이용하면 빠르게 찾을 수 있다. 그러므로 데이터 = 책의 내용, 인덱스 = 책의 목차, 물리적 주소 = 책의 페이지 번호라고 생각할 수 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fmf8up%2FbtrcQfeY8br%2FlokgR9fitqflfot0Ur0T61%2Fimg.png)

<br/>

## 인덱스의 장단점

### 장점

인덱스의 장점은 앞에서 말했듯이 **테이블을 검색하는 속도와 성능이 향상된다.** 또 그에 따라 시스템의 전반적인 부하를 줄일 수 있다.

핵심은 인덱스에 의해 데이터들이 **정렬된 형태를 갖는다는 것** 이다. 기존엔 Where문으로 특정 조건의 데이터를 찾기 위해서 테이블의 전체를 조건과 비교해야 하는 '풀 테이블 스캔(Full Table Scan)' 작업이 필요했는데, 인덱스를 이용하면 데이터들이 정렬되어 있기 때문에 조건에 맞는 데이터를 빠르게 찾을 수 있다.

또 ORDER BY 문이나 MIN/MAX 같은 경우도 이미 정렬되어 있기 때문에 빠르게 수행할 수 있다.

<br/>

### 단점

인덱스가 항상 정렬된 상태로 유지되어 오는 장점도 있지만, 그에 따른 여러 단점도 존재한다.

1. 인덱스를 관리하기 위한 추가 작업이 필요
2. 추가 저장 공간 필요
3. 잘못 사용하는 경우 오히려 검색 성능 저하

인덱스를 항상 정렬된 상태로 유지해야 하기 때문에 인덱스가 적용된 컬럼에 삽입(INSERT), 삭제(DELETE), 수정(UPDATE) 작업을 수행하면 다음과 같은 추가 작업이 필요하다.

* INSERT : 새로운 데이터에 대한 인덱스를 추가
* DELETE : 삭제하는 데이터의 인덱스를 사용하지 않는다는 작업 수행
* UPDATE : 기존의 인덱스를 사용하지 않음 처리, 갱신된 데이터에 대한 인덱스 추가(INSERT, DELETE의 작업 모두)

이처럼 인덱스의 수정도 추가적으로 필요하기 때문에 데이터의 수정이 잦은 경우 성능이 낮아진다. 또 데이터의 인덱스를 **제거하는 것이 아니라 '사용하지 않음'으로 처리하고 남겨두기 때문에** 수정 작업이 많은 경우 실제 데이터에 비해 인덱스가 과도하게 커지는 문제점이 발생할 수 있다. 별도의 메모리 공간에 저장되기 때문에 추가 저장 공간이 많이 필요하게 된다.

또한 인덱스는 전체 데이터의 10 ~ 15% 이상의 데이터를 처리하거나, 데이터의 형식에 따라 오히려 성능이 낮아질 수 있다. 예를 들어 나이나 성별과 같이 값의 range가 적은 컬럼인 경우, 인덱스를 읽고 나서 다시 많은 데이터를 조회해야 하기 때문에 비효율적이다.

<br/>

## 인덱스를 사용하면 좋은 경우

인덱스를 효율적으로 사용하기 위해선 데이터의 range가 넓고 중복이 적을수록, 조회가 많거나 정렬된 상태가 유용한 컬럼에 사용하는 것이 좋다. 따라서 다음과 같은 경우들에 인덱스를 사용하면 효율적이다. 

* 규모가 큰 테이블
* 삽입(INSERT), 수정(UPDATE), 삭제(DELETE) 작업이 자주 발생하지 않는 컬럼
* WHERE나 ORDER BY, JOIN 등이 자주 사용되는 컬럼
* 데이터의 중복도가 낮은 컬럼

<br/>

## 인덱스의 자료 구조

인덱스는 여러 자료구조를 이용해서 구현할 수 있는데, 대표적으로 해시 테이블(Hash Table)과 B+Tree가 있다.

<br/>

### 해시 테이블

**해시 테이블은 key와 value를 한 쌍으로 데이터를 저장하는 자료구조** 이다. (key, value)로 쌍을 표현하며, key값을 이용해 대응되는 value 값을 구하는 방식이다. 해시 충돌이라는 변수가 존재하지만 평균적으로 O(1)의 매우 빠른 시간안에 원하는 데이터를 탐색할 수 있는 구조이다.

해시 테이블을 이용한다면 인덱스는 (key, value) = (컬럼의 값, 데이터의 위치)로 구현하는데, 해시 테이블은 실제로 인덱스에서 잘 사용되지 않는다. 그 이유는 **해시 테이블은 등호(=) 연산에 최적화되어있기 떄문이다.** 데이터베이스에선 부등호(<, >) 연산이 자주 사용되는데, 해시 테이블 내의 데이터들은 정렬되어 있지 않으므로 특정 기준보다 크거나 작은 값을 빠른 시간 내에 찾을 수가 없다.

<br/>

### B+ Tree

기존의 [B-Tree](https://rebro.kr/169)는 어느 한 데이터의 검색은 효율적이지만, 모든 데이터를 한 번 순회하는 데에는 트리의 모든 노드를 방문해야 하므로 비효율적이다. 이러한 B-Tree의 단점을 개선시킨 자료구조가 B+Tree이다.

B+ Tree는 **오직 leaf node에만 데이터를 저장하고 leaf node가 아닌 node에서는 자식 포인터만 저장한다.** 그리고 **leaf node끼리는 Linked list로 연결되어 있다.** 또, B+ Tree에서는 반드시 leaf node에만 데이터가 저장되기 때문에 중간 node에서 key를 올바르게 찾아가기 위해서 key가 중복될 수 있다.

#### B-Tree 예시

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSvp6z%2FbtrdEi9c2DR%2FR4Dnmqkl8RWcqQPBACI9fK%2Fimg.png)


#### B+Tree 예시
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAARBC%2FbtrdDydoUp7%2F9h4KOXBRyDNKpKDAe2ugq0%2Fimg.png)

이로 인한 장점이 무엇일까?

1. leaf node를 제외하고 데이터를 저장하지 않기 때문에 메모리를 더 확보할 수 있다. 따라서 하나의 node에 더 많은 포인터를 가질 수 있기 떄문에 트리의 높이가 더 낮아지므로 검색 속도를 높일 수 있다.

2. Full scan을 하는 경우 B+Tree는 leaf node에만 데이터가 저장되어 있고, leaf node끼리 linked list로 연결되어 있기 때문에 선형 시간이 소모된다. 반면 B-Tree는 모든 node를 확인해야 한다.

반면, B-Tree의 경우 최상의 경우 특정 key를 root node에서 찾을 수 있지만, **B+Tree의 경우 반드시 특정 key에 접근하기 위헤서 leaf node까지 가야 하는 단점이 있다.**

인덱스에서 B-Tree 대신 주로 B+Tree를 사용하는 이유는 뭘까?

해시 테이블에서 언급했듯이 **인덱스 컬럼은 부등호를 이용한 순차 검색 연산이 자주 발생할 수 있다.** 따라서 B+Tree의 Linked list를 이용하면 순차 검색을 효율적으로 할 수 있게 된다.

B+Tree의 검색 과정은 B-Tree와 동일하다. 반면 B+Tree의 삽입과 삭제 과정은 약간 차이가 있다. 기본적으로 B+Tree의 삽입과 삭제는 항상 leaf node에서 일어난다.

<br/>

### 삽입

1. key의 수가 최대보다 적은 leaf node에 삽입하는 경우

해당 node의 가장 앞이 아닌 곳에 삽입되는 경우는 단순히 삽입해 주면 된다. 

하지만, leaf node의 가장 앞에 삽입되는 경우는, 해당 node를 가리키는 부모 node의 포인터의 오른쪽에 위치한 key를 K로 바꿔준다. 그리고 leaf node끼리 Linked list로 이어줘야 하므로 삽입된 key에 Linked list로 연결한다. 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDUQXU%2FbtrdJ3Qj4is%2FWKfTSC6lw2Ew6lgILENrs0%2Fimg.png)

<br/>

## key의 수가 최대인 leaf node에 삽입하는 경우

key의 수가 최대이므로 삽입하는 경우 분할을 해주어야 한다. 만약 중간 node에서 분할이 일어나는 경우는 B-Tree와 동일하게 해주면 된다. 

leaf node에서 분할이 일어나는 경우는 중간 key를 부모 node로 올려주는데 이때, 오른쪽 node에 중간 key를 붙여 분할한다. 그리고 분할된 두 node를 Linked List로 연결해준다. 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnNNnn%2Fbtrdz7gbjh3%2Fi9TYzadkhRWaskIjuSWw3K%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fp7dZ2%2FbtrdC5Jm92U%2FmmjNOZmkM5CxgJZnOvcWE0%2Fimg.png)

이후 과정은 B-Tree의 과정과 동일하다.

<br/>

### 삭제

1. 삭제할 key가 leaf node의 가장 앞에 있지 않은 경우

B-Tree와 동일한 방법으로 삭제된다.

<br/>

2. 삭제할 key가 leaf node의 가장 앞에 위치한 경우

이 경우는 leaf node가 아닌 node에 key가 중복해서 존재한다. 따라서 해당 key를 노드보다 오른쪽에 있으면서 가장 작은 값으로 바꿔주어야 한다. 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdyUcln%2FbtrdC4XYNZS%2Foq8kTj1kR35RbO2mdmGfjK%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZwaQY%2FbtrdEkF1vqc%2FXhimY4ETxpu9U6vRDoir91%2Fimg.png)

<br/>

## Index의 성능과 고려해야할 사항

SELECT 쿼리의 성능을 월등히 향상시키는 INDEX 항상 좋은 것일까? 쿼리문의 성능을 향상시킨다는데, 모든 컬럼에 INDEX를 생성해두면 빨라지지 않을까? **결론부터 말하자면 그렇지 않다.** 우선, 첫번째 이유는 INDEX를 생성하게 되면 INSERT, DELETE, UPDATE 쿼리문을 실행할 때 별도의 과정이 추가적으로 발생한다. INSERT의 경우 INDEX에 대한 데이터도 추가해야 하므로 그만큼 성능에 손실이 따른다. DELETE의 경우 INDEX에 존재하는 값은 삭제하지 않고 사용안한다는 표시로 남게 된다. 즉 row의 수는 그대로인 것이다. 이 작업이 반복되면 어떻게 될까?

실제 데이터는 10만건인데 데이터가 100만건 있는 결과를 낳을 수도 있는 것이다. 이렇게 되면 인덱스는 더 이상 제 역할을 못하게 되는 것이다. UPDATE의 경우는 INSERT의 경우, DELETE의 경우의 문제점을 동시에 수반한다. 이전 데이터가 삭제되고 그 자리에 새 데이터가 들어오는 개념이다. 즉 변경 전 데이터는 삭제되지 않고 insert로 인한 split도 발생하게 된다.

하지만 더 중요한 것은 컬럼을 이루고 있는 데이터의 형식에 따라서 인덱스의 성능이 악영향을 미칠 수 있다는 것이다. 즉, 데이터의 형식에 따라 인덱스를 만들면 효율적이고 만들면 비효율적은 데이터의 형식에 존재한다는 것이다. 어떤 경우에 그럴까?

`이름`, `나이`, `성별` 세 가지의 필드를 갖고 있는 테이블을 생각해보자. 이름은 온갖 경우의 수가 존재할 것이며 나이는 INT 타입을 갖을 것이고, 성별은 남,녀 두 가지 경우에 대해서만 데이터가 존재할 것임을 쉽게 예측할 수 있다. 이 경우 어떤 컬럼에 대해서 인덱스를 생성하는 것이 효율적일까? **결론부터 말하자면 이름에 대해서만 인덱스를 생성하면 효율적이다.**

왜 성별이나 나이는 인덱스를 생성하면 비효율적일까? 10000 레코드에 해당하는 테이블에 대해서 2000 단위로 성별에 인덱스를 생성했다고 가정하자. 값의 range가 적은 성별은 인덱스를 읽고 다시 한 번 디스크 I/O 가 발생하기 때문에 그 만큼 비효율적인 것이다.

<br/>

원본링크 3 및 B-Tree 설명 읽어보기!

**참조**
* [원본링크1](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database#index)
* [원본링크2](https://gyoogle.dev/blog/computer-science/data-base/Index-.html)
* [원본링크3](https://rebro.kr/167?category=484170)
* [자료구조 - 그림으로 알아보는 B-Tree](https://velog.io/@emplam27/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-B-Tree)
* [자료구조 - 그림으로 알아보는 B+Tree](https://velog.io/@emplam27/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-B-Plus-Tree)