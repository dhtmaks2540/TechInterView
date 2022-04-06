# Anomaly(이상)

**이상 현상** 이란, **테이블 내의 데이터들이 불필요하게 중복되어 테이블을 조작할 때 발생되는 데이터 불일치 현상** 이다. 테이블을 잘못 설계하여 삽입, 삭제, 갱신할 때 오류가 발생하게 되는 것이다. 이상현상에는 크게 3가지 이상현상이 있으며, 정규화를 통해서 이상 현상들을 해결할 수 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FccleUR%2FbtraQrfiyuS%2FjMrM5xrAkdCJjqTpiUZP81%2Fimg.png)

위와 같은 테이블을 예로 들어 어떤 이상현상이 발생하는지 알아보자.

<br/>

## 삽입 이상

* 강의를 수강하지 않은 학생을 추가할 때, 과목 번호와 성적에 null값이 들어가거나 불필요한 데이터를 추가해야 삽입할 수 있는 문제점이 발생한다. 아래와 같은 데이터를 삽입할 수 없다.

* ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fndk9t%2FbtraQrsOCmT%2FMeh2Yk20kNGwUZoinbllik%2Fimg.png)

* 학생이 수강신청 할 때 반드시 과목 번호를 알아야 삽입이 가능하다.

<br/>

## 삭제 이상

* 학번이 300인 항생이 과목 수강을 취소하면 C-73인 강의에 대한 정보도 모두 삭제된다.

* ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcbtfnI%2FbtraGcqIqEM%2Fq3AUrjKNB1zDKFpvm44PJK%2Fimg.png)

* P1 교수가 강의하는 과목을 취소하는 경우 학번이 123인 학생에 대한 정보도 모두 삭제된다.

<br/>

## 갱신 이상

* 학번이 123인 학생의 지도교수가 P2로 변경되면, 123인 학생이 수강하는 모든 과목(행)에서의 지도교수를 변경시켜주어야 한다.

<br/>

**참조**
* [원본링크](https://rebro.kr/159?category=484170)