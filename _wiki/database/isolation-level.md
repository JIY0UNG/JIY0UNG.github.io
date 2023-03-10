---
layout  : wiki
title   : 격리 수준(Isolation level)
summary : 
date    : 2023-03-10 15:43:00 +0900
updated : 2023-03-10 15:43:00 +0900
tag     :
toc     : true
public  : true
parent  : database
latex   : false
---

* TOC
{:toc}

## 격리 수준(isolation level)이란?
여러 트랜잭션이 동시에 처리될 때 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 말지를 결정하는 것이다.

## MySQL의 격리 수준
### READ UNCOMMITTED
각 트랜잭션의 변경 내용이 COMMIT이나 ROLLBACK 여부에 상관없이 다른 트랜잭션에서 보인다.

예를 들어 다음과 같은 순서대로 각 트랜잭션이 실행됐을 때,

1. 사용자 A가 employees 테이블에 Lara를 INSERT
2. 사용자 B가 employees 테이블 조회
3. 사용자 A가 변경사항을 COMMIT

사용자 B는 A가 COMMIT하기 전에 A가 INSERT한 "Lara"의 정보를 조회할 수 있다.

이처럼 어떤 트랜잭션에서 처리한 작업이 완료되지 않았는데도 다른 트랜잭션에서 볼 수 있는 현상을 더티 리드(Dirty read)라 하는데, READ UNCOMMITTED 레벨에서는 더티 리드가 허용된다. 그래서 MySQL 사용 시 최소한 READ COMMITTED 이상의 격리 수준을 사용할 것을 권장한다.

### READ COMMITTED
오라클 DBMS에서 기본으로 사용하는 격리 수준이며, 온라인 서비스에서 가장 많이 선택되는 격리 수준이다.  
이 레벨에서는 어떤 트랜잭션에서 데이터를 변경했더라도 COMMIT이 완료된 데이터만 다른 트랜잭션에서 조회할 수 있기 때문에 더티 리드(Dirty read) 같은 현상은 발생하지 않는다.

예를 들어 다음과 같은 순서대로 각 트랜잭션이 실행됐을 때,

1. 사용자 A가 employees 테이블에서 Lara의 이름을 Toto로 UPDATE
2. 사용자 B가 employees 테이블 조회
3. 사용자 A가 변경사항을 COMMIT

사용자 A가 UPDATE작업을 했을 때 새로운 값인 "Toto"는 테이블에 즉시 기록되고, 이전 값인 "Lara"는 언두 로그(Undo Log)[^1]로 백업된다. 그리고 사용자 B가 테이블 조회를 했을 때는 언두 로그에 백업된 레코드가 조회되어, "Toto"가 아닌 "Lara"가 반환된다.

그런데 READ COMMITTED 격리 수준에서도 "NON-REPEATABLE READ"("REPEATABLE READ"가 불가능하다)라는 부정합[^2]의 문제가 있다.

예를 들어 다음과 같은 순서대로 각 트랜잭션이 실행됐을 때,
1. 사용자 B가 employees 테이블 조회 (Where first_name='Toto')
2. 사용자 A가 employees 테이블에서 Lara의 이름을 Toto로 UPDATE
3. 사용자 A가 변경사항을 COMMIT
4. 사용자 B가 employees 테이블 조회 (Where first_name='Toto')

사용자 B가 처음에 테이블 조회를 했을 때에는 조회 결과가 없었는데, 사용자 A가 커밋한 후에 다시 조회를 했을 때에는 "Toto"가 반환된다. 즉 사용자 B가 하나의 트랜잭션 내에서 똑같은 SELECT 쿼리를 실행했을 때는 항상 같은 결과를 가져와야 한다는 "REPEATABLE READ" 정합성에 어긋나는 것이다.

이런 부정합 현상은 하나의 트랜잭션에서 동일 데이터를 여러 번 읽고 변경하는 작업이 금전적인 처리와 연결되면 문제가 될 수도 있다.

### REPEATABLE READ

### SERIALIZABLE

## 참고 문헌
- Real MySQL 8.0 / 백은빈, 이성욱 저 / 위키북스 / 2022년 09월 30일

## 주석
[^1]: 변경되기 이전 버전의 데이터를 별도로 백업해놓은 것
[^2]: 두 데이터가 서로 일치하지 않는 것
