`DUAL`은 Oracle에서 **임시로 1개의 행을 반환하는 테이블**로, **SELECT문을 쓸 때 반드시 FROM 절이 필요한 규칙 때문에** 만들어진 특수한 테이블입니다.

### 언제 쓰는가?
- **계산 결과만 출력하고 싶을 때**
- **함수 실행 결과만 보고 싶을 때**
- **시스템 값(예: SYSDATE, USER 등)을 조회할 때**

---

### 예시
```sql
SELECT 1 + 1 FROM dual;
-- 결과: 2
```

```sql
SELECT SYSDATE FROM dual;
-- 결과: 현재 날짜
```

```sql
SELECT 'Hello' || ' World' FROM dual;
-- 결과: Hello World
```

---

### 왜 꼭 필요했을까?
Oracle SQL 문법상 `SELECT`는 항상 `FROM` 절이 필요해서,  
**테이블이 없을 때도 임시로 사용할 대상**이 필요했기 때문입니다.

---

### 참고: 다른 DB에서는?
- **MySQL, PostgreSQL, SQL Server**에서는 `FROM dual` 없이 그냥 `SELECT`만 써도 됩니다:
```sql
-- MySQL 예시
SELECT 1 + 1;
```

Oracle만의 특징이라고 보면 됩니다.
