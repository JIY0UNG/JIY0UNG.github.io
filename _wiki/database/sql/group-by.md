---
layout  : wiki
title   : GROUP BY
summary : 
date    : 2023-05-09 00:08:08 +0900
updated : 2023-05-09 00:08:08 +0900
tag     : sql
toc     : true
public  : true
parent  : sql
latex   : false
---

* TOC
{:toc}

특정 컬럼의 값으로 레코드를 그루핑하고, 그룹별로 집계된 결과를 하나의 레코드로 조회할 때 사용한다.

## WITH ROLLUP
`GROUP BY`가 사용된 쿼리에서는 그루핑된 그룹별로 소계를 가져올 수 있는 롤업(ROLLUP) 기능을 사용할 수 있다. 이때 `GROUP BY`에 사용된 컬럼 개수에 따라 소계의 레벨이 달라진다.

```sql
SELECT dept_no, COUNT(*)
FROM dept_emp
GROUP BY dept_no WITH ROLLUP;
```

실행 결과

| dept\_no | COUNT\(\*\) |
| :--- | :--- |
| d001 | 20211 |
| d002 | 17346 |
| ... | ... |
| d009 | 23580 |
| null | 331603 |

## 참고 문헌
- RealMySQL 8.0 2 / 백은빈, 이성욱 저 / 위키북스 / 2022년 03월 18일
