# Join
* 두 개 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법
* 테이블을 연결하려면, 적어도 하나의 칼럼을 서로 공유하고 있어야 하므로 이를 이용하여 데이터 검색에 활용한다.

<br/>

## Join 종류
`INNER JOIN`, `LEFT OUTER JOIN`, `RIGHT OUTER JOIN`, `FULL OUTER JOIN`, `CROSS JOIN`, `SELF JOIN`으로 분류할 수 있다.

``` sql
문법
SELECT 
테이블별칭.조회할컬럼,
테이블별칭.조회할 컬럼
FROM 기분테이블 별칭
INNER JOIN 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기분지 .....
```

<br/>

* **INNER JOIN**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile9.uf.tistory.com%2Fimage%2F99799F3E5A8148D7036659)

* 쉽게 말해 **교집합**으로 생각하면 된다.
* 기준 테이블과 join 테이블의 중복된 값을 보여준다.
* 결과값은 A의 테이블과 B 테이블이 모두 가지고 있는 데이터만 검색된다.

```sql
SELECT
A.NAME, B.AGE
FROM EX_TABLE A
INNER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
```

<br/>

* **LEFT OUTER JOIN**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F997E7F415A81490507F027)

* **기준 테이블값 + 조인테이블과 기준테이블의 중복된 값**을 보여준다.
* 왼쪽 테이블을 기준으로 JOIN을 한다고 생각하면 편하다.
* 결과값은 A 테이블의 모든 값과 A 테이블과 B 테이블의 중복되는 값이 검색된다.

``` sql
SELECT
A.NAME, B.AGE
FROM EX_TABLE A
LEFT OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
```

<br/>

* **RIGHT OUTER JOIN**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile25.uf.tistory.com%2Fimage%2F9984CE355A8149180ABD1D)

* LEFT OUTER JOIN과는 반대이다.
* 오른쪽 테이블을 기준으로 Join을 하겠다고 생각하면 된다.
* 결과값은 B 테이블의 모든 값과 A 테이블과 B 테이블에서 중복되는 값이 검색된다.

``` sql
SELECT
A.NAME, B.AGE
FROM EX_TABLE A
RIGHT OUTER JOIN JOIN_TABLE B ON A.NO_ENP = B.NO_ENP
```

<br/>

* **FULL OUTER JOIN**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile24.uf.tistory.com%2Fimage%2F99195F345A8149391BE0C3)

* 쉽게 말해 **합집합**을 말한다. 
* A 테이블이 가지고 있는 데이터, B 테이블이 가지고 있는 **데이터 모두 검색**된다.
* 사실상, 기준 테이블의 의미가 없다.

``` sql
SELECT
A.NAME, B.NAME
FROM EX_TABLE A
FULL OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
```

<br/>

* **CROSS JOIN**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F993F4E445A8A2D281AC66B)

* **모든 경우의 수를 전부 표현해주는 방식**이다.
* 기준 테이블이 A일 경우, A의 데이터 한 Row를 B 테이블 전체와 JOIN 하는 방식이다.
* 결과값은 N * M 이다.
* 위 그림의 경우, A 테이블의 데이터가 3개, B 테이블의 데이터가 4개가 있으므로 총 3*4 = 12개의 데이터가 검색된다.

``` sql
-- 첫 번째 방식--
SELECT
A.NAME, B.AGE
FROM EX_TABLE A
CROSS JOIN JOIN_TABLE B

-- 두 번째 방식--
SELECT 
A.NAME, B.AGE
FROM EX_TABLE A, JOIN_TABLE B
```

<br/>

* **SELF JOIN**

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile25.uf.tistory.com%2Fimage%2F99341D335A8A363D0614E8)

* 자기자신과 자기자신을 조인하는 것이다.
* 하나의 테이블을 여러번 복사해서 조인한다고 생각하면 편하다.
* 자신이 갖고 있는 칼럼을 다양하게 변형시켜 활용할 때 자주 사용한다.

``` sql
SELECT 
A.NAME, B.AGE
FROM EX_TABLE A, EX_TABLE B
```

<br/>

**참조**
* [원본링크](https://gyoogle.dev/blog/computer-science/data-base/Join.html)