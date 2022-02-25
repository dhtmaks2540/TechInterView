# Red Black Tree

RBT(Red-Black Tree)는 BST(Binary Search Tree)를 기반으로하는 트리 형식의 자료 구조이다. 결론부터 말하자면 Red-Black Tree 에 데이터를 저장하게되면 Search, Insert, Delete 에 O(lon n)의 시간복잡도가 소요된다. 동일한 노드의 개수일 때, depth를 최소화하여 시간 복잡도를 줄이는 것이 핵심 아이디어이다. 동일한 노드의 개수일 때, depth가 최소가 되는 경우는 tree가 complete binary tree인 경우이다.

<br/>

## Red-Black Tree의 정의

Red-Black Tree는 다음의 성질들을 만족하는 BST이다.

1. 각 노드는 `Red` or `Black` 이라는 색깔을 갖는다.
2. Root node의 색깔은 `Black`이다.
3. 각 leaf node는 `Black`이다.
4. 어떤 노드의 색깔이 `red`라면 두 개의 children의 색깔은 모두 `black`이다. 즉, red node가 연속으로 올 수 없다.
5. 각 노드에 대해서 노드로부터 descendant leaves 까지의 단순 경로는 모두 같은 수의 black nodes 들을 포함하고 있다. 이를 해당 노드의 `Black-Height`라고 한다. cf) Black-Height : 노드 x로부터 노드 x를 포함하지 않은 leaf node 까지의 simple path 상에 있는 black nodes 들의 개수

![](https://upload.wikimedia.org/wikipedia/commons/thumb/6/66/Red-black_tree_example.svg/1024px-Red-black_tree_example.svg.png)

<br/>

## Red-Black Tree의 특징

1. Binary Search Tree 이므로 BST의 특징을 모두 갖는다.
2. Root node 부터 leaf node 까지의 모든 경로 중 최소 경로와 최대 경로의 크기 비율은 2보다 크지 않다. 이러한 상태를 `balanced` 상태라고 한다.
3. 노드의 child 가 없을 경우 child 를 가리키는 포인터는 NIL 값을 저장한다. 이러한 NIL 들을 leaf node 로 간주한다.

> RBT는 BST 의 삽입, 삭제 연산 과정에서 발생할 수 있는 문제점(한쪽으로 몰리는 편향 트리가 될 수 있음)을 해결하기 위해 만들어진 자료구조이다. 이를 어떻게 해결할 것인가?

<br/>

## 검색

Red-Black Tree에서의 검색은 일반 BST에서의 검색과 동일하다. 만약 parent node보다 찾으려는 숫자가 작다면 왼쪽 child node로 이동해주고, 숫자가 크다면 오른쪽 child node로 이동해주면 된다.

<br/>

## 삽입

우선 BST의 특성을 유지하면서 노드를 삽입을 한다. 그리고 삽입된 노드의 색깔을 **RED** 로 지정한다. Red 로 지정하는 이유는 Black-Height 변경을 최소화하기 위함이다. 삽입 결과 RBT 의 특성 위배(violation)시 노드의 색깔을 조정하고, Black-Height 가 위배되었다면 rotation 을 통해 height 를 조정한다. 이러한 과정을 통해 RBT 의 동일한 height 에 존재하는 internal node 들의 Black-height 가 같아지게 되고 최소 경로와 최대 경로의 크기 비율이 2 미만으로 유지된다.

<br/>

## 삭제

삭제도 삽입과 마찬가지로 BST 의 특성을 유지하면서 해당 노드를 삭제한다. 삭제될 노드의 child 의 개수에 따라 rotation 방법이 달라지게 된다. 그리고 만약 지워진 노드의 색깔이 Black 이라면 Black-Height 가 1 감소한 경로에 Black node 가 1개 추가되도록 rotation 하고 노드의 색깔을 조정한다. 지워진 노드의 색깔이 red 라면 Violation 이 발새하지 않으므로 RBT 가 그대로 유지된다.

Java COllection 에서 ArrayList 도 내부적으로 RBT로 이루어져 있고, HashMap 에서의 `Separate Chaining`에서도 사용된다. 그만큼 효율이 좋고 중요한 자료구조 이다.

<br/>

**참조**

* [원본링크](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#red-black-tree)
* [Red-Black Tree 속성/검색/삽입/삭제](https://lemonlemon.tistory.com/135)
* [알고리즘 Red-Black Tree](https://zeddios.tistory.com/237)