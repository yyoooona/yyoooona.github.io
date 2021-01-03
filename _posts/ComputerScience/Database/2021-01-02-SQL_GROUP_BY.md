---
title: Programmers SQL 고득점 Kit - GROUP BY편
excerpt: SQL 고득점 Kit - GROUP BY편
search: true

categories:
  - Database
  
tags: 
  - [SQL, Coding Test, GROUP BY]

last_modified_at: 2021-01-02
---


### GROUP BY

[programmers SQL 고득점 Kit - GROUP BY 편](https://programmers.co.kr/learn/courses/30/parts/17044)

#### 1. (*) 고양이와 개는 몇 마리 있을까

- GROUP BY

  => Select ~~~ From ~ where ~ GROUP BY ~ Order By (정렬하고 싶은 열) ASC|DESC

- MySQL 그룹별 카운트 값 구할 때,

  => SELECT COUNT(칼럼명) ~~~~ GROUP BY (칼럼명)

- 오래 걸린 이유 -> ORDER BY 해주지 않아도 알아서 오름차순으로 정렬될 줄 알았는데 ORDER BY 할 COLUMN명을 명시해줬어야 함!

  ```sql
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) AS count FROM ANIMAL_INS GROUP BY ANIMAL_TYPE ORDER BY ANIMAL_TYPE ASC
  ```
  
  

#### 2. 동명 동물 수 찾기

- GROUP BY ~ HAVING

  => 그룹으로 묶은 다음 해당 그룹마다 조건을 주고 싶을 때 HAVING 절을 사용한다


  ```sql
  SELECT NAME, COUNT(NAME) as COUNT FROM ANIMAL_INS WHERE NAME IS NOT NULL GROUP BY NAME HAVING COUNT(NAME)>=2 ORDER BY NAME
  ```



#### 3. (**) 입양 시각 구하기(1)

- 조건절에 BETWEEN을 사용하려다가 오래걸림(아래 두 가지 조건절 모두 가능!)

  => WHERE HOUR(DATETIME) BETWEEN 9 AND 19

  => WHERE HOUR(DATETIME)>=9 AND HOUR(DATETIME)<=19

- DATETIME을 시간으로 뽑으려면 HOUR(DATETIME)

  => 그룹으로 묶은 다음 해당 그룹마다 조건을 주고 싶을 때 HAVING 절을 사용한다

  ```sql
  SELECT HOUR(DATETIME) AS HOUR, COUNT(HOUR(DATETIME)) AS COUNT
  FROM ANIMAL_OUTS
  WHERE HOUR(DATETIME)>=9 AND HOUR(DATETIME)<=19
  GROUP BY HOUR(DATETIME)
  ORDER BY HOUR(DATETIME)
  ```




>  (*) SQL 구문 읽는 순서 FROM(1)-> WHERE(2) -> GROUPBY(3) -> HAVING(4) -> SELECT(5) -> ORDER BY(6)



#### 4. (***) 입양 시각 구하기(2)

- SET 변수 선언 ( = ,  := )

  => SET @hour := -1

  (*) 이후에 해당 변수 사용할 때 계속 '@' 붙여주면서 사용해야해

  (*) += 이런 수식은 사용 불가하고 변수에 값을 대입하려면 '@hour := @hour +1' 이런식으로 써야돼

- SUBQUERY

  > 스칼라 서브쿼리 -  SELECT절에 사용하는 SUBQUERY
  >
  > 중첩 서브쿼리 - WHERE문에 작성하는 서브쿼리
  >
  > 인라인 뷰 - FROM 문에 작성하는 서브쿼리
  
- 사실 이런문제까지 코테 SQL에 나올 것 같진 않다...

  ```sql
  SET @hour := -1;
  
  SELECT (@hour := @hour + 1) AS HOUR, 
  (SELECT COUNT(*) FROM ANIMAL_OUTS WHERE @hour = HOUR(DATETIME))
  FROM ANIMAL_OUTS
  WHERE @hour<23
  ```

