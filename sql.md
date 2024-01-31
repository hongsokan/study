## [참고도서](https://m.hanbit.co.kr/store/books/book_view.html?p_code=B8289488788)

## SQL문 종류
| 종류 | 명령어 | 
| ------ | ------ |
| DML(Manipulation, 조작어) | SELECT INSERT UPDATE DELETE |
| DDL(Definition, 정의어 | CREATE ALTER DROP RENAME |
| DCL(Control, 제어어) | GRANT REVOKE |
| TCL(Transaction Control, 트랜젝션 제어) | COMMIT ROLLBACK |

## 대표적 데이터형 4가지
| 데이터형 | 설명 |
| ------ | ------ |
| CHAR(L) | 고정 문자열 (나머지는 공백으로 채움) |
| VARCHAR2(L) | 가변 문자열 (데이터만큼 공간 차지, 공백도 문자로 취급) |
| NUMBER(L, D) | 정수 및 실수 저장 (L: 전체수, D: 소수점수) ex. NUMBER(3, 2) : 9.99까지 저장 |
| DATE | 날짜와 시간정보 |

## 제약조건
| 제약조건 | 설명 |
| ------ | ------ |
| 기본키 | 식별 값, 유일 인덱스 자동 생성, UNIQUE + NOT NULL |
| 고유키 | 식별 값, NULL 가능, UNIQUE + NULL |
| NOT NULL | NULL 불가 |
| CHECK | 값의 종류 혹은 범위 제한 |
| 외래키 | 다른 테이블의 기본키 참조 (참조 무결성 제약조건) |
| 기본값 | 디폴트 값 지정 |

---

## DDL
### CREATE TABLE
```sql
CREATE TABLE TB_RN_TMP --도로명임시
(
  RN_CD VARCHAR2(12) NOT NULL --도로명코드
, RN VARCHAR2(150) NOT NULL --도로명
);
```

#### 예제
```sql
--부모 테이블 생성
CREATE TABLE TB_SUBWAY_STATN_TMP --지하철역임시
(
  SUBWAY_STATN_NO CHAR(6) NOT NULL --지하철역번호
, LN_NM VARCHAR2(50) NOT NULL --노선명
, STATN_NM VARCHAR2(50) NOT NULL --역명
, CONSTRAINT PK_TB_SUBWAY_STATN_TMP PRIMARY KEY(SUBWAY_STATN_NO) -- 제약조건 종류명 지정
);

--자식 테이블 생성
CREATE TABLE TB_SUBWAY_STATN_TK_GFF_TMP --지하철역승하차임시
(
  SUBWAY_STATN_NO CHAR(6) NOT NULL --지하철역번호
, STD_YM CHAR(6) NOT NULL --기준년월
, BEGIN_TIME CHAR(4) NOT NULL --시작시간
, END_TIME CHAR(4) NOT NULL --종료시간
, TK_GFF_SE_CD VARCHAR2(6) NOT NULL --승하차구분코드
, TK_GFF_CNT NUMBER(15) NOT NULL --승하차횟수
, CONSTRAINT PK_TB_SUBWAY_STATN_TK_GFF_TMP PRIMARY KEY (SUBWAY_STATN_NO, STD_YM, BEGIN_TIME, END_TIME, TK_GFF_SE_CD)
);

--코드 5-4 외래키 생성
ALTER TABLE TB_SUBWAY_STATN_TK_GFF_TMP --지하철역승하차임시 테이블에
ADD CONSTRAINT FK_TB_SUBWAY_STATN_TK_GFF_TMP1 --참조 무결성 제약조건을 생성
FOREIGN KEY (SUBWAY_STATN_NO) --지하철역승하차임시 테이블의 지하철역번호 칼럼은
--지하철역임시 테이블의 지하철역번호를 참조
REFERENCES TB_SUBWAY_STATN_TMP (SUBWAY_STATN_NO);
```

---

### ALTER TABLE
```sql
ALTER TABLE TB_SUBWAY_STATN_TMP ADD (OPER_YN CHAR(1)); --운영여부 칼럼 추가
ALTER TABLE TB_SUBWAY_STATN_TMP DROP COLUMN OPER_YN; -- 칼럼 제거
ALTER TABLE TB_SUBWAY_STATN_TMP ADD (OPER_YN CHAR(1) NULL); -- 칼럼 추가생성 (NULL 허용)
ALTER TABLE TB_SUBWAY_STATN_TMP MODIFY(OPER_YN NUMBER(1) DEFAULT 0 NOT NULL NOVALIDATE); -- 칼럼 변경
-- NOVALIDATE: NULL인 칼럼도 NOT NULL 제약조건을 줄 수 있게 해주는 옵션
```

```sql
-- 외래키 제거
ALTER TABLE TB_SUBWAY_STATN_TK_GFF_TMP
DROP CONSTRAINT FK_TB_SUBWAY_STATN_TK_GFF_TMP1;
-- 외래키 생성
ALTER TABLE TB_SUBWAY_STATN_TK_GFF_TMP --지하철역승하차임시 테이블에
ADD CONSTRAINT FK_TB_SUBWAY_STATN_TK_GFF_TMP1 --참조 무결성 제약조건을 생성
FOREIGN KEY ( SUBWAY_STATN_NO ) --지하철역승하차임시 테이블의 지하철역번호 칼럼은
--지하철역임시 테이블의 지하철역번호를 참조
REFERENCES TB_SUBWAY_STATN_TMP (SUBWAY_STATN_NO );
```

```sql
-- 테이블명 변경
RENAME TB_SUBWAY_STATN_TK_GFF_TMP TO TB_SUBWAY_STATN_TK_GFF_TMP_2;
```

```sql
-- 테이블 내 데이터 제거 (ROLLBACK 불가)
TRUNCATE TABLE TB_SUBWAY_STATN_TK_GFF_TMP_2;

-- 테이블 제거
DROP TABLE TB_SUBWAY_STATN_TK_GFF_TMP_2;
DROP TABLE TB_SUBWAY_STATN_TMP;
DROP TABLE TB_RN_TMP;
```

---

## DML
### INSERT
```sql
INSERT INTO TB_SUBWAY_STATN_TMP T
(
  T.SUBWAY_STATN_NO
, T.LN_NM
, T.STATN_NM
)
VALUES
(
  '000032'
, '2호선'
, '강남'
)
;

COMMIT; --DB에 최종적으로 적용(커밋)
```

### UPDATE
```sql
UPDATE TB_SUBWAY_STATN_TMP A
   SET A.LN_NM = '녹색선'
     , A.STATN_NM = '강남스타일'
 WHERE A.SUBWAY_STATN_NO = '000032' --역명이 '강남'인 행
;

COMMIT;
```

### DELETE
```sql
DELETE
  FROM TB_SUBWAY_STATN_TMP A
 WHERE A.SUBWAY_STATN_NO = '000032'
;

COMMIT;
```

### SELECT
```sql
SELECT A.INDUTY_CL_CD
     , A.INDUTY_CL_NM
     , A.INDUTY_CL_SE_CD
     , NVL(A.UPPER_INDUTY_CL_CD, '(Null)') AS UPPER_INDUTY_CL_CD
  FROM TB_INDUTY_CL A
 WHERE INDUTY_CL_SE_CD = 'ICS001' --대
;

--합성 연산자의 사용
SELECT SUBWAY_STATN_NO || '-' || STATN_NM ||'('||LN_NM ||')' AS "지하철역번호-역명(노선명)"
  FROM TB_SUBWAY_STATN
 WHERE LN_NM = '9호선'
;
```

### DISTINCT : 중복행 제거
```sql
--DISTINCT - 1개 칼럼
SELECT DISTINCT A.INDUTY_CL_SE_CD
  FROM TB_INDUTY_CL A
;

--DISTINCT - 2개 칼럼
SELECT DISTINCT A.POPLTN_SE_CD, A.AGRDE_SE_CD
  FROM TB_POPLTN A
;
```

### DUAL 연산
```sql
SELECT ( (1+1)*3 ) / 6 AS "연산결과 값"
  FROM DUAL
;
```

---

## TCL (COMMIT, ROLLBACK)
- 트랜젝션 : DB의 논리적 연산 단위

### COMMIT
```sql
UPDATE TB_SUBWAY_STATN
   SET STATN_NM = '역삼역'
 WHERE STATN_NM = '역삼'
;

--COMMIT 전에는 DB에 적용되지 않음
SELECT SUBWAY_STATN_NO
     , LN_NM
     , STATN_NM
  FROM TB_SUBWAY_STATN
 WHERE STATN_NM = '역삼역'
;

--커밋 실행 후 DB에 최종 적용
COMMIT;
```

### ROLLBACK
```sql
UPDATE TB_SUBWAY_STATN
   SET STATN_NM = '역삼역'
 WHERE STATN_NM = '역삼'
;

--롤백 실행
ROLLBACK;
```

### SAVEPOINT : SAVEPOINT까지의 트랜젝션만 적용
```sql
SAVEPOINT SVPT1; --첫 번째 savepoint

UPDATE TB_SUBWAY_STATN
   SET STATN_NM = '역삼역'
 WHERE STATN_NM = '역삼'
;

SAVEPOINT SVPT2; --두 번째 savepoint

UPDATE TB_SUBWAY_STATN
   SET STATN_NM = '강남역'
 WHERE STATN_NM = '강남'
;

SAVEPOINT SVPT3; --세 번째 savepoint

UPDATE TB_SUBWAY_STATN
   SET STATN_NM = '방배역'
 WHERE STATN_NM = '방배'
;

ROLLBACK TO SVPT3; -- SVPT3까지 롤백
```

---

### WHERE
```sql
SELECT A.INDUTY_CL_CD
     , A.INDUTY_CL_NM
     , A.INDUTY_CL_SE_CD
     , A.UPPER_INDUTY_CL_CD
  FROM TB_INDUTY_CL A
 WHERE INDUTY_CL_NM LIKE '%커피%'
;
```

## 비교연산자
| 연산자 | 의미 |
| ------ | ------ |
| = | 같다 |
| > | 크다 |
| >= | 크거나 같다 |
| < | 작다 |
| <= | 작거나 같다 |

## SQL 연산자
| 연산자 | 의미 |
| ------ | ------ |
| BETWEEN A AND B | A와 B 사이 |
| IN (리스트) | 리스트 중 하나라도 포함 |
| LIKE | 비교문자열의 형태와 일치 |
| IS NULL | 값이 NULL |
| IS NOT NULL | 값이 NOT NULL |
```sql
SELECT A.SUBWAY_STATN_NO
     , A.STD_YM
     , A.BEGIN_TIME
     , A.END_TIME
     , A.TK_GFF_SE_CD
     , A.TK_GFF_CNT
  FROM TB_SUBWAY_STATN_TK_GFF A
 WHERE A.SUBWAY_STATN_NO = '000032' --2호선 강남
   AND A.STD_YM = '202010' --2020년 10월
   AND A.BEGIN_TIME LIKE '18%'
   AND A.END_TIME LIKE '19%'
   AND A.TK_GFF_SE_CD IN ('TGS001', 'TGS002') --TGS001:승차, TGS002:하차
;
```

## 와일드카드
| 종류 | 설명 |
| ------ | ------ |
| % | 0개 이상의 어떤 문자 |
| _(언더바) | 1개의 단일 문자 |

## 논리 연산자
| 연산자 | 의미 |
| ------ | ------ |
| AND | 앞 뒤 조건 모두 참 |
| OR | 앞 뒤 조건 중 하나라도 참 |
| NOT | 조건이 거짓 |

## 부정 연산자
| 연산자 | 의미 |
| ------ | ------ |
| != | 같지 않음 |
| <> | 같지 않음 |
| ^= | 같지 않음 |
| NOT A = B | A는 B와 같지 않음 |
| NOT A > B | A는 B보다 크지 않음 |
| NOT A < B | A는 B보다 작지 않음 |

## 부정 SQL 연산자
| 연산자 | 의미 |
| ------ | ------ |
| NOT BETWEEN A AND B | A와 B 값 사이에 있지 않음 |
| NOT IN (LIST) | 같지 않음 |
| IS NOT NULL | NULL 값이 아님 |

## CHAR VS VARCHAR2
> CHAR는 공백을 문자로 취급하지 않음. VARCHAR2는 공백도 문자로 취급.
```sql
-- CHAR 데이터형 간 "=" 연산자 비교
SELECT CHAR_NO
     , REPLACE(CHAR_4, ' ', '_') AS CHAR_4 --공백이 있는 경우 '_'로 치환
     , REPLACE(CHAR_5, ' ', '_') AS CHAR_5 --공백이 있는 경우 '_'로 치환
  FROM TB_CHAR
 WHERE CHAR_4 = 'SQLD'
   AND CHAR_4 = CHAR_5
;
```
> 1001 | SQLD | SQLD_
>> CHAR 데이터 끼리 비교 시 **길이가 달라도 공백만 다른 경우 같음.**

```sql
-- CHAR 데이터형 간 "＜" 연산자 비교
SELECT CHAR_NO
     , REPLACE(CHAR_4, ' ', '_') AS CHAR_4 --공백이 있는 경우 '_'로 치환
     , REPLACE(CHAR_5, ' ', '_') AS CHAR_5 --공백이 있는 경우 '_'로 치환
  FROM TB_CHAR
 WHERE CHAR_NO = '1002'
   AND CHAR_4 < CHAR_5
;
```
> 1002 | SQLD | SQLP_
>> 'D'보다 'P'가 더 크기 때문에CHAR_NO = 1002 출력. **공백은 결과집합에 영향을 주지 않음.**

```sql
-- CHAR 타입 'SQLD'과 VARCHAR2 타입 'SQLD ' 비교
SELECT REPLACE(CHAR_4, ' ', '_') AS CHAR_4
     , REPLACE(VARCHAR2_5, ' ', '_') AS VARCHAR2_5
  FROM TB_VARCHAR2
 WHERE VARCHAR_NO = '1001'
   AND CHAR_4 = VARCHAR2_5
;
```
> 공집합이 리턴됨. VARCHAR2 타입인 'SQLD '은 공백도 문자로 판단하기 때문에 다른 문자로 인식.

## ROWNUM
```sql
-- WHERE절에 ROWNUM 이용
SELECT ROWNUM AS RNUM
     , A.BSSH_NO
     , A.CMPNM_NM
     , NVL(A.BHF_NM, '(Null)') AS BHF_NM
     , A.LNM_ADRES
  FROM TB_BSSH A
 WHERE ROWNUM <= 10
;
```

## 함수
### 단일행 함수 : 내장 함수 중 단 하나의 출력값을 리턴
| 종류 | 설명 | 주요 단일행 함수 |
| ------ | ------ | ------ |
| 문자형 | 문자 입력 시 문자나 숫자 값 반환 | LOWER, UPPER, SUBSTR, LENGTH, TRIM, ASCII |
| 숫자형 | 숫자 입력 시 숫자 값 반환 | ABS, MOD, ROUND, TRUNC |
| 날짜형 | DATE 타입 값 연산 | SYSDATE, EXTRACT, TO_NUMBER |
| 변환형 | 문자, 숫자, 날짜형을 다른 데이터형으로 변환 | TO_CHAR, TO_DATE, CONVERT |
| NULL 관련형 | NULL 처리 함수 | NVL, NULLIF, COALESCE |

## GROUP BY : 집계 함수는 GROUP BY 절에 기재한 칼럼을 기준으로 집계
```sql
SELECT A.POPLTN_SE_CD
     , SUM(A.POPLTN_CNT) "인구수합계"
  FROM TB_POPLTN A
 WHERE A.STD_YM = '202010'
GROUP BY A.POPLTN_SE_CD
;
```
