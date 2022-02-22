# Overriding vs Overloading

둘 다 다형성을 높여주는 개념이고 비슷한 이름이지만, 전혀 다른 개념이라고 봐도 무방할 만큼 차이가 있다(오버로딩은 다른 시그니쳐를 만든다는 관점에서 다형성으로 보지 않는 의견도 있다). 공통점으로는 같은 이름의 다른 함수를 호출한다는 것이다.

<br/>

## 오버라이딩(Overriding)

상위 클래스 혹은 인터페이스에 존재하는 메소드를 하위 클래스에서 필요에 맞게 재정의하는 것을 의미한다. 자바의 경우는 오버라이딩 시 동적바인딩된다.

### 예제

아래와 같은 경우, SuperClass의 fun이라는 인터페이스를 통해 SubClass의 fun이 실행된다.

``` java
SuperClass object = new SubClass();
object.fun();
```

<br/>

## 오버로딩(Overloading)

오버로딩(Overloading) 메소드의 이름은 같다. return 타입은 동일하거나 다를 수 있지만, return 타입만 다를 수는 없다. 매개변수의 타입이나 갯수가 다른 메소드를 만드는 것을 의미한다. 다양한 상황에서 메소드가 호출될 수 있도록 한다. 언어마다 다르지만, 자바의경우 오버로딩은 다른 시그니쳐를 만드는 것으로, 아예 다른함수를 만든것과 비슷하다고 생각하면 된다. 시그니쳐가 다르므로 정적바인딩으로 처리 가능하며, 자바의 경우 정적으로 바인딩된다(코틀린은 default 파라미터를 사용하면 오버로딩과 비슷하게 동작, 코틀린 in action 책에서도 이를 추천하고 있음).

### 예제

아래와 같은 경우, fun(SuperClass super)이 실행된다.

``` java
main(blabla) {
  SuperClass object = new SubClass();
  fun(object);
}

fun(SuperClass super) {
  blabla....
}

fun(SubClass sub) {
  blabla....
}
```

<br/>

**참조**
* [원본링크](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Java#overriding-vs-overloading)