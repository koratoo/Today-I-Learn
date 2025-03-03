`NULLIF`는 두 개의 표현식이 동일하면 `NULL`을 반환하고, 그렇지 않으면 첫 번째 값을 그대로 반환하는 SQL 함수야. 주로 **0으로 나누기 오류 방지**나 **중복된 값 처리** 같은 상황에서 사용해.

---

### 1. **0으로 나누기 오류 방지**
`NULLIF`를 사용해서 나누는 값이 `0`일 경우 `NULL`을 반환하도록 하면, 0으로 나누는 오류를 방지할 수 있어.

```sql
SELECT column1 / NULLIF(column2, 0) AS result
FROM table_name;
```
- `column2`가 `0`이면 `NULLIF(column2, 0)`이 `NULL`을 반환해서 `0으로 나누기` 오류가 발생하지 않음.
- 단, 결과가 `NULL`이 될 수 있으니 `COALESCE(result, 0)`을 사용해 기본값을 지정할 수도 있음.

```sql
SELECT COALESCE(column1 / NULLIF(column2, 0), 0) AS result
FROM table_name;
```

---

### 2. **중복된 값 처리**
두 개의 컬럼 값이 같으면 `NULL`을 반환하도록 설정할 수 있어.

```sql
SELECT NULLIF(price, discount_price) AS final_price
FROM products;
```
- `price`와 `discount_price`가 같으면 `NULL`을 반환하고, 다르면 `price` 값을 그대로 반환.

---

### 3. **CASE WHEN을 간단하게 대체**
보통 `CASE WHEN`으로 처리할 수 있는 로직을 `NULLIF`로 간결하게 만들 수 있어.

```sql
-- CASE WHEN 사용
SELECT CASE WHEN status = 'inactive' THEN NULL ELSE status END AS adjusted_status
FROM users;

-- NULLIF 사용
SELECT NULLIF(status, 'inactive') AS adjusted_status
FROM users;
```
- `status`가 `'inactive'`이면 `NULL`을 반환, 아니면 원래 값을 반환.

---

### 📌 정리
- `NULLIF(a, b)`: `a == b`이면 `NULL`, 아니면 `a` 반환
- **0으로 나누기 방지** (`NULLIF(column, 0)`)
- **동일한 값이면 NULL로 처리** (`NULLIF(price, discount_price)`)
- **CASE WHEN을 간결하게 대체 가능**
