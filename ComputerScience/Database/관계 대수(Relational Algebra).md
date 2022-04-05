# Data Manipulation Languages(DMLs)

데이터베이스로부터 정보를 얻거나, 저장하는 데 사용하는 두 가지 방식의 '데이터 언어'가 있다.

* **절차적 언어**: Relational Algebra(관계 대수). 원하는 결과를 찾기 위해서 데이터의 처리 과정을 구체화해야 한다.
* **비절차적 언어**: Relational Calculus(관계 해석). 원하는 결과만 구체화하고, 어떻게 찾을지는 필요하지 않다.

원하는 데이터를 얻기 위해서 릴레이션에 필요한 쿼리(Query)를 수행할 때 사용한다. 데이터를 쉽고 빠르고 정확하게 얻기 위해 사용되며, 절차적 또는 비절차적 언어로 모든 쿼리를 기술할 수 있으면 데이터 언어를 관계적으로 완전(Relationally Complete)하다고 할 수 있다.

<br/>

# Relational Algebra

**Relational Algebra, 관계 대수** 는 절차적 언어로서, 릴레이션에서 튜플을 얻거나 조작하기 위한 기본적인 연산들의 집합이다. 각 연산은 하나 이상의 릴레이션을 피연산자로 사용하여, 새로운 릴레이션을 반환한다. 또 여러 연산들이 chain 형태로 연결되어 더 복잡한 연산을 만들 수 있다.

다음과 같은 릴레이션이 주어져 있다고 하자.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbTV2el%2Fbtq6DSn6wzp%2FaoO1InyzvfOjAq7H5UBZq1%2Fimg.png)

<br/>

## Select Operation : σpredicate(R)

Select operation은 릴레이션(**R**)에서 조건(**predicate**)를 만족하는 tuple들을 추출해주는 연산자이다. 하나의 릴레이션에 대해 수행하는 단항 연산이며, 조건들을 다른 연산자(and, or, not 등)들을 이용하여 더 복잡하게 구성할 수도 있다.

ex) σgrade=3(student)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdAyxnz%2Fbtq6DSBAVmN%2FO5iXUKqsEkU3nXrL4zrhFk%2Fimg.png)

ex) σgrade≤3∧major=Physics(student)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FOzZER%2Fbtq6CBG2Lsd%2FXv8svRJEztssrda04xUjtk%2Fimg.png)

<br/>

## Projection Operation: πA1,A2,...,An(R)

Projection Operation은 릴레이션(**R**)에서 주어진 속성(Attribute)들만 포함하는 릴레이션을 반환한다. 하나의 릴레이션에 대해 수행하는 단항 연산이며, **중복되는 원소가 생기면 중복을 제거한다.**

ex) πName,Grade(student) 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fu2GG3%2Fbtq6EVZjNNP%2F13sPPymaeK92drxWglG1mk%2Fimg.png)

ex) πGrade,Major(student)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdcvOP5%2Fbtq6DRJs2ZA%2FKcn8bXBawCo25AUPk0f3qK%2Fimg.png)

<br/>

## Union Operation : (R∪S)

두 릴레이션의 합집합을 반환한다. R 또는 S 릴레이션에 들어있는 모든 튜플들을 포함한다. 입력으로 들어오는 두 릴레이션은 속성(Attribute)의 개수가 같고, 대응되는 필드들이 동일한 도메인(domain)을 가지고 있어야 한다. (이 성질을 합병가능(Union-compatible)이라고 한다)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZfUNp%2Fbtq6Ft2D0Ud%2F00td0DllISMB7bhKYlUkJk%2Fimg.png)

ex) (student∪R)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fv8DQq%2Fbtq6AOtuBNA%2FqkmUKx6rScMsQcz5KTbM30%2Fimg.png)

<br/>

## Intersection Operation : (R∩S)

두 릴레이션의 교집합을 반환한다. R과 S에 동시에 들어있는 튜플들의 집합을 릴레이션으로 반환한다. 마찬가지로 R과 S의 속성의 개수가 같고, 대응되는 필드들이 동일한 도메인을 가지고 있어야 한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCY7d5%2Fbtq6z3q5rQR%2F7HkExkAQrMHvkDzRuL2kjK%2Fimg.png)

ex) (student∩R)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc7SRqS%2Fbtq6BDZvdp2%2FbPVYGBk89PtEk1KHJtXV7K%2Fimg.png)

<br/>

## Difference Operation: (R-S)

두 릴레이션의 차집합을 반환한다. R에 속해있으면서 S에 속해있지 않은 튜플들의 집합을 릴레이션으로 반환한다. 마찬가지로 R과 S의 속성의 개수가 같고, 대응되는 필드들이 동일한 도메인을 가지고 있어야 한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCY7d5%2Fbtq6z3q5rQR%2F7HkExkAQrMHvkDzRuL2kjK%2Fimg.png)

ex) (student−R)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0njQQ%2Fbtq6CAOZ89m%2F2beLDzZlCjfaMLl5kIw8lk%2Fimg.png)

<br/>

## Cartesian Product Operation : (R×S)

R과 S 두 릴레이션의 튜플들로 구성 가능한 모든 조합의 튜플을 생성한다. 결과로 나온 릴레이션의 속성의 개수는 R의 속성의 개수 + S의 속성의 개수가 되며, 기수(cardinality)는 R의 기수 * S의 기수가 된다. 만약 R과 S에 공통된 속성이 있는 경우 둘 중 하나의 이름을 재정의해야 한다.

Cartesian Product의 결과로 나오는 릴레이션은 크기가 매우 클 수 있는 반면, 실제로 사용자가 원하는 것은 일부인 경우가 많으므로 효율적인 연산자는 아니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkAHRY%2Fbtq6AdtOOC3%2Fxjp0GrkOY6bCiHqDnwtWDK%2Fimg.png)

ex) (student×R)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHoUXo%2Fbtq6BB1v3eG%2Fgm3MqMXWzL9p68WedTMiq0%2Fimg.png)

<br/>

## Join Operation (R⋈S)

두 릴레이션 R과 S의 공통 속성에 대해서 해당 속성이 같은 튜플을 뽑는 연산이다. Cartesian Product 연산과 Selection 연산의 결합으로 생각할 수 있다. 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbB806s%2Fbtq6GomJX0j%2FGUSHPyrXppIVnWp53FTo41%2Fimg.png)

ex) (student⋈R)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb13w6C%2Fbtq6APMVeMM%2FeZ0DNINoN0sSKeJRL6Iqhk%2Fimg.png)

공통 속성으로 (Grade, Major) = (4, Computer Science), (2, Physics)가 있으므로, 전체 Cartesian Product에서 해당 속성을 갖는 튜플만 뽑아오는 형태이다. 

<br/>

**참조**
* [원본링크](https://rebro.kr/146?category=484170)