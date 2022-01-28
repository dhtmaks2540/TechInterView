# 이분 탐색(Binary Search)

* 이진 탐색 혹은 이분 탐색이라고 부른다.
* 이미 정렬되어 있는 자료 구조에서 특정 값을 찾을 때, 탐색 범위를 절반씩 나눠가면서 해당 값을 찾아가는 것이다.
* 즉, 탐색 범위를 두 부분으로 분할하면서 찾는 방식이다.
* 처음부터 끝까지 돌면서 탐색하는 것(순차 탐색 등이 해당)보다 훨씬 빠르다는 장점을 가지고 있다.

<br/>

## 시간 복잡도
* 전체 탐색 : O(N)
* 이분 탐색 : __O(logN)__

<br/>

## 진행 순서
* 우선 정렬을 수행해야 한다.
* left와 right로 mid 값을 설정한다.
* mid(index)의 값과 구하고자 하는 값을 비교한다.
* 구할 값이 mid보다 크다면 -> left = mid + 1
* 구할 값이 mid보다 작다면 -> right = mid - 1
* left > right가 될 때까지 계속 반복한다.

<br/>

## 코드
### 코틀린 - 반복 구조
``` kotlin
// 반복구조 이진 탐색
fun binarySearch(array: ArrayList<Int>, target: Int): Int {
    // 정렬 수행
    array.sort()
    
    // 왼쪽, 오른쪽, 중간을 나타내는 변수
    var left = 0
    var right = array.size
    var mid = 0
    
    while (left <= right) {
        mid = left + (right - left) / 2

        // 목표값에 해당하는 경우        
        when {
            target == array[mid] -> {
                return mid
            }
            target < array[mid] -> { // 목표값보다 큰 경우
                right = mid - 1
            }
            else -> { // 목표값보다 작은 경우
                left = mid + 1
            }
        }
    }

    return -1
}
```

### 코틀린 - 재귀
``` kotlin
// 재귀구조 이진 탐색
// 이미 정렬이 수행되어 있다고 가정
fun recursionBinarySearch(array: ArrayList<Int>, left: Int = 0, right: Int = array.size - 1, target: Int): Int {
    if(left > right) return -1

    val mid = left + (right - left) / 2

    return when {
        // 목표값과 같다면 mid return
        target == array[mid] ->
            mid
        // 목표값보다 크다면 right = mid - 1
        target < array[mid] ->
            recursionBinarySearch(array, left, mid - 1, target)
        // 목표값보다 작다면 left = mid + 1
        else ->
            recursionBinarySearch(array, mid + 1, right, target)
    }
}
```

### 파이썬 - 반복
``` python
def binary_search(array, target):
    left, right = 0, len(array) - 1

    while left <= right:
        mid = left + (right - left) // 2

        if array[mid] == target:
            return mid
        elif array[mid] > target:
            right = mid - 1
        else:
            left = mid + 1

    return -1
```

### 파이썬 - 재귀
``` python
def recursion_binary_search(array, target, left, right):
    if left > right:
        return -1
    
    mid = left + (right - left) // 2

    if array[mid] == target:
        return mid
    elif array[mid] > target:
        return recursion_binary_search(array, target, left, mid - 1)
    else:
        return recursion_binary_search(array, target, mid + 1, right)
```

<br/>

__[참조]__
* [원본링크](https://gyoogle.dev/blog/algorithm/Binary%20Search.html)