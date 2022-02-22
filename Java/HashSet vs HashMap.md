# HashSet, HashMap

HashMap과 HashSet은 모두 Collection Framework에 속한다.
기본적으로 Collection Framework는 Set, List, Queue 인터페이스로 나뉘어진다.

* Set : 객체를 받지만, 중복되는 값은 허용하지 않는다.(순서가 없다.)
* List : 인덱싱하여 중복을 허용한다.
* Queue : 선입선출(FCFS) 알고리즘을 기반으로 한다.

<br/>

## HashSet

Set 인터페이스를 구현한 것으로 중복된 값을 허용하지 않는다. HashSet에 들어가는 객체들은 반드시 equals()와 hashCode() 메소드를 구현해야 하는데, 이 메소드들을 가지고 HashSet에 들어갈 때, 중복된 객체가 있는지 여부를 체크하게 된다.

<br/>

## HashMap

Map 인터페이스를 구현한 것으로 Key-Value 형태의 데이터를 저장한다.
중복된 Key 값은 허용되지 않는다. 다만, 중복된 값의 저장은 허용한다.
null value, null key 를 허용한다.

* HashMap : 집어 넣은 순서를 유지하지 않는다.
* TreeMap : 내부적으로 RedBlack Tree로 저장되며, Key 값을 기준으로 알파벳 순서대로 정렬한다.
* LinkedHashMap : 집어 넣은 순서를 유지한다.

<br/>

## 차이점

* HashMap
   1. Map 인터페이스 구현
   2. 데이터를 Key-Value 형식으로 저장한다.
   3. HashMap에서 hashCode 값은 key value를 이용하여 생성한다.
   4. HashMap은 unique key를 이용하여 데이터에 바로 접근하기에 HashSet에 비해서 빠르다.

* HashSet
   1. Set 인터페이스를 구현
   2. 객체만 저장할 수 있다.
   3. 들어가는 객체를 이용하여 hashCode를 생성하고, equals() 메소드를 이용하여 hashCode를 비교해서 중복된 객체가 있는지 체크한다.(equals() 메소드는 중복된 객체가 있으면 true, 없으면 false를 반환)
   4. HashMap에 비해서 느리다.

<br/>

## HashMap vs HashTable vs ConcurrentHashMap

* 컬렉션의 Map 인터페이스를 구현한 클래스이다.
* key, value를 이용하여 값을 저장하는 구조이다.

1. HashTable
   * 동기화를 지원하므로 멀티 스레드 환경에서 사용할 수 있다.
   * Key, Value에는 Null을 허용하지 않는다.
   * put, get과 같은 주요 메소드에 synchronized 키워드가 선언되어 있다.
   * 반환값은 Enumeration(컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스인데, Enumeration은 컬렉션 프레임워크 이전에 사용되던 인터페이스로 Iterator의 사용을 권장한다.)

2. HashMap
   * 동기화를 지원하지 않으므로 멀티 스레드 환경에는 적합하지 않다.
   * Key, Value에 Null을 허용한다.
   * 주요 메소드에 synchronized 키워드가 없다.
   * 반환값은 Fail-Fast Iterator(Iterator은 remove() 메소드가 추가되고 메소드의 네이밍이 간략화 되었으며, 다른 스레드(lock된 상황에서)에서 해당 자료에 요소를 수정(삽입, 삭제, 수정 등)이 발생하면 ConcurrentModificationException을 발생시켜 일관성을 보장한다. 이를 Fail-Fast Iterator라 한다. 

3. ConcurrentHashMap
   * HashMap에 ThreadSafe를 추가한 것으로서 멀티 스레드 환경에서 HashMap을 동기화 시킨 Collections이다.
   * HashTable은 메소드 전체에 synchronized를 선언하는 반면 **ConcurrentHashMap은 동기화가 필요한 부분에만 동기화를 적용했기 때문에 동기화에서 발생하는 오버헤드가 줄어든다.**
   * 반환값은 Fail-Safe Iterator(ConcurrentHashMap의 경우 Map의 복사본을 참조하는 Iterator를 반환하며 다시 반환받은 시점에 Map에 수정이 있을 경우 해당 Iterator는 반영되지 않는다. 고로 ConcurrentModificationException또한 발생하지 않는다. 이는 약한 일관성(Weakly Consistent)를 제공하지만 다중 스레드상황에서 해당 Map의 무결성(?)을 보장)

<br/>

**결론**

멀티 스레드 환경이라면 HashMap보단 HashTable 혹은 ConcurrentHashMap을 사용하는 것을 고려해야하고, HashTable보다는 Java 5부터 제공하는 ConcurrentHashMap을 사용하는 것이 성능적인 면에서 더 좋다고 볼 수있다. 멀티 스레드 환경이 아니라면 HashMap을 사용하면 된다.

<br/>

**참조**
* [원본링크](https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Java/HashSet%20vs%20HashMap.md)