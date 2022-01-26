# 힙 정렬(Heap Sort)
완전 이진 트리를 기본으로 하는 힙(Heap) 자료구조를 기반으로한 정렬 방식

[완전 이진 트리]
* 삽입할 때 왼쪽부터 차례대로 추가하는 이진 트리이다.

힙 소트는 `불안정 정렬`에 속한다.

<br/>

## 시간복잡도
| 평균 | 최선 | 최악 |  
|:---:| :---: | :---: |  
| Θ(nlogn) | Ω(nlogn) | O(nlogn) |

<br/>

## 과정(로직)
1. 최대 힙을 구성한다.
2. 현재 힙 루트는 가장 큰 값이 존재한다. 따라서 루트의 값을 마지막 요소와 바꾼 후, 힙의 사이즈 하나를 줄인다.
3. 힙의 사이즈가 1보다 크면 위 과정을 반복한다.

![](https://t1.daumcdn.net/cfile/tistory/999896445AD4953023)

루트를 마지막 노드로 대채(11 -> 4), 그리고 다시 최대 힙 구성

![](https://t1.daumcdn.net/cfile/tistory/99E1AD445AD4953015)

이와 같은 방식으로 최대 값을 하나씩 뽑아내면서 정렬하는 것이 힙 소트이다.

<br/>

## Kotlin Code
``` kotlin
fun heapSort(array: ArrayList<Int>) {
    val n = array.size

    // max heap 초기화
    for(i in n / 2 - 1 downTo 0) {
        heapify(array, n, i) // 1.
    }

    // extract 연선
    for(i in n-1 downTo 1) {
        swap(array, 0, i)
        heapify(array, i, 0) // 2.
    }
}
```

**1번째 heapify**
* 일반 배열을 힙으로 구성하는 역할을 한다.
* 자식노드로부터 부모노드를 비교한다.
* n/2 - 1부터 0까지 인덱스가 도는 이유는 부모 노드의 인덱스를 기준으로 왼쪽 자식노드(i * 2 + 1), 오른쪽 자식노드(i * 2 + 2)이기 때문이다.

<br/>

**2번째 heapify**
* 요소가 하나 제거된 이후에 다시 최대 힙을 구성하기 위함이다.
* 루트를 기준으로 진행한다(extract 연산 처리를 위해).

``` kotlin
fun heapify(array: ArrayList<Int>, n: Int, i: Int) {
    var p = i
    var l = i * 2 + 1
    var r = i * 2 + 2

    // 왼쪽 자식노드
    if(l < n && array[p] < array[l]) {
        p = l
    }
    // 오른쪽 자식노드
    if(r < n && array[p] < array[r]) {
        p = r
    }

    // 부모노드 < 자식노드
    if(i != p) {
        swap(array, p, i)
        heapify(array, n, p)
    }
}
```

* **다시 최대 힙을 구성할 때까지** 부모 노드와 자식 노드를 swap하며 재귀를 진행한다.
* 퀵정렬과 합병정렬의 성능이 좋기 때문에 힙 정렬의 사용빈도가 높지는 않다.
* 하지만 힙 자료구조가 많이 활용되고 있으며, 이때 함께 따라오는 개념이 `힙 소트`이다.
* **힙 소트가 유용할 때**
  * 가장 크거나 가장 작은 값을 구할 때
  <br/>
  최소 힙 or 최대 힙의 루트 값이기 때문에 한번의 힙 구성을 통해 구하는 것이 가능하다.
  * 최대 k 만큼 떨어진 요소들을 정렬할 때
  <br/>
  삽입정렬보다 더욱 개선된 결과를 얻어낼 수 있다.

<br/>

## 전체 소스 코드
``` kotlin
fun heapSort(array: ArrayList<Int>) {
    val n = array.size

    // max heap 초기화
    for(i in n/2 - 1 downTo 0) {
        heapify(array, n, i) // 1.
    }

    // extract 연선
    for(i in n-1 downTo 1) {
        swap(array, 0, i)
        heapify(array, i, 0)
    }
}

fun heapify(array: ArrayList<Int>, n: Int, i: Int) {
    var p = i
    var l = i * 2 + 1
    var r = i * 2 + 2

    // 왼쪽 자식노드
    if(l < n && array[p] < array[l]) {
        p = l
    }
    // 오른쪽 자식노드
    if(r < n && array[p] < array[r]) {
        p = r
    }

    // 부모노드 < 자식노드
    if(i != p) {
        swap(array, p, i)
        heapify(array, n, p)
    }
}

fun swap(array: ArrayList<Int>, a: Int, b: Int) {
    val temp = array[a]
    array[a] = array[b]
    array[b] = temp
}

fun main() {
    val array = arrayListOf(230, 10, 60, 550, 40, 220, 20)
    heapSort(array)

    array.forEach {
        println(it)
    }
}
```

<br/>

**[참조]**
* [원본링크](https://gyoogle.dev/blog/algorithm/Heap%20Sort.html)