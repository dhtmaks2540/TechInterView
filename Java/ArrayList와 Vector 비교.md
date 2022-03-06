# ArrayList와 Vector 비교

## Vector

* Vector란?
  * Vector 클래스는 예전의 자바에서 제공했던 **레거시 클래스** 이다.
  * 레거시 클래스란 Collections 프레임워크가 포함되어 있지 않던 초기 자바 버전에서 정의한 인터페이스이다.
  * 현재는 재구성 및 설계되어서 현재의 Collections 프레임워크와 완벽하게 호환된다.

* Vector의 특징
  * 필요에 따라 크기를 동적으로 저잘할 수 있는 동적배열을 구현한다.
  * 배열과 마찬가지로 정수 인덱스를 사용하여 배열에 액세스 할 수 있다.
  * 동기화(Thread Safe) 되어있으며 한 번에 하나의 스레드만 벡터의 메소드를 호출할 수 있다. 즉, 멀티 스레드가 동시에 이 메소드들을 실행할 수 없고, 하나의 스레드가 실행을 완료해야만 다른 스레드들이 실행할 수 있다. 그래서 멀티 스레드 환경에서 안전하게 객체를 추가하고 삭제할 수 있다.

<br/>

### Vector의 기본적인 사용법

* Vector 선언

``` kotlin
val intVector = Vector<Int>()
val stringVector = Vector<String>()
val studentVector = Vector<Student>()
```

<br/>

* Vector 값 추가

``` kotlin
val intVector = Vector<Int>()
intVector.add(10)
intVector.add(null)
intVector.add(0, 20)
```

Vector에 값을 추가하려면 add(index, value) 메소드를 사용한다. 구조는 ArrayList나 LinkedList와 같다. 인덱스를 생략하면 Vector의 맨 뒤에 값이 추가되며 index 중간에 값을 추가하면 해당 인덱스부터 마지막 인덱스까지 모두 한 칸씩 뒤로 밀려난다.

<br/>

* Vector 값 삭제

``` kotlin
intVector.remove(10)
intVector.removeAllElements()
```

Vector에서 값을 삭제하려면 remove(element) 메소드를 사용하면 된다. 모든 값을 제거하려면 clear() 메소드나 removeAllElements() 메소드를 사용하면 된다.

위의 예제에서 보았듯이, Vector의 사용법은 ArrayList와 굉장히 비슷하다. 

<br/>

## ArrayList

* ArrayList란?
  * **Collections 프레임워크**의 일부이며 java.util 패키지 내에 존재한다.
  * Vector와 마찬가지로 동적 배열을 사용하기 위해 사용된다.

* ArrayList의 특징
  * 자바 표준 배열보다 약간 느릴수 있지만 배열에서 많은 조작이 필요로 할 때 유용하게 사용된다.
  * 기본 데이터 타입(int, char 등)에 대해 만들 수 없기 때문에 Integer, Object 등의 객체에 대해 참조해서 사용한다(코틀린은 기본이 객체이기에 그대로 사용하면 된다).

<br/>

## ArrayList와 Vector의 주요 차이점

1. 동기화(Synchronize)

Vector가 동기화 된다면 ArrayList는 동기화 되지 않은 상태이다. 쉽게 말해 Vector는 한 번에 하나의 스레드만 엑세스(접근) 가능하며, ArrayList는 동시에 여러 스레드가 작업할 수 있다. ArrayList에서 여러 스레드가 동시에 액세스하는 경우 개발자가 명시적으로 동기화하는 코드를 추가하거나 동기화 옵션을 사용해야 한다. 그렇지 않으면 자원 공유의 문제(동기화)가 발생한다.

<br/>

2. **스레드 안전(Thread Safe)**

스레드 안전이란 멀티 스레드 프로그래밍에서 여러 스레드가 동시에 접근이 이루어져도 프로그램에 실행에 문제가 없음을 뜻한다. 앞서 말했듯이 Vector는 동기화가 되어있기 때문에 한 번에 하나의 스레드만 접근할 수 있기에 스레드 안전하다. 반면에 ArrayList는 동기화되지 않았기 때문에 명시적으로 동기화 할 필요가 있다.

<br/>

3. **성능**

ArrayList는 동기화되지 않았기 때문에 동기화 된 **벡터보다 성능면에서 더 빠르다.**

<br/>

4. **크기 증가**

Vector와 ArrayList 모두 동적 배열 클래스인데, 다만 최대 인덱스를 초과할 때 추가되는 인덱스의 수가 다르다. Vector는 현재 배열 크기의 100% 가 증가하며, ArrayList의 경우 현재 배열 크기의 50% 가 증가한다.

<br/>

## 결론
멀티스레드 환경이 아닌 경우 **ArrayList를 사용하는 것이 바람직** 하다. 따라서 Vector를 사용하기 위한 명시적 요구사항이 없는 경우 ArrayList를 사용해야 한다.

<br/>

**참조**
* [원본링크](https://yeolco.tistory.com/94)
* [Vector 기본적인 사용법](https://coding-factory.tistory.com/553)