# DFS & BFS
* 그래프 알고리즘으로, 문제를 풀 때 상당히 많이 사용한다.
* 경로를 찾는 문제 시, 상황에 맞게 DFS와 BFS를 활용하면 된다.

<br/>

## DFS(Depth-First-Search)
* 루트 노드 혹은 임의 노드에서 **다음 브랜치로 넘어가기 전에, 해당 브랜치를 모두 탐색**하는 방법

* **스택 or 재귀함수**를 통해 구현한다.

* 모든 경로를 방문해야 할 경우 사용에 적합하다.

![](https://upload.wikimedia.org/wikipedia/commons/7/7f/Depth-First-Search.gif)

<br/>

### 동작과정
스택 자료구조를 이용한 구체적인 동작 과정은 아래와 같다.

1. 탐색 시작 노드를 스택에 삽입하고 방문 처리를 한다.
2. 스택의 최상단 노드에 방문하지 않은 인접 노드가 있다면 그 인접 노드를 스택에 넣고 방문 처리를 한다. 
<br/>
방문하지 않은 인접 노드가 없으면 스택에서 최상단 노드를 꺼낸다.
3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다.

<br/>

### 시간 복잡도
* 인접 행렬 : O(V^2)
* 인접 리스트 : O(V + E)

> V는 접점(노드), E는 간선을 뜻한다.

<br/>

### Python Code - 스택
``` python
# 인접 리스트
graph = [
    [],
    [2,5,9],
    [3],
    [4],
    [],
    [6,8],
    [7],
    [],
    [],
    [10],
    []
]

# 스택은 재귀와 똑같이 DFS이지만 pop이 원소의 뒤에서부터 출력하므로 순서가 뒤에서부터
def iterative_dfs(graph, start_v):
    # 결과 변수
    discovered = []
    # 리스트 선언(스택)
    stack = [start_v]
    # 스택이 비어있지 않는 동안에
    while stack:
        v = stack.pop()
        # 방문한적이 없다면
        if v not in discovered:
            discovered.append(v)
            for w in graph[v]:
                stack.append(w)

    return discovered
```

### Python Code - 재귀
``` python
def recursive_dfs(graph, v, discovered=[]):
    discovered.append(v)
    for w in graph[v]:
        # 방문한적이 없다면
        if w not in discovered:
            # 연결된 노드 재귀 호출
            discovered = recursive_dfs(graph, w, discovered)

    return discovered
```

### Kotlin Code - 스택
``` kotlin
fun iterativeDfs(graph: Array<Array<Int>>, start_v: Int): ArrayList<Int> {
    // 결과 변수
    val discovered = ArrayList<Int>()
    // 스택
    val stack = Stack<Int>()
    // 시작값 넣기
    stack.add(start_v)
    
    // 스택이 비어있지 않는 동안에
    while(stack.isNotEmpty()) {
        val v = stack.pop()
        // 방문한적이 없다면
        if(v !in discovered) {
            discovered.add(v)
            for(w in graph[v]) {
                stack.add(w)
            }
        }
    }

    return discovered
}
```

### Kotlin Code - 재귀
``` kotlin
// 재할당을 위해서 전역 변수로 설정
var discovered: ArrayList<Int> = ArrayList()

fun recursiveDfs(graph: Array<Array<Int>>, v: Int): ArrayList<Int> {
    discovered.add(v)
    for(w in graph[v]) {
        // 방문한적이 없다면
        if(w !in discovered) {
            // 연결된 노드 재귀 호출
            discovered = recursiveDfs(graph, w)
        }
    }

    return discovered
}
```

<br/>

## BFS(Breadth-First-Search)
* 루트 노드 또는 임의 노드에서 **인접한 노드부터 먼저 탐색**하는 방법
* **큐**를 통해 구현한다.(해당 노드의 주변부터 탐색해야하기 떄문)
* 최소 비용(즉, 모든 곳을 탐색하는 것보다 최소 비용이 우선일 때)에 적합하다.

![](https://upload.wikimedia.org/wikipedia/commons/5/5d/Breadth-First-Search-Algorithm.gif)

<br/>

## 동작 방식
BFS 구현에서는 선입선출(First In FIrst Out) 방식인 **큐 자료구조**를 이용하는 것이 정석이다.
<br/>
인접한 노드를 반복적으로 큐에 넣도록 알고리즘을 작성하면 자연스럽게 먼저 들어온 것이 먼저 나가게 되어, 가까운 노드부터 탐색을 진행하게 된다.

1. 탐색 시작 노드를 큐에 삽입하고 방문 처리를 한다.
2. 큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리를 한다.
3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다.

<br/>

## 시간 복잡도
* 인접 행렬 : O(V^2)
* 인접 리스트 : O(V+E)

<br/>

## Python Code
``` python
graph = [
    [],
    [2, 3, 4],
    [5],
    [6,7],
    [8],
    [9],
    [10],
    [],
    [],
    [],
    []
]

# 큐를 구현하기 위한 deque 라이브러리 사용
from collections import deque

def bfs(graph, start):
    # 시작 노드로 deque 선언
    queue = deque([start])
    # 현재 노드 방문 처리
    discovered = [start]

    # 큐가 비어있지 않는 동안에
    while queue:
        # 큐에서 원소 뽑기
        v = queue.popleft()
        # 해당 노드와 연결된, 아직 방문하지 않은 노드들을 큐에 삽입
        for w in graph[v]:
            if w not in discovered:
                discovered.append(w)
                queue.append(w)

    return discovered
```

### Kotlin Code
``` kotlin
fun bfs(graph: Array<Array<Int>>, start_v: Int): ArrayList<Int> {
    // 코틀린도 Deque가 존재
    // O(1)의 시간안에 원소의 삭제, 추가를 하기 위해 Deque 사용
     /* queue 선언법
     val queue: Queue<Int> = LinkedList<Int>() */
    
    // deque 선언
    val queue: Deque<Int> = ArrayDeque()
    // 시작 노드 방문 처리 및 큐에 삽입
    queue.add(start_v)
    val discovered = arrayListOf(start_v)
    
    // 큐가 비어있지 않는 동안에
    while(queue.isNotEmpty()) {
        val v = queue.pollFirst()
        for(w in graph[v]) {
            // 해당 노드와 연결된, 아직 방문하지 않는 노드들을 큐에 삽입
            if(w !in discovered) {
                discovered.add(w)
                queue.add(w)
            }
        }
    }

    return discovered
}
```

<br/>

## 정리
| | DFS | BFS |
|:---:|:---:|:---:|
| 동작원리 | 스택 또는 재귀 | 큐 |
| 구현방법 | 스택 또는 재귀 함수 이용 | 큐 자료구조 이용|

<br/>

__[참조]__
* [원본링크](https://gyoogle.dev/blog/algorithm/DFS%20&%20BFS.html)
* [DFS(깊이 우선 탐색)](https://velog.io/@changhee09/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-DFS%EA%B9%8A%EC%9D%B4-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89)
* [BFS(너비 우선 탐색)](https://velog.io/@changhee09/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-BFS%EB%84%88%EB%B9%84-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89)