# 동적 계획법(Dynamic Programming)

* **복잡한 문제를 간단한 여러 개의 문제로 나누어 푸는 방법이다.**

* 흔히 말하는 DP가 바로 '동적 계획법'에 해당한다.

* **한 가지 문제**에 대해서, **단 한 번만 풀도록** 만들어주는 알고리즘이다.

* 즉, 똑같은 연산을 반복하지 않도록 만들어준다. 실행 시간을 줄이기 위해 많이 이용되는 수학적 접근 방식의 알고리즘이라고 할 수 있다.

* 동적 계획법은 **Optimal Substructure**에서 효과를 발휘한다.
<br/>
Optimal Substructure는 답을 구하기 위해 이미 했던 똑같은 계산을 계속 반복하는 문제 구조를 뜻한다.

<br/>

## 접근 방식
커다란 문제를 쉽게 해결하기 위해 작게 쪼개서 해결하는 방법인 분할 정복과 매우 유사하다. 하지만 간단한 문제로 만드는 과정에서 중복 여부에 대한 차이점이 존재한다.

즉, 동적 계획법은 간단한 작은 문제들 속에서 '계속 반복되는 연산'을 활용하여 빠르게 풀 수 있는 것이 핵심이다.

<br/>

## 간단한 예시
4살짜리 아이에게 다이나믹 프로그래밍을 어떻게 설명할까요?
> 종이에 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 = 를 적은 후, 아이에게 답을 묻는다.
<br/>
아이 : (숫자를 센 후) 8!
<br/>
문제의 왼쪽에 1 + 을 추가한 후, 아이에게 다시 답을 묻는다.
<br/>
아이 : (빠르게) 9!
<br/>
어떻게 그렇게 빠르게 9라고 알았어? 라고 물으니 아이는 "단지 1을 더했어요!"라고 답한다.
<br/>
답이 8이였다는 것을 기억하기 때문에 단지 그 값에 1을 더했을 뿐, 1부터 9까지 다시 셀 필요가 없다! 다이나믹 프로그래밍은 이처럼 그저 "시간을 절약하기 위해 무언가를 기억하는 것" 입니다.

<br/>

## 조건
* 작은 문제에서 반복이 일어난다.
* 같은 문제는 항상 정답이 같다.

이 두 가지 조건이 충족한다면, 동적 계획법을 이용하여 문제를 풀 수 있다.

같은 문제가 항상 정답이 같고, 반복적으로 일어난다는 점을 활용해 메모이제이션(Memoization)으로 큰 문제를 해결해나가는 것이다.

* **메모이제이션(Memoization)**
<br/>
한 번 계산한 문제는 다시 계산하지 않도록 저장해두고 활용하는 방식이다.

<br/>

## 예시
파보나치 수열에서 재귀를 활용하여 풀 경우, 같은 연산을 계속 반복함을 알 수 있다.
<br/>
이때, 메모이제이션을 통해 같은 작업을 되풀이 하지 않도록 구현하면 효율적이다.

``` python
fibonacci(5) = fibonacci(4) + fibonacci(3)
fibonacci(4) = fibonacci(3) + fibonacci(2)
fibonacci(3) = fibonacci(2) + fibonacci(1)
```

파보나치 구현이 재귀를 활용했다면 시간복잡도는 O(2^n)이지만, 동적 계획법을 활용하면 O(N)으로 해결할 수 있다.

<br/>

## 구현 방식
* Bottom-up : 작은 문제부터 차근차근 구하는 방법
* Top-down : 큰 문제를 풀다가 풀리지 않은 작은 문제가 있다면 그때 해결하는 방법(재귀 방식)

> Bottom-up은 해결이 용이하지만, 가독성이 떨어집니다.
<br/>
Top-down은 가독성이 좋지만, 코드 작성이 힘듭니다.

동적 계획법으로 문제를 풀 때는, 우선 작은 문제부터 해결해나가보는 것이 좋다.

작은 문제들을 풀어나가다보면 이전에 구해둔 더 작은 문제들이 활용되는 것을 확인하게 된다. 이에 대한 규칙을 찾았을 때 **점화식**을 도출해내어 동적 계획법을 적용시키자.

<br/>

## 파보나치 코드
### Python - 일반적인 파보나치
``` python
# 피보나치 함수를 재귀 함수로 구현
def fibo(x):
    # x가 2이하인 경우(1, 2)
    if x <= 2:
        return 1

    return fibo(x - 1) + fibo(x - 2)
```

### Python - 탑다운 방식
``` python
# 한 번 계산된 결과를 메모이제이션하기 위한 리스트 초기화
d = [0] * 100

# 파보나치 함수를 재귀적으로 구현(탑다운 다이나믹 프로그래밍)
def fibo(x):
    # 종료 조건(x가 2이하일 때 1을 반환)
    if x <= 2:
        return 1
    # 이미 계산된 적 있다면 그대로 반환
    if d[x] != 0:
        return d[x]

    # 아직 계산하지 않은 문제라면 점화식에 따라 파보나치 결과 반환
    d[x] = fibo(x - 1) + fibo(x - 2)

    return d[x]
```

### Python - 보텀업 방식
``` python
# 앞서 계산된 결과를 저장하기 위한 DP 테이블
dp = [0] * 100

# 첫 번째 파보나치 수와 두 번째 파보나치 수는 1
dp[1] = 1
dp[2] = 1
# 파보나치 99의 값을 알아보기 위한 수
n = 99

# 파보나치 함수 반복문으로 구현(보텀업 다이나믹 프로그래밍)
for i in range(3, n + 1):
    dp[i] = dp[i - 1] + dp[i - 2]

print(dp[n])
```

### Kotlin - 탑다운 방식
``` kotlin
// 한 번 계산된 결괄르 메모이제이션하기 위한 리스트 초기화
val dp = MutableList(100) { BigInteger.ZERO }

fun fibo(x: Int): BigInteger {
    // 종료 조건(2 이하면 1을 반환)
    if(x <= 2) return BigInteger.ONE
    // 이미 계산한 적 있다면 그대로 반환
    if(dp[x] != BigInteger.ZERO) return dp[x]
    // 아직 계산하지 않은 문제라면 점화식에 따라 파보나치 결과 반환
    dp[x] = fibo(x - 1) + fibo(x - 2)

    return dp[x]
}
```

### Kotlin - 보텀업 방식
``` kotlin
fun fibo(x: Int): BigInteger {
    // 앞서 계산된 결과를 저장하기 위한 DP 테이블
    val dp = MutableList(100) { BigInteger.ZERO }

    // 첫 번째 피보나치 수와 두 번째 피보나치 수는 1
    dp[1] = BigInteger.ONE
    dp[2] = BigInteger.ONE

    // 피보나치 함수 반복문으로 구현(보텀업 다이나믹 프로그래밍)
    for(i in 3..x) {
        dp[i] = dp[i - 1] + dp[i - 2]
    }

    return dp[x]
}
```

### Kotlin - tailrec 활용
tailrec은 꼬리재귀(tail recursive)라는 의미로, 추가적인 연산이 없이 자신 스스로 재귀 호출하다가 어떤 값을 리턴하는 함수를 의미한다. 자신만 반복적으로 호출하는 함수(tailrec)는 while과 같이 루프를 사용하는 코드로 변환이 가능한데, 이렇게 되면 소비되는 스택을 아낄 수 있다. 자세한 내용은 참고자료 참조!!

``` kotlin
tailrec fun fibonacci(n: Int, first: BigInteger, second: BigInteger): BigInteger {
    return if(n == 0) first else fibonacci(n - 1, second, first + second)
}

fun main() {
    val first = BigInteger.ZERO
    val second = BigInteger.ONE
    println(fibonacci(99, first, second))
}
```

<br/>

**[참고 자료]**
* [원본링크](https://gyoogle.dev/blog/algorithm/Dynamic%20Programming.html)
* [다이나믹 프로그래밍](https://velog.io/@changhee09/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%8B%A4%EC%9D%B4%EB%82%98%EB%AF%B9-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
* [tailrec(꼬리재귀)에 대해서 알아보기](https://codechacha.com/ko/kotlin-tailrect/)