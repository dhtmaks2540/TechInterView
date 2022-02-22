# Multi-Thread 환경에서의 개발

개발을 시작하는 입장에서 멀티 스레드를 고려한 프로그램을 작성할 일이 별로 없고 실제로 부딪히기 힘든 문제이기 때문에 많은 입문자들이 잘 모르고 있는 부분 중 하나라고 생각한다. 하지만 이 부분은 정말 중요하며 고려하지 않았을 경우 엄청난 버그를 양산할 수 있기 때문에 정말 중요하다.

<br/>

## Field member(코틀린에서는 프로퍼티)

`필드(field)`란 클래스에 변수를 정의하는 공간을 의미한다. 이곳에 변수를 만들어두면 메소드 끼리 변수를 주고 받는 데 있어서 참조하기 쉬우므로 정말 편리한 공간 중 하나이다. 하지만 객체가 여러 스레드가 접근하는 싱글톤 객체라면 field 에서 상태값을 갖고 있으면 안된다. 모든 변수를 parameter 로 넘겨받고 return 하는 방식으로 코드를 구성해야 한다.

<br/>

## 동기화(Synchronized)

필드에 Collection 이 불가피하게 필요할 때는 어떠한 방법을 사용할까? Java 에서는 `synchronized` 키워드를 사용하여 스레드 간 race condition 을 통제한다. 이 키워드를 기반으로 구현된 Collection 들도 많이 존재한다. `List`를 대신하여 `Vector`를 사용할 수 있고, `Map`을 대신하여 `HashTable`을 사용할 수 있다. 하지만 이 Collection 들은 제공하는 API 가 적고 성능도 좋지 않다.

기본적으로는 `Collections`라는 util 클래스에서 제공되는 static 메소드를 통해 이를 해결할 수 있다. `Collections.synchronizedList()`, `Collections.synchronizedSet()`, `Collections.synchronizedMap()` 등이 존재한다. JDK 1.7 부터는 `concurrent package`를 통해 `ConcurrentHashMap`이라는 구현체를 제공한다. Collections util 을 사용하는 것보다 `synchronized` 키워드가 적용된 범위가 좁아서 보다 좋은 성능을 낼 수 있는 자료구조이다.

<br/>

## ThreadLocal

스레드 사이에 간섭이 없어야 하는 데이터에 사용한다. 멀티스레드 환경에서는 클래스의 필드에 멤버를 추가할 수 없고 매개변수로 넘겨받아야 하기 때문이다. 즉, 스레드 내부의 싱글톤을 사용하기 위해 사용한다. 주로 사용자 인증, 세션 정보, 트랜잭션 컨텍스트에 사용한다.

스레드 풀 환경에서 ThreadLocal 을 사용하는 경우 ThreadLocal 변수에 보관된 데이터의 사용이 끝나면 반드시 해당 데이터를 삭제해 주어야 한다. 그렇지 않을 경우 재사용되는 쓰레드가 올바르지 않은 데이터를 참조할 수 있다.

ThreadLocal 을 사용하는 방법은 간단하다.

1. ThreadLocal 객체를 생성한다.
2. ThreadLocal.set() 메서드를 이용해서 현재 스레드의 로컬 변수에 값을 저장한다.
3. ThreadLocal.get() 메서드를 이용해서 현재 스레드의 로컬 변수 값을 읽어온다.
4. ThreadLocal.remove() 메서드를 이용해서 현재 스레드의 로컬 변수 값을 삭제한다.

<br/>

**참조**
* [원본링크](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Java#multi-thread-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C%EC%9D%98-%EA%B0%9C%EB%B0%9C)