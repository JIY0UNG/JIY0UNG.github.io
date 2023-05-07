---
layout  : wiki
title   : SELECT 절
summary : 
date    : 2023-05-07 22:29:00 +0900
updated : 2023-05-07 22:29:00 +0900
tag     : sql
toc     : true
public  : true
parent  : sql
latex   : false
---

* TOC
{:toc}

`SELECT`는 여러 개의 테이블로부터 데이터를 조합해서 빠르게 가져와야 하기 때문에, 여러 개의 테이블을 어떻게 읽을 것인가에 많은 주의를 기울여야 한다.

## 일반적인 처리 순서
어느 절이 먼저 실행되는지 모르면 처리 내용이나 처리 결과를 예측하기 어려우므로 잘 알아두자.

```sql
SELECT s.emp_no,
       COUNT(DISTINCT e.first_name) AS cnt
FROM salaries s
  INNER JOIN employees e ON e.emp_no=s.emp_no
WHERE s.emp_no IN (100001, 100002)
GROUP BY s.emp_no
HAVING AVG(s.salary) > 1000
ORDER BY AVG(s.salary)
LIMIT 10;
```

1. WHERE 적용 및 JOIN 실행  
  드라이빙 테이블[^1] ⇢ 드리븐 테이블[^2](1), 드리븐 테이블(2)
2. GROUP BY
3. DISTINCT
4. HAVING 조건 적용
5. ORDER BY
6. LIMIT

각 요소가 없는 경우를 제외하고, 이 순서가 바뀌어서 실행되는 형태의 쿼리는 거의 없다.

## 예외적으로 ORDER BY가 JOIN보다 먼저 실행되는 경우
1. WHERE 적용  
  드라이빙 테이블
2. ORDER BY
3. JOIN 실행  
  드리븐 테이블(1), 드리븐 테이블(2)
4. LIMIT

여기서는 첫 번째 테이블만 읽어서 정렬을 수행한 뒤 나머지 테이블을 읽는데, 주로 `GROUP BY` 절 없이 `ORDER BY`만 사용된 쿼리에서 위 순서를 따른다.

## 인라인 뷰(Inline View)
위 실행 순서들을 벗어나는 쿼리가 필요하다면 서브 쿼리로 작성된 인라인 뷰를 사용해야 한다.

예를 들어, 위의 쿼리에서 `LIMIT`을 먼저 적용한 후 `ORDER BY`를 실행하려고 한다면 다음과 같이 인라인 뷰를 사용해야 한다. 다음 쿼리는 `ORDER BY`와 `LIMIT`의 순서가 바뀌었기 때문에 위의 쿼리와 동일한 결과를 반환하지 않을 수도 있다.

```sql
SELECT emp_no, cnt
FROM (
  SELECT s.emp_no,
         COUNT(DISTINCT e.first_name) AS cnt,
         MAX(s.salary) AS max_salary
  FROM salaries s
    INNER JOIN employees e ON e.emp_no=s.emp_no
  WHERE s.emp_no IN (100001, 100002)
  GROUP BY s.emp_no
  HAVING MAX(s.salary) > 1000
  LIMIT 10
) temp_view -- 인라인 뷰
ORDER BY max_salary;
```

`LIMIT`을 `GROUP BY` 전에 실행하고자 할 때도 마찬가지로 서브 쿼리로 인라인 뷰를 만들어서 그 뷰 안에서 `LIMIT`을 적용하고 바깥 쿼리(아우터 쿼리)에서 `GROUP BY`와 `ORDER BY`를 적용해야 한다.

```sql
SELECT emp_no, cnt
FROM (
  SELECT tv.emp_no,
         COUNT(DISTINCT e.first_name) AS cnt,
         MAX(tv.salary) AS max_salary
  FROM (
    SELECT emp_no, salary
    FROM salaries
    WHERE emp_no IN (100001, 100002)
    LIMIT 10 -- 인라인 뷰에서 LIMIT 적용
  ) tv
    INNER JOIN employees e ON e.emp_no=tv.emp_no
  GROUP BY tv.emp_no
) temp_view
ORDER BY max_salary;
```

하지만 이렇게 인라인 뷰가 사용되면 임시 테이블이 사용되기 때문에 주의해야 한다.  
MySQL의 `LIMIT`은 오라클의 `ROWNUM`과 조금 성격이 달라서 `WHERE` 조건으로 사용하지 않고 항상 모든 처리의 결과에 대해 레코드 건수를 제한하는 형태로 사용한다.

## 참고 문헌
- RealMySQL 8.0 2 / 백은빈, 이성욱 저 / 위키북스 / 2022년 03월 18일

## 주석
[^1]: Driving Table. JOIN 시 먼저 액세스되는 테이블
[^2]: Driven Table. JOIN 시 나중에 액세스되는 테이블
