## 1. 형 변환 함수
### CAST(데이터 AS 데이터형식), CONVERT(데이터, 데이터형식)
> 해당 데이터형식으로 형변환

```sql
-- 묵시적 형 변환
SELECT '100' + '200';
SELECT 1 > '2mega';
-- result --
399
0 -- 1 : true, 0 : false
```

## 2. 제어 흐름 함수

### IF(수식, 참, 거짓)
> 수식이 참 또는 거짓인지 결과에 따라서 분기하는 함수

### IFNULL(수식1, 수식2)
> 수식 1이 NULL이 아니면 수식 1이 변환, 수식 1이 NULL이면 수식 2 반환

```sql
SELECT IFNULL(NULL, '값이 없음');
SELECT NVL(NULL, '값이 없음'); -- ⛔10.3 버전부터 지원
```

### NVL2(수식1, 수식2, 수식3) ⛔10.3 버전부터 지원
> 수식1이 NULL 이면 수식2, 아니면 수식3 반환



## 3. 문자열 함수

### ASCII(문자), CHAR(숫자)
> 문자 <-> 숫자

### LENGTH
```sql
-- 영문 : 1Byte, 한글 : 3Byte(MariaDB)
SELECT BIT_LENGTH('ABC'), -- bit크기
       CHAR_LENGTH('ABC'), -- 문자 개수자
       LENGTH('ABC'); -- Byte 수
```

### CONCAT(문자열1, 문자열2, ...)
> 문자열을 이어줌
``` sql
SELECT CONCAT('2024', '12', '16');
-- CONCAT_WS 함수는 구분자와 함께 이어줌
SELECT CONCAT_WS('/', '2024', '12', '16');
-- result --
20241216
2024/12/16
```


### 문자열 위치 반환 함수

```sql
SELECT ELT(2, '하나', '둘', '셋');
SELECT FIELD('둘', '하나', '둘', '셋');
SELECT FIND_IN_SET('둘', '하나,둘,셋');
SELECT INSTR('하나 둘 셋', '둘'); -- ⛔띄어쓰기
SELECT LOCATE('둘', '하나 둘 셋');
-- result --
둘
2
4
3
4
```

### FORMAT(숫자, 소수점 자릿수)
> 지정된 소수점 자릿수까지 표현

```sql
-- 반올림, 3자리 콤마를 표시
SELECT FORMAT(123456789.789,2);
-- result --
123456789.79
```


### INSERT(기준 문자열, 위치, 길이, 삽입할 문자열)
> 기준 문자열의 위치부터 길이만큼 지우고 삽입할 문자열을 끼워 넣음

```sql
SELECT INSERT('abcdefghi', 3, 4, '####');
-- result --
ab####ghi
```

### LEFT, RIGHT(문자열, 길이)
> 왼쪽(오른쪽)에서 문자열의 길이만큼 반환

```sql
SELECT LEFT('abcdefg', 3);
-- reseult --
abc
```

### UPPER, LOWER(문자열)
> 소문자->대문자 (대문자-> 소문자)


### LPAD, RPAD(문자열, 길이,채울 문자열)
> 문자열을 길이만큼 왼쪽(오른쪽)으로 늘린 후, 빈 곳을 해당 문자열로 채움

```sql
SELECT LPAD('HELLO', 10); -- 💡3번째 매개 생략시 공백
```

### LTRIM, RTRIM, TRIM(문자열)
> 문자열의 공백을 제거 (cf>중간 공백 제거 x)


```sql
-- TRIM(<방향> <자를 문자열> FROM <문자열>) ⛔띄어쓰기로 구분
SELECT TRIM(BOTH 'Z' FROM 'ZZZHELLOZZZ');
SELECT TRIM(LEADING 'Z' FROM 'ZZZHELLOZZZ');
SELECT TRIM(TRAILING 'Z' FROM 'ZZZHELLOZZZ');
-- result --
HELLO
HELLOZZZ
ZZZHELLO
```

### REPEAT(문자열, 횟수)
> 문자열을 횟수만큼 반복

### REPLACE(문자열, 원래 문자열, 바꿀 문자열)
> 문자열에서 원래 문자열을 찾아 바꿀 문자열로 바꿈

### SPACE(길이)
> 길이 만큼의 공백 반환

### SUBSTRING, SUBSTR, MID
> 시작 위치부터 길이만큼 반환

```sql
SELECT SUBSTRING('대한민국만세', 3);
SELECT SUBSTRING('대한민국만세' FROM 3); -- ⛔띄어쓰기로 구분
SELECT SUBSTRING('대한민국만세', 3, 2);
SELECT SUBSTRING('대한민국만세' FROM 3 FOR 2);
SELECT SUBSTRING('대한민국만세', -2, 2); -- 💡음수는 오른쪽부터 셈
-- result --
한국만세
한국만세
민국
민국
만세
만세
```

### SUBSTRING_INDEX(문자열, 구분자, 횟수)
> 문자열에서 구분자가 왼쪽 횟수 번째 나오면 그 이후 버림(횟수가 음수이면 오른쪽부터 세고 왼쪽을 버림)

## 4.수학 함수

### 올림, 내림, 반올림
``` sql
SELECT CEILING(4.3); -- 올림
SELECT FlOOR(4.7); -- 내림
SELECT ROUND(4.5); -- 반올림
SELECT ROUND(4.335, 2); -- 2째자리까지 나타냄
SELECT ROUND(4.5, -1); --음수는 소수점 기준 왼쪽으로
```

### TRUNCATE(숫자, 정수)
> 숫자를 소수점 기준 정수 위치까지 구하고 나머지는 버림

```sql
SELECT TRUNCATE(123.456, 0);
SELECT TRUNCATE(123.456, 2);
-- result --
123
123.45
```

### MOD(숫자1, 숫자2)
> 숫자1을 숫자 2로 나눈 나머지

### RAND()
> 0 ~ 1 미만의 실수

## 5.날짜/시간 함수

### ADDDATE(날짜, 차이),, SUBDATE(날짜, 차이),
> 날짜 기준 차이를 더한(뺀) 날짜 변환
>>  DATE_ADD(),  SUBDATE()와 동일
```sql
SELECT ADDDATE()
```

### ADDTIME(날짜/시간, 시일), SUBTIME(날짜/시간, 시간)
> 날짜/시간을 기준으로 시간을 더한(뺀) 결과를 반환

```sql
SELECT ADDDATE()
```

### 날짜, 시간 정보
```sql
SELECT CURDATE(), -- 현재 연-월-일
       CURTIME(), -- 현재 시-분-초
       NOW(), -- 현재 연-원-일 시:분:초
       SYSDATE(); -- 상동
```

### 날짜 또는 시간 추출 함수
```sql
SELECT YEAR(CURDATE()),
       MONTH(CURDATE()),
       DAYOFMONTH(CURDATE()), -- 일(DAY()함수와 같음)
       DATE(NOW); -- 연-월-일 정보를 추출 

SELECT HOUR(CURTIME()),
       MINUTE(CURTIME()),
       SECOND(CURTIME()),
       MICROSECOND(CURTIME()),
       TIME(NOW()); -- 시-분-초 정보를 추출
```

### 날짜/시간 차이
```sql
SELECT DATEDIFF(CURDATE(), '2024-05-05'), -- 시간 차이
       TIMEDIFF(CURDATE(), '09:00:00'); -- 날짜 차이
```

### 날짜 반화
```sql
SELECT DAYOFWEEK(CURDATE()), -- 날짜의 요일을 숫자로 변환(1: 월...)
       MONTHNAME(CURDATE()), -- 날짜의 월이름 변환
       DAYOFYEAR(CURDATE()), -- 연도에서 정수만큼 지난 날짜
       LAST_DAY(CURDATE()); -- 주어진 날짜의 달의 마지막 날짜
```
### 지난 날짜/시간
```sql
SELECT MAKEDATE(2025, 100), -- 2025년에서 100일째 되는 날
       MAKETIME(22, 58, 55), -- 22:58:55
       PERIOD_ADD(202505, 11), -- 25년 05월에 11개월 이후
       PERIOD_DIFF(202505, 2024505); -- 25년 5월 - 24년 5월
```

### QUATER(날짜)
> 분기 정보 반환

### TIME_TO_SEC(시간)
> 시간을 초 단위로 반환

## 6. 윈도우 함수
> 행과 행 사이의 관계를 위한 함수

### A. 순위 함수
> 데이터를 순서대로 번호 매기거나, 특정 조건에 따라 순위 매기기 가능

#### a. ROW_NUMBER()
> 순번 매


```sql
-- ORDER BY height DESC 키로 내림차순 정렬
-- 이후 ROW_NUMBER()로 순번을 매김
SELECT ROW_NUMBER() OVER(ORDER BY height DESC);
```

#### b. RANK()
>

#### c. DENSE_RANK()
>

#### d. NTILE(n)
> 전체 인원을 키 순서로 세운 후에 n개의 그룹으로 분할

### B. 분석 함수
> 다른 행의 데이터를 참조하는 함수

#### a. LEAD(a, n), LAG(a, n)
> n행 이후(이전)의 행

```sql
SELECT LEAD(height, 1) OVER (ORDER BY height desc);
```

#### b. FIRST_VALUE(), LAST_VALUE()
> 가장 큰(작은) 값

```sql
SELECT FIRST_VALUE(height) OVER(PARTITION BY addr ORDER BY height DESC); -- 지역별
```
