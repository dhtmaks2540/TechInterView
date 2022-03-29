# 트리(Tree)란

## Goal

* 트리의 기본 개념을 이해한다.
* 트리의 종류를 구분할 수 있다.
* 트리의 특징을 이해할 수 있다.

<br/>

## 트리의 개념

트리는 **노드로 이루어진 비선형 자료구조** 이다. 트리는 계층적 관계(Hierarchical Relationship)을 표현하는 자료구조이다.

1. 트리는 하나의 루트 노드를 갖는다.
2. 루트 노드는 0개 이상의 자식 노드를 갖고 있다.
3. 그 자식 노드 또한 0개 이상의 자식 노드를 갖고 있고, 이는 반복적으로 정의된다. 즉, 재귀로 정의된(Recursively Defined) 자기 참조(Self-Referential) 자료구조이다. 쉽게 말하자면, 트리는 자식도 트리고 또 그 자식도 트리이다(흔히 서브트리로 구성된다고 표현한다).

* 노드(node)들과 노드들을 연결하는 간선(edge)들로 구성되어 있다.
    * 트리에는 사이클(cycle)이 존재할 수 없다.
    * 노드들은 특정 순서로 나열될 수도 있고 그럴 수 없을 수도 있다.
    * 각 노드는 부모 노드로의 연결이 있을 수도 있고 없을 수도 있다.
    * 각 노드는 어떤 자료형으로도 표현 가능하다.

    ``` java
    class Node {
    public String name;
    public Node[] children;
    }
    ```
* 비선형 자료구조로 계층적 관계를 표현한다. Ex) 디렉터리 구조, 조직도
* 그래프의 한 종류
    * **사이클(cycle)이 없는 하나의 연결 그래프(Connected Graph)**. 즉, 트리는 순환 구조를 갖지 않는 그래프이다.
    * 또는 **DAG(Directed Acyclic Graph, 방향성이 있는 비순환 그래프)의 한 종류** 이다. 즉, 부모 노드에서 자식 노드를 가리키는 단방향뿐이며, 자식 노드는 하나의 부모 노드를 갖는다.

<br/>

## 트리와 관련된 용어

![](https://gmlwjd9405.github.io/images/data-structure-tree/tree-terms.png)

* 노드(node): 트리를 구성하고 있는 각각의 요소
* 루트 노드(root node): 부모가 없는 노드, 트리는 하나의 루트 노드만을 가진다.
* 단말 노드(leaf node = terminal node): 자식이 없는 노드, '말단 노드' 또는 '잎 노드' 라고도 부른다.
* 내부(internal) 노드: 단말 노드가 아닌 노드
* 간선(edge): 노드를 연결하는 선 (line, branch 라고도 부름)
* 형제(sibling): 같은 부모를 가지는 노드
* 노드의 크기(size): 자신을 포함한 모든 자손 노드의 개수
* 노드의 레벨(level): 트리의 특정 깊이를 가지는 노드의 집합
* 노드의 차수(degree): 하위 트리 개수 / 간선 수(degree) = 각 노드가 지닌 가지의 수
* 트리의 차수(degree of tree): 트리의 최대 차수
* 트리의 높이(height): 루트 노드에서 가장 깊숙히 있는 노드의 깊이

<br/>

## 트리의 특징

* 그래프의 한 종류이다. **'최소 연결 트리'** 라고도 불린다.
* 트리는 **계층 모델** 이다.
* 트리는 DAG(Directed Acyclic Graphs, 방향성이 있는 비순환 그래프)의 한 종류이다.
    * loop나 circuit이 없다. 당연히 self-loop도 없다.
    * 즉, 사이클이 없다.
* 노드가 N개인 트리는 항상 N-1개의 간선(edge)을 가진다.
    * 즉, 간선은 항상 (정점의 개수 - 1) 만큼을 가진다.
* 루트에서 어떤 노드로 가는 경로는 유일하다.
    * 임의의 두 노드 간의 경로도 유일하다. 즉, 두 개의 정점 사이에 반드시 1개의 경로만을 가진다.
* 한 개의 루트 노드만이 존재하며 모든 자식 노드는 한 개의 부모 노드 만을 가진다.
    * 부모-자식 관계이므로 흐름은 top-bottom 아니면 bottom-top으로 이루어진다.
* 순회는 Pre-order, In-order 아니면 Post-order로 이루어진다. 이 3가지 모두 DFS/BFS 안에 있다.
* 트리는 이진 트리, 이진 탐색 트리, 균형 트리(AVL 트리, red-black 트리), 이진 힙(최대힙, 최소힙) 등이 있다.

<br/>

## 트리의 종류

### 트리 vs 이진 트리

* 이진 트리(Binary Tree)
    * 각 노드가 최대 두 개의 자식을 갖는 트리
    * 모든 트리가 이진 트리는 아니다.
    * 이진 트리 순회
        * 중위 순회(in-order traversal): 왼쪽 가지 -> 현재 노드 -> 오른쪽 가지
        ``` java
        void inOrderTraversal(TreeNode node) {
          if(node != null) {
            inOrderTraversal(node.left);
            visit(node);
            inOrderTraversal(node.right);
          }
        }
        ```

        * 전위 순회(pre-order traversal): 현재 노드 -> 왼쪽 가지 -> 오른쪽 가지

        ``` java
        void preOrderTraversal(TreeNode node) {
          if(node != null) {
            visit(node);
            preOrderTraversal(node.left);
            preOrderTraversal(node.right);
          }
        }
        ```

        * 후위 순회(post-order traversal): 왼쪽 가지 -> 오른쪽 가지 -> 현재 노드

        ``` java
        void postOrderTraversal(TreeNode node) {
          if(node != null) {
            postOrderTraversal(node.left);
            postOrderTraversal(node.right);
            visit(node);
          }
        }
        ```

<br/>

### 이진 트리 VS 이진 탐색 트리

* 이진 탐색 트리(Binary Search Tree)
    * 모든 노드가 아래와 같은 특성을 따르는 속성이 있는 이진 트리
    * 모든 왼쪽 자식들 <= n < 모든 오른쪽 자식들 (모든 노드 n에 대해서 반드시 참)

<br/>

### 균현 트리 VS 비균형 트리

* 균형 트리
    * O(logN) 시간에 insert와 find를 할 수 있을 정도로 균형이 잘 잡혀 있는 경우
    * Ex) 레드-블랙 트리, AVL 트리

<br/>

### 완전 이진 트리 VS 전 이진 트리 VS 포화 이진 트리

![](https://gmlwjd9405.github.io/images/data-structure-tree/tree-types-example.png)

* 완전 이진 트리(Complete Binary Tree)
    * ![](https://gmlwjd9405.github.io/images/data-structure-tree/Complete-Binary-Tree.png)
    * 트리의 모든 높이에서 노드가 꽉 차 있는 이진 트리. 즉, 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져 있다.
    * 마지막 레벨은 꽉 차 있지 않아도 되지만 노드가 왼쪽부터 채워져야 한다.
    * 마지막 레벨 h에서 (1-2h-1)개의 노드를 가질 수 있다.
    * 또 다른 정의는 가장 오른쪽의 잎 노드가 (아마도 모두) 제거된 포화 이진 트리다.
    * 완전 이진 트리는 배열을 사용해 효율적으로 표현이 가능하다.
* 전 이진 트리(Full Binary Tree 또는 Strictly Binary Tree)
    * ![](https://gmlwjd9405.github.io/images/data-structure-tree/Full-Binary-Tree.png)
    * 모든 노드가 0개 또는 2개의 자식 노드를 갖는 트리
* 포화 이진 트리(Perfect Binary Tree)
    * ![](https://gmlwjd9405.github.io/images/data-structure-tree/Perfect-Binary-Tree.png)
    * 전 이진 트리이면서 완전 이진 트리인 경우
    * 모든 말단 노드는 같은 높이에 있어야 하며, 마지막 단계에서 노드의 개수가 최대가 되어야 한다.
    * 모든 내부 노드가 두 개의 자식 노드를 가진다.
    * 모든 말단 노드가 동일한 깊이 또는 레벨을 갖는다.
    * 노드의 개수가 정확히 2^k - 1 개여야 한다. (k: 트리의 높이) -> 흔하게 나타나는 경우가 아니다. 즉, 이진 트리가 모두 포화 이진 트리일 것이라고 생각하지 않는다.

<br/>

### 이진 힙(최소힙과 최대힙)

* 최소힙(Min Heap)
    * 트리의 마지막 단계에서 오른쪽 부분을 뺀 나머지 부분이 가득 채워져 있는 완전 이진 트리이며, 각 노드의 원소가 자식들의 원소보다 작다.
    * 즉, Key(부모 노드) <= Key(자식 노드)인 완전 이진 트리
    * 가장 작은 값은 루트 노드이다.
    * N개가 힙에 들어가 있으면 높이는 log(N)이다.
* 최대힙(Max Heap)
    * 원소가 내림차순으로 정렬되어 있다는 점에서만 최소힙과 다르다.
    * 각 노드의 원소가 자식들의 원소보다 크다.

<br/>

### 트라이(trie)('접두사 트리(Prefix Tree)' 라고도 부른다)

* 트라이
    * n-차 트리(n-ary Tree)의 변종
    * 각 노드에 문자를 저장하는 자료구조
    * 따라서 트리를 아래쪽으로 순회하면 단어 하나가 나온다.
    * **접두사를 빠르게 찾아보기 위한 흔한 방식** 으로, 모든 언어를 트라이에 저장해 놓는 방식이 있다.
    * **유효한 단어 집합을 이용** 하는 많은 문제들은 트라이를 통해 최적화할 수 있다.

<br/>

## 트리의 구현 방법

기본적으로 트리는 그래프의 한 종류이므로 그래프의 구현 방법(인접 리스트 또는 인접 배열)으로 구현할 수 있다.

### 인접 배열 이용

1. 1차원 배열에 자신의 부모 노드만 저장하는 방법
    * 트리는 부모 노드를 0개 또는 1개를 가지기 때문
    * 부모 노드를 0개: 루트 노드
2. 이진 트리의 경우, 2차원 배열에 자식 노드를 저장하는 방법
    * 이진 트리는 각 노드가 최대 두 개의 자식을 갖는 트리이기 때문
    * Ex) A[i][0]: 왼쪽 자식 노드, A[i][1]: 오른쪽 자식 노드

<br/>

### 인접 리스트 이용

1. 가중치가 없는 트리의 경우
    * ArrayList<ArrayList> list = new ArrayList<>();
2. 가중치가 있는 트리의 경우
    * 1) class Node { int num, dist; // 노드 번호, 거리 } 정의
    * 2) ArrayList[] lit = new ArrayList[정점의 수 + 1];

**그래프와 트리의 차이**

![](https://gmlwjd9405.github.io/images/data-structure-graph/graph-vs-tree.png)

<br/>

# 추가 내용

## Binary Tree

루트 노드를 중심으로 두 개의 서브 트리(큰 트리에 속하는 작은 트리)로 나뉘어 진다. 또한 나뉘어진 두 서브 트리도 모두 이진 트리어야 한다. 재귀적인 정의라 맞는듯 하면서도 이해가 쉽지 않을 듯하다. 한 가지 덧붙이자면 공집합도 이진 트리로 포함시켜야 한다. 그래야 재귀적으로 조건을 확인해갔을 때, leaf node 에 다다랐을 때, 정의가 만족되기 때문이다. 자연스럽게 노드가 하나 뿐인 것도 이진 트리 정의에 만족하게 된다.

트리에서는 각 `층별로` 숫자를 매겨서 이를 트리의 `Level(레벨)`이라고 한다. 레벨의 값은 0 부터 시작하고 따라서 루트 노드의 레벨은 0 이다. 그리고 트리의 최고 레벨을 가리켜 해당 트리의 `height(높이)`라고 한다.

#### Perfect Binary Tree (포화 이진 트리), Complete Binary Tree (완전 이진 트리), Full Binary Tree (정 이진 트리)

모든 레벨이 꽉 찬 이진 트리를 가리켜 포화 이진 트리라고 한다. 위에서 아래로, 왼쪽에서 오른쪽으로 순서대로 차곡차곡 채워진 이진 트리를 가리켜 완전 이진 트리라고 한다. 모든 노드가 0개 혹은 2개의 자식 노드만을 갖는 이진 트리를 가리켜 정 이진 트리라고 한다. 배열로 구성된 `Binary Tree`는 노드의 개수가 n 개이고 root가 0이 아닌 1에서 시작할 때, i 번째 노드에 대해서 parent(i) = i/2 , left_child(i) = 2i , right_child(i) = 2i + 1 의 index 값을 갖는다.

<br/>

### BST(Binary Search Tree)

효율적인 탐색을 위해서는 어떻게 찾을까만 고민해서는 안된다. 그보다는 효율적인 탐색을 위한 저장방법이 무엇일까를 고민해야 한다. 이진 탐색 트리는 이진 트리의 일종이다. 단 이진 탐색 트리에는 데이터를 저장하는 규칙이 있다. 그리고 그 규칙은 특정 데이터의 위치를 찾는데 사용할 수 있다.

* 규칙 1. 이진 탐색 트리의 노드에 저장된 키는 유일하다.
* 규칙 2. 부모의 키가 왼쪽 자식 노드의 키보다 크다.
* 규칙 3. 부모의 키가 오른쪽 자식 노드의 키보다 작다.
* 규칙 4. 왼쪽과 오른쪽 서브트리도 이진 탐색 트리이다.

이진 탐색 트리의 탐색 연산은 O(log n)의 시간 복잡도를 갖는다. 사실 정확히 말하면 O(h)라고 표현하는 것이 맞다. 트리의 높이를 하나씩 더해갈수록 추가할 수 있는 노드의 수가 두 배씩 증가하기 때문이다. 하지만 이러한 이진 탐색 트리는 Skewed Tree(편향 트리)가 될 수 있다. 저장 순서에 따라 계속 한 쪽으로만 노드가 추가되는 경우가 발생하기 때문이다. 이럴 경우 성능에 영향을 미치게 되며, 탐색의 Worst Case 가 되고 시간 복잡도는 O(n)이 된다.

배열보다 많은 메모리를 사용하며 데이터를 저장했지만 탐색에 필요한 시간 복잡도가 같게 되는 비효율적인 상황이 발생한다. 이를 해결하기 위해 `Rebalancing` 기법이 등장하였다. 균형을 잡기 위한 트리 구조의 재조정을 `Rebalancing`이라 한다. 이 기법을 구현한 트리에는 여러 종류가 존재하는데 그 중에서 하나가 뒤에서 살펴볼 `Red-Black Tree`이다.

<br/>

#### Personal Recommendation

* Binary Search Tree 구현하기
* 주어진 트리가 Binary 트리인지 확인하는 알고리즘 구현하기

<br/>

**참조**
* [원본링크](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#tree)
* [원본링크2](https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html)
* [참조](https://velog.io/@changhee09/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%8A%B8%EB%A6%AC)
