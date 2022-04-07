# Transaction

## 트랜잭션(Transaction)이란 무엇인가?

트랜잭션은 **데이터베이스의 논리적 작업 단위** 이다. 하나의 작업을 수행하기 위해 필요한 데이터베이스 연산 기능들을 모아놓은 것이며 분리되지 않도록 하여 작업의 완전성을 보장한다. 즉, 논리적인 작업 셋을 모두 완벽하게 처리하거나 또는 처리하지 못할 경우에는 원 상태로 복구해서 작업의 일부만 적용되는 현상이 발생하지 않게 만들어주는 기능이다.

트랜잭션을 통해서 데이터베이스의 회복과 병행 제어가 가능하다. 즉, 데이터베이스에서 오류가 발생하는 경우의 빠른 회복이나, 여러 사용자가 동시에 데이터베이스를 사용할 수 있도록 제어해주는 중요한 역할을 한다. 데이터베이스의 연산은 SQL문으로 표현한다면, 하나의 작업을 수행하는 SQL문의 집합으로 생각할 수 있다.

<br/>

## 트랜잭션과 Lock

잠금(Lock)과 트랜잭션은 서로 비슷한 개념 같지만 사실 잠금은 동시성을 제어하기 위한 기능이고 트랜잭션은 데이터의 정합성을 보장하기 위한 기능이다. 잠금은 여러 커넥션에서 동시에 동일한 자원을 요청할 경우 순서대로 한 시점에는 하나의 커넥션만 변경할 수 있게 해주는 역할을 한다. 여기서 자원은 레코드나 테이블을 말한다. 이와는 조금 다르게 트랜잭션은 꼭 여러 개의 변경 작업을 수행하는 쿼리가 조합되었을 때만 의미있는 개념은 아니다. 트랜잭션은 하나의 논리적인 작업 셋 중 하나의 쿼리가 있든 두 개 이상의 쿼리가 있든 관계없이 논리적인 작업 셋 자체가 100% 적용되거나 아무것도 적용되지 않아야 함을 보장하는 것이다. 예를 들면 HW 에러 또는 SW 에러와 같은 문제로 인해 작업에 실패가 있을 경우, 특별한 대책이 필요하게 되는데 이러한 문제를 해결하는 것이다.

<br/>

## 예시

작업 단위 -> **많은 SQL 명령문들을 사람이 정하는 기준에 따라 정하는 것**

```
예시) 사용자 A가 사용자 B에게 만원을 송금한다.

* 이때 DB의 작업
1. 사용자 A의 계좌에서 만원을 차감한다 : UPDATE 문을 사용해 사용자 A의 잔고를 변경
2. 사용자 B의 계좌에 만원을 추가한다 : UPDATE 문을 사용해 사용자 B의 잔고를 변경

현재 작업 단위 : 출금 UPDATE문 + 입금 UPDATE문
→ 이를 통틀어 하나의 트랜잭션이라고 한다.
- 위 두 쿼리문 모두 성공적으로 완료되어야만 "하나의 작업(트랜잭션)"이 완료되는 것이다. `Commit`
- 작업 단위에 속하는 쿼리 중 하나라도 실패하면 모든 쿼리문을 취소하고 이전 상태로 돌려놓아야한다. `Rollback`
```

<br/>

## 트랜잭션의 특성

트랜잭션은 어떠한 특성을 만족할까? Transaction은 다음의 ACID라는 4가지 특성을 만족해야 한다.

* **원자성(Atomicity)**

**만약 트랜잭션 중간에 어떠한 문제가 발생한다면 트랜잭션에 해당하는 어떠한 작업 내용도 수행되어서는 안되며 아무런 문제가 발생되지 않았을 경우에만 모든 작업이 수행되어야 한다.**

* **일관성(Consistency)**

**트랜잭션이 완료된 다음의 상태에서도 트랜잭션이 일어나기 전의 상황과 동일하게 데이터의 일관성을 보장해야 한다.** 데이터의 모순이 있어서는 안 된다.

은행 송금 기능을 생각해보면, 트랜잭션 수행 이전의 송금자와 수금자의 잔액 합이 수행 후에 달라지거나, 혹은 잔액을 나타내는 자료형이 정수형에서 문자열로 바뀌는 등의 모순이 발생하면 안된다.

* **독립성, 격리성(Isolation)**

**각각의 트랜잭션은 서로 간섭없이 독립적으로 수행되어야 한다.**

둘 이상의 트랜잭션이 병행 실행되는 경우, 현재 수행 중인 트랜잭션이 완료되기 전에 현재 트랜잭션이 생성한 중간 연산 결과를 다른 트랜잭션이 접근해서는 안 된다. 독립성을 통해서, 사용자들은 여러 트랜잭션이 동시에 수행되고 있는 것처럼 느끼면서도 정확한 결과를 얻을 수 있게 된다.

* **지속성(Durability)**

**트랜잭션이 정상적으로 종료된 다음에는 영구적으로 데이터베이스에 작업의 결과가 저장되어야 한다.** 시스템의 장애가 발생하더라도 결과는 데이터베이스에 그대로 남아있어야 하며, 지속성을 보장하기 위해서 회복 기능이 필요하다.

<br/>

## 트랜잭션의 연산

트랜잭션의 연산으로는 크게 Commit과 Rollback 두 가지가 있다.

1. **Commit 연산**

Commit 연산은 하나의 트랜잭션이 성공적으로 종료된 후, 데이터베이스가 일관된 상태를 유지할 때 **갱신 연산이 완료되었다고 트랜잭션 관리자에게 알려주고 최종적으로 데이터베이스에 반영하는 연산** 이다.

2. **Rollback 연산**

Rollback 연산은 하나의 트랜잭션이 바정상적으로 종료되어 데이터베이스의 일관성을 잃었을 때 **트랜잭션이 지금까지 실행한 연산의 결과가 취소되고 트랜잭션 수행 이전의 상태로 돌아가는 연산** 이다.
Rollback을 하는 경우엔 해당 트랜잭션을 재시작하거나 폐기한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcJP2nH%2FbtrbIemBsy1%2FyQkjAG5WfK7NtqS7vKfak1%2Fimg.png)

<br/>

## 트랜잭션의 상태

![](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/raw/master/Database/images/transaction-status.png)

* **Active**

트랜잭션이 작업을 시작하여 실행 중인 상태

* **Failed**

트랜잭션에 오류가 발생하여 실행이 중단된 상태. 트랜잭션이 더 이상 정상적으로 진행할 수 없는 상태를 말한다.

* **Partially Committed**

트랜잭션의 마지막 연산까지 실행되고 commit 요청이 들어온 직후의 상태, 최종 결과를 데이터베이스에 아직 반영하지 않은 상태. 즉, 트랜잭션의 `commit` 이전 `sql` 문이 수행되고 `commit` 만 남은 상태

* **Committed**

트랜잭션이 성공적으로 종료되어 commit 연산을 실행한 후의 상태

* **Aborted**

트랜잭션이 비정상적으로 종료되어 Rollback 연산을 수행한 상태. 즉, 트랜잭션 실행 이전의 데이터로 돌아간 상태

* **Partially Committed 와 Committed 의 차이점**

`Commit` 요청이 들어오면 상태는 `Partial Commited` 상태가 된다. 이후 `Commit`을 문제없이 수행할 수 있으면 `Committed` 상태로 전이되고, 만약 오류가 발생하면 `Failed` 상태가 된다. 즉, `Partial Commited`는 `Commit` 요청이 들어왔을때를 말하며, `Commited`는 `Commit`을 정상적으로 완료한 상태를 말한다.

<br/>

## 트랜잭션을 사용할 때 주의할 점

트랜잭션은 꼭 필요한 최소의 코드에만 적용하는 것이 좋다. 즉 트랜잭션의 범위를 최소화하라는 의미다. 일반적으로 데이터베이스 커넥션은 개수가 제한적이다. 그런데 각 단위 프로그램이 커넥션을 소유하는 시간이 길어진다면 사용 가능한 여유 커넥션의 개수는 줄어들게 된다. 그러다 어느 순간에는 각 단위 프로그램에서 커넥션을 가져가기 위해 기다려야 하는 상황이 발생할 수도 있는 것이다.

<br/>

## 교착상태

### 교착상태란 무엇인가

복수의 트랜잭션을 사용하다보면 교착상태가 일어날수 있다. 교착상태란 두 개 이상의 트랜잭션이 특정 자원(테이블 또는 행)의 잠금(Lock)을 획득한 채 다른 트랜잭션이 소유하고 있는 잠금을 요구하면 아무리 기다려도 상황이 바뀌지 않는 상태가 되는데, 이를 교착상태라고 한다.

<br/>

### 교착상태의 예(MySQL)

MySQL [MVCC](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)에 따른 특성 때문에 트랜잭션에서 갱신 연산(Insert, Update, Delete)를 실행하면 잠금을 획득한다. (기본은 행에 대한 잠금)

![](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/raw/master/Database/images/deadlock.png)

트랜잭션 1이 테이블 B의 첫번째 행의 잠금을 얻고 트랜잭션 2도 테이블 A의 첫번째 행의 잠금을 얻었다고 가정하자.

``` sql
Transaction 1> create table B (i1 int not null primary key) engine = innodb;
Transaction 2> create table A (i1 int not null primary key) engine = innodb;

Transaction 1> start transaction; insert into B values(1);
Transaction 2> start transaction; insert into A values(1);
```

트랜잭션을 commit 하지 않은 채 서로의 첫번째 행에 대한 잠금을 요청하면

``` sql
Transaction 1> insert into A values(1);
Transaction 2> insert into B values(1);
ERROR 1213 (40001): Deadlock found when trying to get lock; try restarting transaction
```

Deadlock이 발생한다. 일반적인 DBMS는 교착상태를 독자적으로 검출해 보고한다.

<br/>

### 교착 상태의 빈도를 낮추는 방법

* 트랜젝션을 자주 커밋한다.

* 정해진 순서로 테이블에 접근한다. 위에서 트랜잭션 1 이 테이블 B -> A 의 순으로 접근했고, 트랜잭션 2 는 테이블 A -> B의 순으로 접근했다. 트랜잭션들이 동일한 테이블 순으로 접근하게 한다.

* 읽기 잠금 획득 (SELECT ~ FOR UPDATE)의 사용을 피한다.

* 한 테이블의 복수 행을 복수의 연결에서 순서 없이 갱신하면 교착상태가 발생하기 쉽다, 이 경우에는 테이블 단위의 잠금을 획득해 갱신을 직렬화 하면 동시성을 떨어지지만 교착상태를 회피할 수 있다.

<br/>

**참조**

* [원본링크1](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database#transaction)
* [원본링크2](https://gyoogle.dev/blog/computer-science/data-base/Transaction.html)
* [트랜잭션의 여러가지 개념](https://rebro.kr/162?category=484170)