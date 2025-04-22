먼저, 오라클(Oracle) SQL에서 `BETWEEN` 문법은 **특정 범위 내에 값이 포함되는지** 확인할 때 사용하는 조건문입니다.

---

✅ **기본 문법:**

```sql
SELECT 컬럼명
FROM 테이블명
WHERE 컬럼명 BETWEEN 값1 AND 값2;
```

- `값1`과 `값2` 사이의 값을 포함합니다. 즉, **경계값(값1, 값2)도 포함됩니다.**
- 숫자, 날짜, 문자 모두 사용 가능합니다.

---

🧾 **예제1: 숫자 범위 조회**

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 3000 AND 5000;
```

> 💡 salary가 3000 이상, 5000 이하인 직원들을 조회합니다.

---

🗓️ **예제2: 날짜 범위 조회**

```sql
SELECT *
FROM orders
WHERE order_date BETWEEN TO_DATE('2024-01-01', 'YYYY-MM-DD')
                     AND TO_DATE('2024-12-31', 'YYYY-MM-DD');
```

> 💡 2024년 한 해 동안의 주문 내역을 조회합니다.

---

🔤 **예제3: 문자 범위 조회**

```sql
SELECT *
FROM products
WHERE product_name BETWEEN 'A' AND 'M';
```

> 💡 제품명이 A부터 M 사이(사전순)의 제품들을 조회합니다.

---

🧠 **주의사항**
- `BETWEEN A AND B`는 `A <= 컬럼 <= B`와 동일합니다.
- **범위 순서를 바꾸면 결과가 달라질 수 있으므로** 항상 작은 값 → 큰 값 순으로 적는 것이 좋습니다.

---
