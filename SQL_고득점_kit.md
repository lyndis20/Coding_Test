예전에 다 풀었었는데, 새로운 문제가 더 추가되었다!!
복습할 겸, 처음부터 쭉 풀어보려고 한다.


SQL 문제풀때 주의사항
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
