### PostgreSQL의 `COALESCE` 함수

`COALESCE`는 **NULL 값을 대체하는 함수**로, 여러 개의 인수를 받아서 **첫 번째로 NULL이 아닌 값을 반환**합니다. 

#### ✅ 기본 문법
```sql
COALESCE(value1, value2, ..., valueN)
```
- `value1, value2, ..., valueN` 중에서 **NULL이 아닌 첫 번째 값을 반환**합니다.
- 만약 모든 값이 NULL이라면 `NULL`을 반환합니다.

---

### 📌 `COALESCE` 사용 예제

#### 1️⃣ NULL 값 대체하기
```sql
SELECT COALESCE(NULL, '대체값', '다른값');
```
🔹 결과: `'대체값'`  
  - 첫 번째 값이 NULL이므로, 두 번째 값 `'대체값'`이 반환됩니다.

#### 2️⃣ 컬럼 값이 NULL일 경우 기본값 설정
```sql
SELECT name, COALESCE(phone, '전화번호 없음') AS phone
FROM customers;
```
- `phone` 컬럼이 NULL이면 `'전화번호 없음'`을 반환합니다.

#### 3️⃣ 여러 개의 컬럼 중 NULL이 아닌 값 선택
```sql
SELECT COALESCE(home_address, work_address, '주소 없음') AS address
FROM users;
```
- `home_address`가 NULL이면 `work_address`를 반환.
- 둘 다 NULL이면 `'주소 없음'` 반환.

---

### ⚠️ `COALESCE` 주의점

#### ❌ 성능 문제 (INDEX 사용 제한)
`COALESCE`를 **WHERE 절**에서 사용하면 **인덱스를 제대로 활용하지 못하는 문제**가 발생할 수 있습니다.
```sql
SELECT * FROM users WHERE COALESCE(phone, 'NONE') = '010-1234-5678';
```
🔺 위처럼 `COALESCE`를 `WHERE`에 사용하면, **모든 행을 조회**해야 하므로 성능 저하 가능성이 있습니다.

✅ **대안: `OR` 사용**
```sql
SELECT * FROM users WHERE phone = '010-1234-5678' OR phone IS NULL;
```
- 이렇게 하면 **인덱스를 활용**할 수 있어 더 빠르게 조회할 수 있습니다.

#### ❌ `COALESCE` vs `CASE WHEN`
- `CASE WHEN`이 더 **유연하고 최적화된 선택**이 될 수 있습니다.
```sql
SELECT 
    CASE 
        WHEN phone IS NOT NULL THEN phone
        ELSE '전화번호 없음'
    END AS phone
FROM users;
```
- **`COALESCE`는 간결하지만, 복잡한 조건이 필요할 땐 `CASE WHEN`을 고려하는 것이 좋습니다.**

---

### 📌 `COALESCE` vs `NULLIF` 차이점
- `COALESCE`: NULL을 다른 값으로 대체
- `NULLIF`: 두 값이 같으면 NULL 반환

```sql
SELECT NULLIF(10, 10); -- NULL 반환
SELECT COALESCE(NULL, 10, 20); -- 10 반환
```

---

### ✅ 정리
| 함수 | 설명 |
|------|------|
| `COALESCE(a, b, c)` | NULL이 아닌 첫 번째 값 반환 |
| `NULLIF(a, b)` | a와 b가 같으면 NULL 반환 |
| `CASE WHEN` | 더 유연한 조건 처리 가능 |

💡 `COALESCE`는 NULL을 다룰 때 유용하지만, 성능 문제를 고려해야 합니다! 🚀
