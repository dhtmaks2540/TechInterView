# 병합 정렬(Merge Sort)

합병 정렬이라고도 부르며, 분할 정복 방법을 통해 구현

**_분할 정복이란?_**
> `큰 문제를 작은 문제 단위로 쪼개면서 해결해나가는 방식`

<br/>

## 개념
* 빠른 정렬로 분류되며, 퀵소트와 함께 많이 언급되는 정렬 방식이다.
* 퀵소트와는 반대로 `안정 정렬`에 속함

요소를 쪼갠 후, 다시 합병시키면서 정렬해나가는 방식으로, 쪼개는 방식은 퀵정렬과 유사

<br/>

## Kotlin Code
### mergeSort
``` kotlin
fun mergeSort(array: ArrayList<Int>, left: Int, right: Int)
{
    if(left < right) {
        val mid = (left + right) / 2

        mergeSort(array, left, mid) // 왼쪽 부분 배열
        mergeSort(array, mid + 1, right) // 오른쪽 부분 배열
        // 나눈 배열 합치기
        // 핵심 로직에 해당
        merge(array, left, mid, right)
    }
}
```

_퀵 소트와의 차이점_
> 퀵정렬 : 우선 피벗을 통해 정렬(partition) -> 영역을 쪼갬(quickSort)
<br/>
합병정렬 : 영역을 쪼갤 수 있을 만큼 쪼갬(mergeSort) -> 정렬(merge)

<br/>

### merge - version 1
``` kotlin
fun merge(array: ArrayList<Int>, left: Int, mid: Int, right: Int)
{
    val L = ArrayList<Int>()
    L.addAll(array.slice(left until mid + 1))

    val R = ArrayList<Int>()
    R.addAll(array.slice(mid+1 until right + 1))

    var (i, j, k) = Triple(0, 0, left)
    val (l1, r1) = Pair(L.size, R.size)

    while(i < l1 && j < r1) {
        if(L[i] <= R[j]) {
            array[k] = L[i++]
        } else {
            array[k] = R[j++]
        }
        k++
    }

    // remain
    while(i < l1) {
        array[k++] = L[i++]
    }
    while(j < r1) {
        array[k++] = R[j++]
    }
}
```

<br/>

*  **합병의 대상이 되는 두 영역이 각 영역에 대해서 정렬이 되어있기 때문**에 단순히 두 배열을 **순차적으로 비교하면서 정렬할 수가 있다.**

* ★★★ **합병정렬은 순차적**인 비교로 정렬을 진행하므로, **LinkedList의 정렬이 필요할 때 사용하면 효율적**이다. ★★★

* LinkedList를 퀵정렬 사용해 정렬하면 성능이 좋지 않다. 
그 이유는 퀵정렬은, 순차 접근이 아닌 임의 접근이기 때문

* **LinkedList는 삽입, 삭제 연산에서 유용**하지만 **접근 연산에서는 비효율적**이다.
따라서 임의로 접근하는 퀵소트를 활용하면 오버헤드 발생이 증가하게 됨

> 배열은 인덱스를 이용해서 접근이 가능하지만, LinkedList는 Head부터 탐색해야 한다.
<br/>
배열 : O(1)
<br/>
LinkedList : O(n)

<br/>

## 시간복잡도
| 평균 | 최선 | 최악 |  
|:---:| :---: | :---: |  
| Θ(nlogn) | Ω(nlogn) | O(nlogn) |

<br/>

## 장점
* 데이터의 분포에 영향을 덜 받는다. 즉, 입력 데이터가 무엇이든 간에 정렬되는 시간은 동일하다. -> O(nlogn)
* 크기가 큰 레코드를 정렬한 경우, LinkedList를 사용한다면 병합 정렬은 퀵 정렬을 포함한 다른 어떤 정렬 방법보다 효율적이다.
* 안정 정렬에 속한다.

<br/>

## 단점
* 레코드를 배열로 구성한다면, 임시 배열이 필요하다.
  * 메모리 낭비를 초래한다.
  * 제자리 정렬이 아니다.
* 레코드의 크기가 큰 경우에는 이동 횟수가 많으므로 매우 큰 시간적 낭비를 초래한다.

<br/>

**[참조]**
* [원본링크](https://gyoogle.dev/blog/algorithm/Heap%20Sort.html)