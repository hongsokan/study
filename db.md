# DB

## ACID
> 데이터베이스 트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 성질  
> ([트랜잭션](https://victorydntmd.tistory.com/129) : 데이터에 대한 하나의 논리적 실행단계)

- A (Atomicity, 원자성) : All or Noting
- C (Consistency, 일관성) : 일관적인 DB상태를 유지
- I (Isolation, 격리성) : 트랜잭션끼리는 서로를 간섭할 수 없음
- D (Durafulation, 지속성) : 성공적으로 수행된 트랜잭션은 영원히 반영


## Isolation Level
> 트랜잭션이 서로 얼마나 고립되어 있는지 나타내는 것

- READ UNCOMMITTED
- READ COMMITTED
- REPEATABLE READ
- SERIALIZABLE


## [Oracle] 조인의 종류
- Nested Loop Join
- Sorted Merge Join
- Hash Join
> [참고자료](https://myjamong.tistory.com/238)


## [Oracle] 특정 테이블과 연결된 모든 테이블 목록 조회
```sql
SELECT fk.owner, fk.constraint_name , fk.table_name 
  FROM all_constraints fk, all_constraints pk 
 WHERE fk.R_CONSTRAINT_NAME = pk.CONSTRAINT_NAME 
   AND pk.owner = '계정명'
   AND fk.owner = '계정명'
   AND fk.CONSTRAINT_TYPE = 'R'
   AND pk.TABLE_NAME = '테이블명'
 ORDER BY fk.TABLE_NAME
 ```
 > [참고자료](https://hermeslog.tistory.com/55)
