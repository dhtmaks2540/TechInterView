# Goal
* Bubble Sort에 대해 설명
* Bubble Sort 과정에 대해 설명
* Bubble Sort를 구현
* Bubble Sort의 시간복잡도와 공간복잡도를 계산

<br/>

# Abstract
Bubble Sort는 Selection Sort와 유사한 알고리즘으로 __`서로 인접한 두 원소의 대소를 비교하고, 조건에 맞지 않다면 자리를 교환하며 정렬하는 알고리즘`__ 이다.

이름의 유래로는 정렬 과정에서 원소의 이동이 거품이 수면으로 올라오는 듯한 모습을 보이기 때문에 지어졌다고 한다.

<br/>

# Process
1. 1회전에 첫 번째 원소와 두 번째 원소를, 두 번째 원소와 세 번째 원소를 세 번째 원소와 네 번째 원소를, ... 이런 식으로 (마지막 - 1)번째 원소와 마지막 원소를 비교하여 조건에 맞지 않는다면 서로 교환한다.

2. 1회전을 수행하고 나면 가장 큰 원소가 맨 뒤로 이동하므로 2회전에서는 맨 끝에 있는 원소는 정렬에서 제외되고, 2회전을 수행하고 나면 끝에서 두 번째 원소까지는 정렬에서 제외된다. 이렇게 정렬을 1회전 수행할 때마다 정렬에서 제외되는 데이터가 하나씩 늘어난다.

<br/>

# Kotlin Code(Ascending)
``` kotlin
fun bubbleSort(arr: ArrayList<Int>) {
    var temp = 0
    for(i in arr.indices) { // 1
        for(j in 1 until arr.size - i) { // 2
            if(arr[j - 1] > arr[j]) { // 3
                // 변수 변경
                temp = arr[j - 1]
                arr[j - 1] = arr[j]
                arr[j] = temp

                // XOR을 사용하여 변경 가능
                /*arr[j - 1] = arr[j - 1] xor arr[j]
                arr[j] = arr[j - 1] xor arr[j]
                arr[j - 1] = arr[j - 1] xor arr[j]*/
            }
        }
    }
    println(arr.toString())
}
```

1. 제외될 원소의 객수를 의미한다. 1회전이 끝난 후, 배열의 마지막 위치에는 가장 큰 원소가 위치하기 때문에 하나씩 증가시켜준다.

2. 원소를 비교할 index를 뽑을 반복문이다. j는 현재 원소를 가리키고, j-1은 이전 원소를 가리키게 되므로, j는 1부터 시작하게 된다.

3. 현재 가리키고 있는 두 원소의 대소를 비교한다. 해당 코드에서는 오름차순 정렬이므로 현재 원소보다 이전 원소가 더 크다면 이전 원소가 뒤로 가야하므로 서로 자리를 교환해준다.

<br/>

# GIF로 이해하는 Bubble Sort
![](https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/bubble-sort-001.gif)

<br/>

# 시간복잡도
시간복잡도를 계산하면 `(n-1) + (n-2) + (n-3) + .... + 2 + 1 => n(n-1)/2`이므로, __O(n^2)__ 이다. 또한, Bubble Sort는 정렬이 돼있던 안돼있던, 2개의 원소를 비교하기 때문에 최손, 평균, 최악의 경우 모두 시간 복잡도가 __O(n^2)__ 으로 동일하다.

<br/>

# 공간복잡도
주어진 배열 안에서 교환(swap)을 통해, 정렬이 수행되므로 __O(n)__ 이다.

<br/>

# 장점
* 구현이 매우 간단하고, 소스코드가 직관적이다.

* 정렬하고자 하는 배열 안에서 교환하는 방식이므로, 다른 메모리 공간을 필요로 하지 않다 => 제자리 정렬(in-place sorting)

* 안정 정렬(Stable Sort) 이다.

<br/>

# 단점
* 시간 복잡도가 최악, 최선, 평균 모두 O(n^2)으로, 굉장히 비효율적이다.
* 정렬 돼있지 않은 원소가 정렬 됐을때의 자리로 가기 위헤서, 교환 연산(swap)이 많이 일어나게 된다.

<br/>

# 결론
정렬 알고리즘 중에 가장 직관적인 Bubble Sort이다. 기술 면접에서도 종종 나오는 정렬 알고리즘이다.

<br/>

> 참고자료
<br/>
[원본링크](https://gyoogle.dev/blog/algorithm/Bubble%20Sort.html)
[링크](https://zeddios.tistory.com/20)
