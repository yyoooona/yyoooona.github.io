---
title: Programmers SQL 고득점 Kit - String, Date편
excerpt: SQL 고득점 Kit - String, Date편
search: true

categories:
  - Database
  
tags: 
  - [SQL, Coding Test, String, Date]

last_modified_at: 2021-01-03
---



### String, Date

[programmers SQL 고득점 Kit - String, Date 편](https://programmers.co.kr/learn/courses/30/parts/17047)

#### 1. 루시와 엘라 찾기

- LIKE 로 문제 풀었다가 너무 코드 길이가 길고 가독성이 떨어져서 다른 방법은 없는지 찾아봄

- IN, OR NOT IN 활용

  => WHERE (COLUMN명) IN ('문자열', '문자열', etc )

  ```sql
  SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE FROM ANIMAL_INS WHERE NAME LIKE "Lucy" OR NAME LIKE "Ella" OR NAME LIKE "Pickle" OR NAME LIKE "Rogan" OR NAME LIKE "Sabrina" OR NAME LIKE "Mitty"
  ORDER BY ANIMAL_ID
  ```
  
  ```sql
  SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE FROM ANIMAL_INS WHERE NAME IN ("Lucy", "Ella", "Pickle", "Rogan", "Sabrina", "Mitty")
  ORDER BY ANIMAL_ID
  ```
  
  

#### 2. 이름에 el이 들어가는 동물 찾기

- 틀린 이유는 DOG을 찾았어야함 (-> 문제 제대로 읽고 뽑아야하는 값의 조건 캐치!)
- 같은 값을 검색할 때 조건문 1) [컬럼명] LIKE "문자열" 안하고 2) [컬럼명] ="문자열" 해도돼! 	 


  ```sql
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE ANIMAL_TYPE LIKE "DOG" AND NAME LIKE '%EL%'
ORDER BY NAME
  ```



#### 3. 중성화 여부 파악하기

- MySQL 에서 IF문 사용하기

  => SELECT IF(조건, TRUE면 출력될 값, FALSE면 출력될 값)

  ```sql
  SELECT ANIMAL_ID, NAME, IF(SEX_UPON_INTAKE LIKE '%Neutered%' OR SEX_UPON_INTAKE LIKE '%Spayed%', 'O', 'X') AS "중성화" FROM ANIMAL_INS ORDER BY ANIMAL_ID
  ```




#### 4. 오랜 기간 보호한 동물(2)

- DATE 타입끼리는 기본적인 연산이 가능!

  => ANIMAL_OUTS의 DATETIME 랑 ANIMAL_INS 의 DATETIME 차이 큰 값으로 내림차순 정렬

- LIMIT

  - 갯수 제한
  - MS SQL에서는 TOP명령어와 비슷
  
     => SELECT ~ FROM ~ WHERE ~ ORDER BY ASC|DESC LIMIT (뽑고 싶은 수 N)
  
     (LIMIT DEFAULT (0, N)) 상위 N개만 가져옴
  
     만약, LIMIT 50, 10 이렇게 쓰면 50부터 10개의 데이터 뽑아옴!
  
  ```sql
  SELECT A.ANIMAL_ID, A.NAME FROM ANIMAL_INS A, ANIMAL_OUTS B WHERE A.ANIMAL_ID = B.ANIMAL_ID ORDER BY B.DATETIME - A.DATETIME DESC LIMIT 2
  ```



#### 5. DATETIME에서 DATE로 형 변환

- DATE_FORMAT 함수

  => SELECT DATE_FORMAT(DATETIME(주어진 DATE관련 COLUMN명) , 출력하고 싶은 형태 ) 

  - %Y : 4자리 연도
  - %m : 숫자 월 (두자리)
  - %d : 일자 ( 두자리)

  ```sql
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') FROM ANIMAL_INS ORDER BY ANIMAL_ID
  ```
  
  

- 참고할 Date Format

  ![image](https://user-images.githubusercontent.com/47768081/103473214-7ae4f780-4dd9-11eb-9aa4-8623ebc6e4ad.png) 
