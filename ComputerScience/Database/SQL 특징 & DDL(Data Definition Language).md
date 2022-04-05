# SQL이란?

SQL은 Structured Query Language의 줄임말로, **관계형 데이터베이스 시스템(DBMS)에서 데이터를 관리, 처리하기 위해 설계된 프로그래밍 언어** 이다. 1970년대 IBM에서 SEQUEL(Structured English Query Language)이라는 이름으로 처음 개발되었으며, 후에 Structured Query Language로 다시 이름이 지어졌다.

SQL 문법의 종류를 다음 세 가지로 분류된다.

* **Data Definition Language(DDL)**: Schema definition (CREATE, ALTER, RENAME, DROP,...)
* **Data Manipulation Language(DML)**: SELECT, INSERT, UPDATE, DELETE
* **Data Control Language(DCL)**: Security, access controls

DML은 데이터를 관리하기 위한 언어이고, DDL은 릴레이션을 정의하기 위한 언어이며, DCL은 사용자별로 데이터를 관리하고 접근하는 권한을 다루는 언어이다.

<br/>

# SQL의 특징

1. 영어 문장과 비슷한 구문을 갖추고 있어 쉽게 배우고 사용할 수 있는 언어이다. 

2. SQL은 데이터 연산에 대한 처리가 절차적으로 진행되지 않고, 데이터의 집합 단위로 처리된다. (Non-Procedural) 따라서, **SQL 문장을 작성하는 사용자는 데이터가 어떻게 처리되는지는 알 필요가 없다는 장점이 있다.**

3. **표준 SQL 문법이 존재**해 DBMS 종류(Oracle, MSSQL, MySQL 등)에 얽매이지 않고 사용할 수 있다.

4. 릴레이션(relation), 튜플(tuple), 속성(attribute)과 같은 관계형 모델의 용어 대신 테이블(table), 행(row), 열(column)과 같은 일반적인 용어를 사용한다.

5. SQL은 기본적으로 대소문자를 구분하지 않는다. 다만 가독성을 위해서 주로 대문자를 사용하기도 하며, 간혹 서버 환경이나 데이터베이스 시스템에 따라 대소문자를 구분하기도 한다. 

6. SQL 명령은 반드시 세미콜론(;)으로 끝나야 하고, 고유의 값은 따옴표(' ')로 감싸준다. 

7. 한 줄 주석은 '--'를 앞에 붙여 사용하고, 여러 줄 주석은 /* */로 감싸준다. 

<br/>

**참조**
* [SQL 특징 & DDL](https://rebro.kr/147?category=484170)
* [DML - INSERT, SELECT](https://rebro.kr/148?category=484170)
* [DML - UPDATE, DELETE](https://rebro.kr/151?category=484170)
* [집계 함수, GROUP BY, HAVING](https://rebro.kr/154?category=484170)