### **SQL에서 `NULL`이란?**
`NULL`은 **알 수 없음(Unknown)** 또는 **값 없음(No Value)** 을 의미한다. 이는 단순히 `0`이나 `''`(빈 문자열)과는 다른 개념이다.

---

## **1. NULL의 주요 특징**
1. **값이 아니다**  
   - `NULL`은 어떤 값이 아니다. 따라서 `=` 연산자로 비교할 수 없다.
   - 예제:  
     ```sql
     SELECT * FROM users WHERE age = NULL;  -- 잘못된 사용 (항상 false)
     SELECT * FROM users WHERE age IS NULL; -- 올바른 사용
     ```

2. **연산 결과는 항상 NULL이 된다**  
   - `NULL`이 포함된 연산은 대부분 `NULL`을 반환한다.
   - 예제:  
     ```sql
     SELECT 10 + NULL;  -- 결과: NULL
     SELECT CONCAT('Hello ', NULL); -- 결과: NULL
     ```

3. **비교 연산에서 예외 처리 필요**
   - `NULL`을 포함한 비교 연산(`=, !=, <, >`)은 `UNKNOWN` 결과를 반환한다.
   - 따라서 `NULL`을 체크할 때는 `IS NULL` 또는 `IS NOT NULL`을 사용해야 한다.
   - 예제:  
     ```sql
     SELECT * FROM employees WHERE salary IS NOT NULL;
     ```

4. **집계 함수(Aggregate Functions)와 NULL**  
   - `COUNT(column)`: `NULL`을 제외한 개수를 반환한다.
   - `SUM(column)`, `AVG(column)`, `MAX(column)`, `MIN(column)`: `NULL` 값을 무시하고 연산한다.
   - 예제:
     ```sql
     SELECT COUNT(age) FROM users;  -- NULL을 제외한 개수
     SELECT AVG(salary) FROM employees; -- NULL을 제외하고 평균 계산
     ```

---

## **2. NULL 처리 방법**
### **1) `COALESCE` 함수**
- `NULL`을 다른 값으로 변환할 때 사용한다.
- 예제:
  ```sql
  SELECT name, COALESCE(age, 0) AS age FROM users;
  ```

### **2) `IFNULL` (MySQL) / `NVL` (Oracle)**
- `NULL`이면 대체 값을 반환한다.
- 예제:
  ```sql
  SELECT name, IFNULL(age, 18) FROM users;  -- MySQL
  SELECT name, NVL(age, 18) FROM users;  -- Oracle
  ```

### **3) `CASE` 문을 이용한 처리**
- 예제:
  ```sql
  SELECT name,
         CASE WHEN age IS NULL THEN 'Unknown' ELSE age END AS age
  FROM users;
  ```

---

## **3. NULL 관련 실수 및 주의점**
### **1) `=`로 비교하지 말 것**
```sql
SELECT * FROM users WHERE age = NULL; -- 잘못된 사용
SELECT * FROM users WHERE age IS NULL; -- 올바른 사용
```

### **2) `NOT IN`에서 주의할 것**
```sql
SELECT * FROM users WHERE age NOT IN (20, 30, NULL); -- 결과 없음!
```
- `NULL`이 포함된 `NOT IN` 조건은 항상 `UNKNOWN`을 반환하여 결과가 나오지 않을 수 있다.
- 해결 방법:
  ```sql
  SELECT * FROM users WHERE age NOT IN (20, 30) OR age IS NULL;
  ```

### **3) `DISTINCT` 사용 시 NULL 값이 하나로 처리됨**
```sql
SELECT DISTINCT age FROM users;
```
- `NULL`이 여러 개 있어도 `DISTINCT`에서는 하나의 `NULL`만 출력된다.

---

## **결론**
- `NULL`은 **값이 없는 상태**를 의미하며, 단순한 `0`이나 `빈 문자열`과 다르다.
- `=`이 아닌 `IS NULL` 또는 `IS NOT NULL`을 사용해야 한다.
- `NULL`과의 연산은 대부분 `NULL`을 반환하므로 `COALESCE`, `IFNULL`, `NVL` 등을 활용하여 대체할 필요가 있다.
