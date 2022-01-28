# 계수 정렬(Counting Sort)

## Comparison Sort
> N개의 원소의 배열이 있을 때, 이를 모두 정렬하는 가짓수는 N! 이다.
<br/>
따라서, Comparison Sort를 통해 생기는 트리의 말단 노드가 N! 이상의 노드 갯수를 갖기 위해서는, 
2^h >= N! 를 만족하는 h를 가져야 하고,
이 식은 h > O(nlogn)을 가져야 한다.(h는 트리의 높이, 즉 Comparison Sort의 시간 복잡도)

이런 O(nlogn)을 줄일 수 있는 방법은 Comparison을 하지 않는 것이다.

<br/>

## Counting Sort 과정
* 시간 복잡도 : O(n + k) -> k는 배열에서 등장하는 최대값이다.
* 공간 복잡도 : O(k) -> k만큼의 배열을 만들어야 한다.
* Counting이 필요 : 각 숫자가 몇 번 등장했는지 갯수를 나타낸다.

<br/>

## Code
### Kotlin
``` kotlin
fun countingSort(arr: ArrayList<Int>): MutableList<Int> {
    // 리스트의 최댓값
    val maxNum = arr.maxOrNull()!!
    // 갯수를 체크하는 count(리스트의 최댓값 + 1 길이로 초기화)
    val count = MutableList(maxNum + 1) { 0 }
    // 누적합을 저장하는 배열
    val countSum = MutableList(maxNum + 1) { 0 }
    // 결과 리스트
    val sortedArr = MutableList(arr.size + 1) { 0 }

    // 숫자 등장 횟수 세기 -> 과정 1
    for (i in arr.indices) {
        count[arr[i]] += 1
    }

    // 숫자 등장 횟수 누적합 구하기 -> 과정 2
    countSum[0] = count[0]
    for (i in 1 until maxNum + 1) {
        countSum[i] = countSum[i - 1] + count[i]
    }

    // 뒤에서부터 배열을 돌면서, 해당하는 값의 인덱스에 값 넣기 -> 과정 3
    for (i in arr.size - 1 downTo 0) {
        sortedArr[countSum[arr[i]]] = arr[i]
        countSum[arr[i]]--
    }

    return sortedArr
}
```

### Python
``` python
def counting_sort(array):
    # 최댓값
    MAX_NUM = max(array)
    N = len(array)

    # 숫자 등장 횟수를 세는 변수
    count = [0] * (MAX_NUM + 1)
    # 누적합을 세는 변수
    countSum = [0] * (MAX_NUM + 1)

    # 숫자 등장 횟수 세기
    for i in range(0, N):
        count[array[i]] += 1

    # 누적합 구성
    countSum[0] = count[0]
    for i in range(1, MAX_NUM + 1):
        countSum[i] = countSum[i - 1] + count[i]

    # 결과 변수
    result = [0] * (N + 1)

    # 뒤에서부터 순회
    for i in range(N-1, -1, -1):
        result[countSum[array[i]]] = array[i]
        countSum[array[i]] -= 1

    for i in range(1, N + 1):
        print(result[i], end=" ")
```


* 사용 : 정렬하는 숫자가 특정한 범위 내에 있을 때 사용
<br/>
(Suffix Array를 얻을 때, 시간복잡도 O(nlogn)으로 얻을 수 있다)

* 장점 : O(n)의 시간복잡도

* 단점 : 배열 사이즈 N만큼 돌 때, 증가시켜주는 Counting 배열의 크기가 커진다(메모리 낭비가 심하다).

<br/>

**[참조]**
* [애니메이션 예시](http://www.cs.miami.edu/home/burt/learning/Csc517.091/workbook/countingsort.html)
* [원본링크](https://gyoogle.dev/blog/algorithm/Counting%20Sort.html)
* [Counting Sort : 계수 정렬](https://bowbowbow.tistory.com/8)