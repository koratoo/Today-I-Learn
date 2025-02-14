## [노랭이 책 과목2 7번]

SQL Server에서 **한 번에 여러 컬럼의 정의(데이터 타입, 제약 조건 등)를 변경하는 것**은 일부 제한이 있습니다.  

### ✅ 여러 컬럼을 한 번에 변경하는 방법  
SQL Server에서는 `ALTER TABLE` 문을 사용하여 컬럼의 정의를 변경할 수 있지만, **여러 개의 컬럼을 한 번의 `ALTER COLUMN` 문으로 수정할 수는 없습니다.**  
즉, **각 컬럼마다 개별적으로 `ALTER COLUMN`을 사용해야 합니다.**  

---

## 🎯 컬럼 정의 변경 예제  

### 📌 ❌ 여러 컬럼을 한 번에 수정 (불가능)  
```sql
ALTER TABLE employees
ALTER COLUMN salary DECIMAL(10,2), 
ALTER COLUMN department NVARCHAR(50);
```
- SQL Server에서는 **이런 방식으로 여러 컬럼을 한 번에 수정할 수 없음.**
- 각각 따로 `ALTER COLUMN`을 실행해야 함.

---

### 📌 ✅ 여러 컬럼을 순차적으로 수정 (가능)  
```sql
ALTER TABLE employees
ALTER COLUMN salary DECIMAL(10,2); -- salary 컬럼을 DECIMAL(10,2)로 변경

ALTER TABLE employees
ALTER COLUMN department NVARCHAR(50); -- department 컬럼을 NVARCHAR(50)로 변경
```
- `ALTER COLUMN`은 **한 번에 하나의 컬럼만 변경 가능**하므로, 여러 번 실행해야 함.

---

## ✅ 컬럼 추가, 삭제는 여러 개 동시에 가능  

### 📌 ✅ 여러 개의 컬럼 추가  
```sql
ALTER TABLE employees
ADD hire_date DATE, 
    is_active BIT;
```
- 새로운 컬럼을 여러 개 추가하는 것은 가능함.

---

### 📌 ✅ 여러 개의 컬럼 삭제  
```sql
ALTER TABLE employees
DROP COLUMN hire_date, is_active;
```
- 여러 컬럼을 한 번에 삭제할 수도 있음.

---

## 🔥 정리  
| 작업 유형 | 여러 컬럼 동시에 변경 가능 여부 |
|-----------|------------------|
| **데이터 타입 변경** (`ALTER COLUMN`) | ❌ (각 컬럼마다 따로 실행해야 함) |
| **컬럼 추가** (`ADD COLUMN`) | ✅ (한 번에 여러 개 가능) |
| **컬럼 삭제** (`DROP COLUMN`) | ✅ (한 번에 여러 개 가능) |

즉, **컬럼 추가/삭제는 여러 개 한 번에 가능하지만, 컬럼 정의 변경(`ALTER COLUMN`)은 하나씩만 가능**합니다. 🎯
