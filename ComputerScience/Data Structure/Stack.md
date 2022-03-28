# 스택이란

## Goal

* 스택(Stack)의 기본 연산을 이해한다.
* java로 스택(Stack)을 구현할 수 있다.

<br/>

## 스택의 개념

한 쪽 끝에서만 자료를 넣고 뺄 수 있는 LIFO(Last In First Out) 형식의 자료구조이다.

<br/>

## 스택의 연산

스택은 **LIFO(Last In First Out)** 를 따른다. 즉, 가장 최근에 스택에 추가한 항목이 가장 먼저 제거될 항목이다.

* pop(): 스택에서 가장 위에 있는 항목을 제거한다.
* push(item): item 하나를 스택의 가장 윗 부분에 추가한다.
* peek(): 스택의 가장 위에 있는 항목을 반환한다.
* isEmpty(): 스택이 비어 있을 때에 true를 반환한다.

<br/>

## 스택의 구현

문제의 종류에 따라 배열보다 스택에 데이터를 저장하는 것이 더 적합한 방법일 수 있다.

* 배열과 달리 스택은 상수 시간에 i번째 항목에 접근할 수 없다.
* 하지만 스택에서 데이터를 추가하거나 삭제하는 연산은 상수 시간에 가능하다.
* 배열처럼 원소들을 하나씩 옆으로 밀어 줄 필요가 없다.

스택은 **연결 리스트** 로 구현할 수 있다. 연결리스트의 같은 방향에서 아이템을 추가하고 삭제하도록 구현한다.

#### Java 구현

``` java
public class MyStack {
  private static class StackNode {
    private T data;
    private StackNode next;

    public StackNode(T data) {
      this.data = data;
    }
  }

  private StackNode top;

  public T pop() {
    if (top == null) throw new NoSuchElementException();
    T item = top.data;
    top = top.next;

    return item;
  }

  public void push(T item) {
    StackNode t = new StackNode(item);
    t.next = top;
    top = t;
  }

  public T peek() {
    if (top == null) throw new NoSuchElementException();
    return top.data;
  }

  public boolean isEmpty() {
    return top == null;
  }
}
```

<br/>

## 스택의 사용 사례

재귀 알고리즘을 사용하는 경우 스택이 유용하다.

* 재귀 알고리즘
    * 재귀적으로 함수를 호출해야 하는 경우에 임시 데이터를 스택에 넣어준다.
    * 재귀함수를 빠져 나와 퇴각 검색(backtrack)을 할 때는 스택에 넣어 두었던 임시 데이터를 빼줘야 한다.
    * 스택은 이런 일련의 행위를 직관적으로 가능하게 해 준다.
    * 또한 스택은 재귀 알고리즘을 반복적 형태(iterative)를 통해서 구현할 수 있게 해준다.
* 웹 브라우저 방문기록(뒤로가기)
* 실행 취소(undo)
* 역순 문자열 만들기
* 수식의 괄호 검사(연산자 우선순위 표현을 위한 괄호 검사)
    * Ex) 올바른 괄호 문자열(VPS, Valid Parenthesis String) 판단하기
* 후위 표기법 계산

<br/>

## java 라이브러리 스택관련 메서드

* push(E item)
    * 해당 item을 Stack의 top에 삽입
    * Vector의 addElement(item)과 동일
* pop()
    * Stack의 top에 있는 item을 삭제하고 해당 item을 반환
* peek()
    * Stack의 top에 있는 item을 삭제하지 않고 해당 item을 반환
* empty()
    * Stack이 비어있으면 true 반환 그렇지 않으면 false를 반환
    * Stack의 top 위치는 1, 해당 Object가 없으면 -1을 반환

<br/>

**참조**
* [원본링크](https://gmlwjd9405.github.io/2018/08/03/data-structure-stack.html)