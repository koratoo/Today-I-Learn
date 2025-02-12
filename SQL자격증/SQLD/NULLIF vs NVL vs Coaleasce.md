`NVL`, `NULLIF`, `COALESCE`는 모두 SQL에서 NULL 값을 처리하는 함수지만, 각각의 동작 방식이 다릅니다.

---

## 1. **NVL (NULL Value Logic)**
- **Oracle에서만 사용 가능** (PostgreSQL, MySQL, SQL Server 등에서는 지원되지 않음)
- NULL이면 대체 값을 반환하는 함수
- **형식:**  
  ```sql
  NVL(value, replacement)
  ```
- **설명:** `value`가 NULL이면 `replacement` 값을 반환하고, NULL이 아니면 `value`를 그대로 반환함.
- **예제:**
  ```sql
  SELECT NVL(NULL, '대체값') FROM dual;  -- 결과: '대체값'
  SELECT NVL('데이터', '대체값') FROM dual; -- 결과: '데이터'
  ```

- **주의점:** `NVL`의 두 번째 인자는 `value`와 같은 데이터 타입이어야 함. (암시적 변환 가능)

---

## 2. **NULLIF**
- 모든 DBMS에서 사용 가능 (Oracle, PostgreSQL, MySQL, SQL Server 등)
- **두 개의 값이 같으면 NULL을 반환**, 다르면 첫 번째 값을 그대로 반환
- **형식:**
  ```sql
  NULLIF(value1, value2)
  ```
- **설명:**  
  - `value1`과 `value2`가 같으면 `NULL` 반환  
  - 다르면 `value1` 반환
- **예제:**
  ```sql
  SELECT NULLIF(10, 10);  -- 결과: NULL
  SELECT NULLIF(10, 20);  -- 결과: 10
  ```
- **활용 예시:**  
  - 나누기 연산에서 0을 방지하는 데 사용될 수 있음
  ```sql
  SELECT 100 / NULLIF(column_value, 0) FROM table_name;
  ```
  → `column_value`가 `0`이면 `NULL`이 반환되어 `0으로 나누기 오류`를 방지할 수 있음.

---

## 3. **COALESCE**
- 모든 DBMS에서 사용 가능 (Oracle, PostgreSQL, MySQL, SQL Server 등)
- **여러 개의 값 중 NULL이 아닌 첫 번째 값을 반환**
- **형식:**
  ```sql
  COALESCE(value1, value2, value3, ...)
  ```
- **설명:**  
  - 왼쪽부터 순차적으로 NULL이 아닌 값을 찾고, 찾으면 즉시 반환  
  - 모든 값이 NULL이면 NULL 반환
- **예제:**
  ```sql
  SELECT COALESCE(NULL, NULL, '대체값', '다른값');  -- 결과: '대체값'
  SELECT COALESCE(NULL, '첫 번째 NULL 아님', '두 번째 NULL 아님'); -- 결과: '첫 번째 NULL 아님'
  ```
- **활용 예시:**  
  - 여러 개의 컬럼 중 값이 있는 첫 번째 컬럼을 반환하는 데 사용됨
  ```sql
  SELECT COALESCE(address1, address2, '주소 없음') FROM customers;
  ```

---

## 🔥 **차이점 정리**
| 함수     | 지원 DBMS | 동작 방식 |
|----------|----------|----------|
| `NVL`    | Oracle 전용 | `value`가 NULL이면 `replacement` 반환 |
| `NULLIF` | 모든 DBMS | `value1 == value2`이면 NULL 반환, 다르면 `value1` 반환 |
| `COALESCE` | 모든 DBMS | NULL이 아닌 첫 번째 값을 반환 |

---

## ❓ **어떤 경우에 어떤 걸 써야 할까?**
- **NULL을 다른 값으로 대체하고 싶을 때** → `NVL` (Oracle) 또는 `COALESCE` (모든 DBMS)
- **두 값이 같으면 NULL을 반환하고 싶을 때** → `NULLIF`
- **여러 개의 값 중 NULL이 아닌 첫 번째 값을 찾을 때** → `COALESCE`

### ✨ **추천**
- **Oracle 전용이 아니라면** `NVL` 대신 `COALESCE`를 사용하는 것이 좋음.
- **NULL 처리 최적화 시** `COALESCE`가 `NVL`보다 유연함 (여러 개의 값을 받을 수 있기 때문).
- **NULLIF는 특정 비교 연산에서 NULL을 유도할 때 사용**.
