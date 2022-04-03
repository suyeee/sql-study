# 1.DB란?

+ `DB` 는 **데이터베이스**를 말함
+ 데이터를 통합하여 관리하는 데이터의 집합



## DBMS

+ 데이터베이스를 관리하는 미들웨어 시스템



## 데이터베이스의 종류

### RDBMS

+ `Relational Database Management System`

+ **Oracle, Mysql, Sqlite...** 이 RDBMS 에 해당
+ 정형데이터를 다룸
+ 데이터 테이블 사이에 키값으로 관계를 가지고 있는 데이터베이스
+ 장점
  + 데이터 사이의 관계설정으로 최적화된 스키마를 설계할수있다.
  + 테이블간에 관계성이 있기에 분류, 정렬, 탐색속도가 빠르다.
+ 단점
  + 스키마 수정이 어렵다.
  + 데이터 저장은 NoSQL 보다는 느리다.
+ `Relationship`
  + 1:1 
  + 1:n
  + n:n



### NoSQL

+ **No**  `Relational Database Management System`
+ 테이블과 테이블사이에 관계가 없다
+ 데이터 사이에 관계가 없으므로 복잡성이 줄어든다
+ No Relational 말고 No sql 이라는 의미 그 자체로 쓰이기도 한다.
+ 장점
  + 데이터간의 관계성이 없으니 복잡성이 줄어 많은 데이터를 빠르게 저장할수있다.



## 데이터베이스의 구조

### Schema

+ 스키마는 데이터베이스의 구조를 만드는 디자인
+ 외부스키마, 개념스키마, 내부스키마가 있다.



### Table

+ 행과 열로 이루어져 있는 데이터베이스를 이루는 기본단위



### Column(열)

+ 테이블의 세로축 데이터
+ **Field, Attribute** 라고 부르기도 한다.



### Row(행)

+ 테이블의 가로축 데이터
+ **Tuple, Record** 라고 부르기도함.



## DDL

> DB, 테이블 등 데이터 구조를 정의하는데 사용되는 명령어



### 명령어

#### 1.CREATE

+ 데이터베이스 생성

```mysql
CREATE DATABASE test; #test 라는 이름의 DB가 생성된다.
```



#### 2.SHOW

+ 데이터베이스를 확인

```mysql
SHOW DATABASES;  #DB 목록들을 보여준다.
```



#### 3.USE

+ 데이터베이스 선택하기
+ 선택된 데이터베이스에 데이터가 들어감.

```mysql
USE test; #test DB 로 선택된다.
```



#### 4.SELECT

+ 현재 선택된 데이터베이스 확인

```mysql
SELECT DATABASE(); #test로 선택되있으니 결과창에 test 로 뜬다.
```



#### 5.테이블 만들기

+ **(무결성) 제약조건이 없는 user1 테이블 생성**

```mysql
CREATE TABLE user1(
	user_id INT,
	name VARCHAR(20),
	email VARCHAR(20),
	age INT(3),
	rdate DATE
);
```



