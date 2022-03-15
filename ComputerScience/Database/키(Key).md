# Key
* Key : 검색, 정렬시 Tuple을 구분할 수 있는 기준이 되는 Attribute

<br/>

## 키의 종류
1. Candidate Key(후보키)

* Tuple을 유일하게 식별하기 위해 사용하는 속성들의 부분 집합(기본키로 사용할 수 있는 속성들)
* 아래 2가지 조건을 만족한다.
  * 유일성 : Key로 하나의 Tuple을 유일하게 식별할 수 있음
  * 최소성 : 꼭 필요한 속성으로만 구성

<br/>

2. Primary Key(기본키)

* 후보키 중 선택한 Main Key
* 특징
  * Null을 가질 수 없다.
  * 동일한 값이 중복될 수 없다.

<br/>

3. Alternate Key(대체키)

* 후보키 중 기본키를 제외한 나머지 키 = 보조키

<br/>

4. Super Key(슈퍼키)

* 유일성은 만족하지만, 최소성은 만족하지 못하는 키

<br/>

5. Foreign Key(외래키)

* 다른 릴레이션의 기본키를 그대로 참조하는 속성의 집합

<br/>

**참조**
* [원본링크](https://gyoogle.dev/blog/computer-science/data-base/Key.html)ㅇㅁ