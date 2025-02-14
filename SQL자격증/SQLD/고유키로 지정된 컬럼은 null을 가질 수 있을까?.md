## 2장 15번 테이블의 제약조건

### ✅ SQL Server에서 고유 키(UNIQUE KEY)와 NULL 값  
SQL Server에서 **고유 키(UNIQUE KEY)** 는 **NULL 값을 가질 수 있습니다.**  
하지만, **기본 키(PRIMARY KEY)** 는 **NULL 값을 가질 수 없습니다.**

---

## 🔹 UNIQUE 제약 조건과 NULL 값  

- **UNIQUE 제약 조건은 중복된 값을 허용하지 않지만, NULL은 예외**  
- SQL Server에서는 **UNIQUE 제약 조건이 적용된 컬럼에 NULL 값이 여러 개 존재할 수 있음**  
  - 단, **UNIQUE INDEX가 적용된 컬럼에서는 NULL 값이 한 개만 허용됨** (SQL Server의 동작 방식)  

---

## 🎯 예제 1: UNIQUE 컬럼에서 NULL 값 허용  
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE  -- UNIQUE 제약 조건이 있어도 NULL 가능
);

INSERT INTO employees (id, email) VALUES (1, 'user1@example.com');
INSERT INTO employees (id, email) VALUES (2, 'user2@example.com');
INSERT INTO employees (id, email) VALUES (3, NULL); -- NULL 허용됨
INSERT INTO employees (id, email) VALUES (4, NULL); -- NULL 허용됨 (SQL Server에서는 UNIQUE에도 NULL 여러 개 가능)
```
- `email` 컬럼은 UNIQUE 제약 조건이 있지만, **NULL 값은 여러 개 삽입 가능함**.

---

## 🎯 예제 2: PRIMARY KEY에서는 NULL 불가능  
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,  -- PRIMARY KEY는 NULL 불가능
    email VARCHAR(100) UNIQUE
);

INSERT INTO employees (id, email) VALUES (NULL, 'user3@example.com'); -- ❌ 오류 발생 (PRIMARY KEY는 NULL 불가능)
```
- **PRIMARY KEY 컬럼은 NULL 값을 허용하지 않음**.

---

## 🔥 UNIQUE INDEX를 사용한 경우  
SQL Server에서는 **UNIQUE INDEX** 를 적용하면 NULL 값이 하나만 허용됨.  

### 📌 UNIQUE INDEX에서 NULL이 하나만 허용됨  
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    email VARCHAR(100) NULL
);

CREATE UNIQUE INDEX UQ_email ON employees(email);

INSERT INTO employees (id, email) VALUES (1, 'user1@example.com');
INSERT INTO employees (id, email) VALUES (2, 'user2@example.com');
INSERT INTO employees (id, email) VALUES (3, NULL); -- ✅ 첫 번째 NULL 가능
INSERT INTO employees (id, email) VALUES (4, NULL); -- ❌ 오류 발생 (UNIQUE INDEX로 인해 NULL 값 중복 불가)
```
- **UNIQUE 제약 조건에서는 NULL 여러 개 허용**  
- **UNIQUE INDEX가 있으면 NULL 값 하나만 허용됨**  

---

## 🔥 정리  
| 제약 조건 | NULL 허용 여부 | 중복 NULL 허용 여부 |
|-----------|--------------|----------------|
| **PRIMARY KEY** | ❌ (허용 안 됨) | ❌ (0개 허용) |
| **UNIQUE KEY** | ✅ (허용됨) | ✅ (여러 개 가능) |
| **UNIQUE INDEX** | ✅ (허용됨) | ❌ (하나만 허용) |

✅ **즉, UNIQUE 제약 조건은 NULL을 여러 개 허용하지만, UNIQUE INDEX가 적용되면 NULL이 하나만 허용됨!** 🚀
