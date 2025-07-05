`LATERAL`은 SQL에서 \*\*서브쿼리(또는 테이블 함수)\*\*가 **왼쪽 테이블의 값을 참조할 수 있도록** 해주는 키워드입니다. PostgreSQL, Oracle 12c 이상, SQL Server (CROSS APPLY/OUTER APPLY로 유사 기능) 등에서 지원됩니다.

---

## 🔍 기본 문법

```sql
SELECT ...
FROM table1
JOIN LATERAL (
    SELECT ...
    FROM ...
    WHERE table1.column = ...
) AS sub ON ...
```

---

## ✅ 왜 필요한가?

일반 서브쿼리에서는 **왼쪽 테이블의 컬럼을 사용할 수 없습니다.**
그러나 `LATERAL`을 사용하면 **왼쪽 행의 값을 기반으로 동적으로 서브쿼리를 실행**할 수 있습니다.

---

## 📌 예제 (PostgreSQL 기준)

### 예제 테이블

```sql
CREATE TABLE users (
  id INT,
  name TEXT
);

CREATE TABLE orders (
  user_id INT,
  product TEXT
);
```

### 1. LATERAL 없이

```sql
SELECT u.id, u.name,
       (SELECT o.product
        FROM orders o
        WHERE o.user_id = u.id
        LIMIT 1) AS first_product
FROM users u;
```

* 이건 가능하지만 `LIMIT`, `ORDER BY` 등 복잡한 작업을 여러 개 해야 할 때 불편합니다.

---

### 2. LATERAL 사용

```sql
SELECT u.id, u.name, o.product
FROM users u
LEFT JOIN LATERAL (
  SELECT product
  FROM orders
  WHERE orders.user_id = u.id
  ORDER BY product
  LIMIT 1
) o ON TRUE;
```

* 여기서 `LATERAL`을 써서 `orders.user_id = u.id`를 서브쿼리에서 참조할 수 있습니다.
* `ON TRUE`는 조건 없이 조인하겠다는 의미입니다 (필요에 따라 변경 가능).

---

## 💡 요약

| 항목                 | 설명                           |
| ------------------ | ---------------------------- |
| 기능                 | 왼쪽 테이블 값을 서브쿼리에서 사용할 수 있게 함  |
| 사용처                | 서브쿼리나 테이블 함수가 동적 참조 필요할 때    |
| 유사 기능 (SQL Server) | `CROSS APPLY`, `OUTER APPLY` |
| 대표 예               | 각 유저별 첫 주문 가져오기 등            |

