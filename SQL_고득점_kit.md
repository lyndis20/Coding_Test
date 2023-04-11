예전에 다 풀었었는데, 새로운 문제가 더 추가되었다!!
복습할 겸, 처음부터 쭉 풀어보려고 한다.
MYSQL 기준 풀이입니다.

### SQL 문제풀때 주의사항
-  (as) 출력할 **컬럼명 바꾸는 것** 잊지 말기
- 컬럼명 결과와 일치시키기! (대문자, 소문자 주의)
- 문제에 나와있는 조건 빼먹지 않기
- limit 숫자 걸어서 확인해 본 다음, 제출할땐 지우기
- order by 정렬 기준 빼먹지 않기

## SELECT

인기있는 아이스크림
```sql
SELECT FLAVOR
FROM FIRST_HALF
ORDER BY TOTAL_ORDER DESC, SHIPMENT_ID 
```

평균 일일 대여 요금 구하기
```sql
SELECT ROUND(AVG(daily_fee),0) as AVERAGE_FEE
FROM CAR_RENTAL_COMPANY_CAR
WHERE car_type = 'SUV'
```

강원도에 위치한 생산공장 목록 출력하기
```sql
SELECT factory_id, factory_name, address
FROM Food_Factory
WHERE address LIKE '강원도%'
ORDER BY factory_id asc
```
> MySQL 와일드 카드
* %: 글자수 상관없이 검색 
* _: 언더바 개수만큼의 글자수만 검색
* [ ]: 대괄호 안 문자만 검색 (부정은 ^, 범위는 -) 

3월에 태어난 여성 회원 목록 출력하기
```sql
SELECT MEMBER_ID, MEMBER_NAME, GENDER, DATE_FORMAT(date_of_birth,'%Y-%m-%d') as DATE_OF_BIRTH
FROM member_profile
WHERE MONTH(date_of_birth) = 3 
AND TLNO IS NOT NULL
AND GENDER = 'W'
ORDER BY member_id ASC
```
조건에 맞는 도서 리스트 출력하기
```sql
SELECT BOOK_ID, date_format(PUBLISHED_DATE,'%Y-%m-%d') as PUBLISHED_DATE
FROM BOOK
WHERE year(PUBLISHED_DATE) = 2021 AND CATEGORY = '인문'
ORDER BY PUBLISHED_DATE ASC
```
> DATE_FORMAT(날짜,'형식')


서울에 위치한 식당 목록 출력하기
```sql
SELECT info.REST_ID, info.REST_NAME, info.FOOD_TYPE, FAVORITES, ADDRESS, ROUND(AVG(review.review_score),2) as SCORE
FROM REST_INFO as info
INNER JOIN REST_REVIEW as review on info.rest_id = review.rest_id
WHERE info.address LIKE '서울%'
GROUP BY info.rest_id
ORDER BY score desc, Favorites desc
```
12세 이하인 여자 환자 목록 출력하기
> NULL값 처리: NVL기능을 하는 IFNULL(컬럼, 'null값을 대체할 값')
```sql
SELECT PT_NAME, PT_NO, GEND_CD, AGE, IFNULL(TLNO,'NONE') TLNO
FROM patient
WHERE age <= 12 
AND GEND_CD = 'W'
order by age desc, PT_NAME asc
```
흉부외과 또는 일반외과 의사 목록 출력하기
> Date_format(날짜, '%형식')
```sql
SELECT DR_NAME, DR_ID, MCDP_CD, DATE_FORMAT(HIRE_YMD, '%Y-%m-%d') HIRE_YMD
FROM DOCTOR
WHERE MCDP_CD = 'CS' OR MCDP_CD = 'GS' 
ORDER BY HIRE_YMD DESC, DR_NAME ASC
```

조건에 맞는 회원수 구하기
> YEAR(), MONTH(), DAY() 함수 사용하기
```sql
SELECT COUNT(*) AS USERS
FROM USER_INFO
WHERE AGE >= 20 
AND AGE <= 29
AND YEAR(JOINED) = 2021
```
재구매가 일어난 상품과 회원 리스트 구하기
```sql
SELECT user_id, product_id
FROM online_sale
GROUP BY user_id, product_id
HAVING count(product_id) >=2
ORDER BY user_id asc, product_id desc
```

```sql
-- 간단 버전
SELECT user_id, product_id
FROM online_sale
GROUP BY 1, 2
HAVING count(product_id) >=2
ORDER BY 1, 2 desc
```

오프라인/온라인 판매 데이터 통합하기 (lv.4)
> 복잡했던 문제!!
> UNION을 이용해서 온라인/오프라인 데이터를 위아래로 붙여서 가져오기

> 오프라인 데이터엔 user_id가 없으므로 NULL 값으로 채움 (NULL as user_id)
> 이때 "NULL"이라고 따옴표를 쓰면 오답이 되기 때문에 주의할 것!!

> 2022년 3월의 데이터만 가져오기
> 날짜 형식을 date_format으로 맞춰주었고, 풀이에는 BETWEEN을 사용 (시간 까지 쓰는 것 주의하기)
> WHERE YEAR(sales_date) = 2022 AND MONTH(sales_date)=3 도 가능!
```sql
SELECT date_format(sales_date,"%Y-%m-%d") as SALES_DATE, PRODUCT_ID, USER_ID, SALES_AMOUNT
FROM online_sale
where sales_date BETWEEN '2022-03-01 00:00:00' AND '2022-03-31 23:59:59'
UNION
SELECT date_format(sales_date,"%Y-%m-%d") as sales_date, product_id, NULL as user_id, sales_amount
FROM offline_sale
where sales_date BETWEEN '2022-03-01 00:00:00' AND '2022-03-31 23:59:59'
order by sales_date, product_id, user_id
```

## SUM, MAX
가장 비싼 상품 구하기
```sql
SELECT MAX(PRICE) as MAX_PRICE
FROM PRODUCT
```
가격이 제일 비싼 식품의 정보 출력하기
- 서브쿼리를 활용한 방식 () 쓰는 것 잊지말기
```sql
SELECT * 
FROM FOOD_PRODUCT 
WHERE PRICE = (SELECT MAX(PRICE) FROM FOOD_PRODUCT)
```
다른 풀이 (order by, Limit 사용)
```sql
SELECT *
FROM FOOD_PRODUCT
ORDER BY PRICE DESC LIMIT 1;
```
## GROUP BY
진료과별 총 예약 횟수 출력하기
```sql
SELECT MCDP_CD as 진료과코드, COUNT(APNT_NO) as 5월예약건수
FROM APPOINTMENT
WHERE MONTH(APNT_YMD) = 5
GROUP BY MCDP_CD 
ORDER BY 2 asc, 1 asc
```

