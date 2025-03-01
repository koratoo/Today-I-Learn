PostgreSQL에서 일별, 주별, 월별, 분기별 매출액을 구하려면 `DATE_TRUNC()` 함수를 활용하면 효율적으로 그룹화할 수 있어.

## 1. 기본 테이블 예시
먼저, 매출 데이터가 들어 있는 테이블 구조를 예로 들자.

```sql
CREATE TABLE sales (
    id SERIAL PRIMARY KEY,
    sale_date DATE NOT NULL,
    amount NUMERIC NOT NULL
);
```

## 2. 일별 매출
```sql
SELECT DATE(sale_date) AS period, SUM(amount) AS total_sales
FROM sales
GROUP BY period
ORDER BY period;
```
- `DATE(sale_date)`는 날짜만 추출하여 그룹화함.
- 매출 합계를 `SUM(amount)`로 계산.

---

## 3. 주별 매출
```sql
SELECT DATE_TRUNC('week', sale_date) AS period, SUM(amount) AS total_sales
FROM sales
GROUP BY period
ORDER BY period;
```
- `DATE_TRUNC('week', sale_date)`를 사용하면 해당 주의 월요일로 변환됨.
- 주별 매출 합계를 구함.

---

## 4. 월별 매출
```sql
SELECT DATE_TRUNC('month', sale_date) AS period, SUM(amount) AS total_sales
FROM sales
GROUP BY period
ORDER BY period;
```
- `DATE_TRUNC('month', sale_date)`는 해당 월의 1일로 변환됨.
- 월별 매출 합계를 구함.

---

## 5. 분기별 매출
```sql
SELECT DATE_TRUNC('quarter', sale_date) AS period, SUM(amount) AS total_sales
FROM sales
GROUP BY period
ORDER BY period;
```
- `DATE_TRUNC('quarter', sale_date)`는 해당 분기의 첫 번째 달의 1일로 변환됨.
- 분기별 매출 합계를 구함.

---

## 6. 일, 주, 월, 분기 매출을 한 번에 구하기
```sql
SELECT 
    DATE(sale_date) AS daily_sales,
    DATE_TRUNC('week', sale_date) AS weekly_sales,
    DATE_TRUNC('month', sale_date) AS monthly_sales,
    DATE_TRUNC('quarter', sale_date) AS quarterly_sales,
    SUM(amount) AS total_sales
FROM sales
GROUP BY 1, 2, 3, 4
ORDER BY 1;
```
