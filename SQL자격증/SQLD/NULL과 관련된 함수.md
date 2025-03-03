### **Oracle과 SQL Server(MSSQL)에서 사용 가능한 NULL 처리 함수 정리**  

| 함수 | **Oracle** | **SQL Server (MSSQL)** | 설명 |
|------|-----------|-----------------|------|
| **NULLIF(a, b)** | ✅ | ✅ | `a == b`이면 `NULL`, 아니면 `a` 반환 |
| **NVL(a, b)** | ✅ | ❌ | `a`가 `NULL`이면 `b` 반환 (Oracle 전용) |
| **COALESCE(a, b, c, ...)** | ✅ | ✅ | 첫 번째 `NULL`이 아닌 값 반환 |
| **ISNULL(a, b)** | ❌ | ✅ | `a`가 `NULL`이면 `b` 반환 (SQL Server 전용) |
| **CASE WHEN** | ✅ | ✅ | 복잡한 조건 처리 가능 |
| **DECODE(a, NULL, b, a)** | ✅ | ❌ | `a`가 `NULL`이면 `b` 반환 (`CASE WHEN`의 축약형, Oracle 전용) |

---

### ✅ **Oracle 전용**
1. `NVL(a, b)`
   ```sql
   SELECT NVL(column_name, '대체값') FROM table_name;
   ```
   - `column_name`이 `NULL`이면 `'대체값'` 반환.

2. `DECODE(a, NULL, b, a)`
   ```sql
   SELECT DECODE(column_name, NULL, '대체값', column_name) FROM table_name;
   ```
   - `column_name`이 `NULL`이면 `'대체값'` 반환.

---

### ✅ **SQL Server (MSSQL) 전용**
1. `ISNULL(a, b)`
   ```sql
   SELECT ISNULL(column_name, '대체값') FROM table_name;
   ```
   - `column_name`이 `NULL`이면 `'대체값'` 반환.

---

### ✅ **Oracle & SQL Server 공통**
1. `NULLIF(a, b)`
   ```sql
   SELECT NULLIF(column1, column2) FROM table_name;
   ```
   - `column1 == column2`이면 `NULL`, 아니면 `column1` 반환.

2. `COALESCE(a, b, c, ...)`
   ```sql
   SELECT COALESCE(column1, column2, '대체값') FROM table_name;
   ```
   - `column1`이 `NULL`이면 `column2`, `column2`도 `NULL`이면 `'대체값'` 반환.

3. `CASE WHEN`
   ```sql
   SELECT 
     CASE WHEN column_name IS NULL THEN '대체값' ELSE column_name END 
   FROM table_name;
   ```
   - `column_name`이 `NULL`이면 `'대체값'` 반환.

---

### **📌 정리**
- **Oracle 전용** → `NVL`, `DECODE`
- **SQL Server 전용** → `ISNULL`
- **Oracle & SQL Server 공통** → `NULLIF`, `COALESCE`, `CASE WHEN`
