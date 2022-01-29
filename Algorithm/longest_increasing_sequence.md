# 최장 증가 수열(Longest Increasing Sequence)

* 최장 증가 수열은 가장 긴 증가하는 부분 수열을 나타낸다.

* [7, __2__, __3__, 8, __4__, __5__] -> 해당 배열에서는 [2,3,4,5]가 LIS로 답은 4이다.

<br/>

## 시간 복잡도
* DP : O(N^2)
* Lower Bound : O(NlogN)

<br/>

## DP 활용 코드
``` kotlin
fun findLis(arr: ArrayList<Int>): MutableList<Int> {
    val dp = MutableList(arr.size) { 0 } // LIS 저장 배열

    for(i in 1 until dp.size) {
        for(j in i-1 downTo 0) {
            if(arr[i] > arr[j]) {
                dp[i] = if(dp[i] < dp[j]+1) dp[j]+1 else dp[i]
            }
        }
    }
    return dp
}

// 저장된 dp 배열 값 : [0,0,1,2,2,3]
// LIS : dp배열에 저장된 값 중 최대 값 + 1

fun main() {
    val array = arrayListOf(7,2,3,8,4,5)
    print(findLis(array))
}
```

하지만, N^2으로 해결할 수 없는 문제라면? (원소의 개수가 최대 10만 등..)

이때는 Lower Bound를 활용한 LIS 구현을 진행해야한다.

~~Lower Bound 및 참조 내용 추가하기~~

<br/>

__[참조]__
* [원본링크]()
* [LIS의 길이를 구하는 3가지 알고리즘](https://shoark7.github.io/programming/algorithm/3-LIS-algorithms)