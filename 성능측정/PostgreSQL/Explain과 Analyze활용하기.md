# SQL 성능 분석의 시작: EXPLAIN과 ANALYZE 활용하기

데이터베이스를 사용하다 보면 쿼리의 성능을 분석해야 하는 경우가 많습니다. 특히, 대량의 데이터를 다루거나 복잡한 조인(join) 연산이 포함된 경우에는 실행 속도가 느려질 수 있습니다. 이러한 문제를 해결하기 위해 **EXPLAIN**과 **ANALYZE**를 활용하는 방법을 알아보겠습니다.

## 1. EXPLAIN이란?
**EXPLAIN**은 SQL 쿼리의 실행 계획을 미리 분석하여 어떻게 실행될지를 보여줍니다. 실행 계획에는 사용된 인덱스, 조인 방식, 예상 실행 비용 등이 포함됩니다.

### 사용 예시:
```sql
EXPLAIN SELECT * FROM employees;
```
위 명령어를 실행하면 `employees` 테이블에서 데이터를 조회할 때의 실행 계획이 출력됩니다.

## 2. ANALYZE란?
**ANALYZE**는 실제 쿼리를 실행하면서 실행 시간과 통계를 제공하는 명령어입니다. 즉, 예상치가 아니라 실제 실행된 데이터를 기반으로 성능을 분석할 수 있습니다.

### 사용 예시:
```sql
ANALYZE SELECT * FROM employees;
```
이 명령어는 `employees` 테이블의 데이터를 실제로 조회하면서 실행 통계를 수집합니다.

## 3. EXPLAIN ANALYZE 조합하기
`EXPLAIN`과 `ANALYZE`를 함께 사용하면 예상 실행 계획과 실제 실행 결과를 비교할 수 있습니다.

### 사용 예시:
```sql
EXPLAIN ANALYZE SELECT * FROM employees;
```
실행하면 다음과 같은 정보가 출력됩니다:
- **실제 실행 시간** (Execution Time)
- **예상 실행 비용** (Estimated Cost)
- **사용된 인덱스** (Indexes Used)
- **조회된 행 수** (Rows Retrieved)

이 정보를 활용하면, 쿼리 성능을 최적화할 수 있습니다.

## 4. 실행 계획 읽는 법
EXPLAIN ANALYZE의 실행 결과를 해석하는 방법을 간략히 설명하면 다음과 같습니다.

```sql
Seq Scan on employees  (cost=0.00..15.70 rows=570 width=244) (actual time=0.010..0.040 rows=10 loops=1)
```
- **Seq Scan**: 순차 검색(Sequential Scan) 방식으로 테이블을 조회함
- **cost=0.00..15.70**: 쿼리 실행 비용(낮을수록 좋음)
- **rows=570**: 예상되는 결과 행 수
- **actual time=0.010..0.040**: 실제 실행 시간 (단위: ms)
- **loops=1**: 실행 반복 횟수

## 5. 쿼리 최적화 방법
EXPLAIN ANALYZE를 통해 성능이 저하된 원인을 파악한 후, 다음과 같은 방법으로 최적화할 수 있습니다.

1. **인덱스 추가**
   - WHERE 조건절에 자주 사용되는 컬럼에 인덱스를 생성
   ```sql
   CREATE INDEX idx_name ON employees(name);
   ```
2. **조인 최적화**
   - 필요하지 않은 조인을 제거하거나 적절한 인덱스를 활용
3. **LIMIT 사용**
   - 불필요한 전체 조회 대신 필요한 데이터만 가져오기
   ```sql
   SELECT * FROM employees LIMIT 10;
   ```
4. **EXPLAIN 결과 분석**
   - Seq Scan 대신 Index Scan이 사용되도록 조정
   - 예상 행 수와 실제 조회 행 수의 차이를 확인하여 통계 갱신

## 6. 마무리
EXPLAIN과 ANALYZE는 SQL 쿼리의 성능을 분석하고 최적화하는 데 필수적인 도구입니다. 이를 활용하여 실행 계획을 확인하고, 인덱스 추가나 조인 방식 변경 등을 통해 최적화하면 보다 빠르고 효율적인 데이터베이스 운영이 가능합니다.

쿼리 성능이 느리다고 느껴진다면, 먼저 EXPLAIN ANALYZE를 실행해 보고 문제의 원인을 분석해보세요!

