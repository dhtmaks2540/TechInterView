# Binary Heap
자료구조의 일종으로 독특한 이름과 달리 Tree의 형식을 하고 있으며, Tree 중에서도 배열에 기반한 `Complete Binary Tree`이다. 배열에 트리의 값을 넣어줄 때, 0 번째는 건너뛰고 1 번 index부터 루트노드가 시작된다. 이는 노드의 고유번호 값과 배열의 index를 일치시켜 혼동을 줄이기 위함이며 계산의 편리함 때문이다. `힙(Heap)`에는 `최대힙(max heap)`, `최소힙(min heap)` 두 종류가 있다.

`Max Heap`이란, 각 노드의 값이 해당 children의 값보다 **크거나 같은** `complete binary tree`를 말한다(Min Heap은 그 반대로 각 노드의 값이 해당 children의 값보다 **작거나 같다**).

`Min Heap`에서는 Root node에 있는 값이 제일 작으므로, 최소값을 찾는데 소요되는 연산의 time complexity이 O(1)이다. 그리고 `complete binary tree`이기 때문에 배열을 사용하여 효율적으로 관리할 수 있다(즉, random access가 가능하다. 반대로 Max Heap에서는 최대값을 찾는데 소요되는 연산의 time complexity가 O(1)이다). 하지만 heap의 구조를 계속 유지하기 위해서는 제거된 루트 노드를 대체할 다른 노드가 필요하다. 여기서 heap은 맨 마지막 노드를 루트 노드로 대체시킨 후, 다시 heapify 과정을 거쳐 heap 구조를 유지한다. 이런 경우에는 결국 O(log n)의 시간복잡도로 최소값 또는 최대값에 접근할 수 있게 된다.

<br/>

* 힙은 정렬된 구조가 아니라는 점이 있다. 최소 힙의 경우 부모 노드가 항상 작다는 조건만 만족할 뿐, 서로 정렬되어 있지 않다. 예를 들어 오른쪽 자식 노드가 레벨 차이에도 불구하고, 왼쪽 노드보다 더 작은 경우도 얼마든지 있을 수 있다. 부모, 자식 간의 관계만 정의할 뿐, 좌우에 대한 관계는 정의하지 않기 때문이다. -> 즉, 이진 탐색 트리같은 구조가 아니다!!

* 이진 힙과 이진 탐색 트리의 차이점을 이야기하자면 힙은 상/하 관계를 보장하고 BST는 좌/우 관계를 보장한다. 이 같은 특징으로 인해 BST는 탐색과 삽입 모두 O(log n)에 가능하며, 모든 값이 정렬되어야 할 때 사용한다. 반면 가장 큰 값을 추출하거나(최대 힙) 가장 작은 값을 추출하려면(최소 힙) 이진 힙을 사용해야 한다. 이진 힙은 이 작업이 O(1)에 가능하다. 우선순위와 연관되어 있으며 따라서 이진 힙은 우선순위 큐에 활용된다.

* 우선순위 큐 ADT(추상 자료형)은 주로 힙으로 구현하기에 우선순위 큐에서 가장 작은 값을 추출하는 것은 매번 힙의 루트를 가져오는 형태로 구현한다(최소힙이라고 가정).

<br/>

## 힙 구현
``` python
# 배열을 이용하여 최소힙 구현
class BinaryHeap(object):
    def __init__(self):
        self.items = [None]

    def __len__(self):
        # 마지막 요소를 가리키게 하기 위해서
        return len(self.items) - 1

    # 삽입 시 실행, 반복 구조 구현
    def _percolate_up(self):
        # 삽입된 원소
        i = len(self)
        # 부모 노드
        parent = i // 2
        # 들어온 값이 부모보다 작으면 swap
        while parent > 0:
            if self.items[i] < self.items[parent]:
                self.items[parent], self.items[i] = \
                    self.items[i], self.items[parent]
            # 부모 노드로 인덱스 갱신
            i = parent
            # 부모 인덱스 갱신
            parent = i // 2

    def insert(self, k):
        # 배열의 제일 끝에 원소 삽입 -> Heap에서는 제일 왼쪽에 원소 삽입
        self.items.append(k)
        self._percolate_up()

    # 추출시 실행, 재귀 구조 구현
    def _percolate_down(self, idx):
        # 왼쪽, 오른쪽 인덱스
        left = idx * 2
        right = idx * 2 + 1
        smallest = idx

        if left <= len(self) and self.items[left] < self.items[smallest]:
            smallest = left

        if right <= len(self) and self.items[right] < self.items[smallest]:
            smallest = right
        
        # 자식 노드에 더 작은 값이 있다면
        if smallest != idx:
            self.items[idx], self.items[smallest] = \
                self.items[smallest], self.items[idx]
            self._percolate_down(smallest)

    def extract(self):
        # 추출은 부모 노드를 추출한다.
        extracted = self.items[1]
        # 가장 마지막 노드를 부모 노드로 삽입
        self.items[1] = self.items[len(self)]
        # 가장 마지막 노드 삭제
        self.items.pop()
        self._percolate_down(1)
        return extracted
```