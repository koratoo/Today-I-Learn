### **CUBE와 ROLLUP 개념**
`CUBE`와 `ROLLUP`은 **OLAP(Online Analytical Processing) 분석을 위해 그룹화(Group By)된 데이터의 다양한 집계(Aggregation)를 계산하는 기능**이야.  
둘 다 `GROUP BY`의 확장 개념으로, 다차원 집계를 쉽게 수행할 수 있도록 돕는 SQL 기능이야.

---

## **1. ROLLUP**
### **(1) 개념**
- `ROLLUP`은 계층적인 수준에서 부분 합계를 자동으로 생성하는 기능이야.
- `GROUP BY` 절에서 다중 그룹핑을 처리할 때, **계층적으로 요약된 데이터를 제공**해.
- 마지막 단계에서는 전체 합계(Total Summary)도 함께 포함돼.

### **(2) 예제**
#### **📌 기본 데이터**
```sql
CREATE TABLE sales (
    year INT,
    region VARCHAR(10),
    sales_amount INT
);

INSERT INTO sales VALUES (2023, 'Seoul', 100);
INSERT INTO sales VALUES (2023, 'Busan', 200);
INSERT INTO sales VALUES (2024, 'Seoul', 150);
INSERT INTO sales VALUES (2024, 'Busan', 250);
```
#### **📌 일반적인 GROUP BY**
```sql
SELECT year, region, SUM(sales_amount) AS total_sales
FROM sales
GROUP BY year, region;
```
✅ **출력 결과**
```
year  | region | total_sales
------+--------+------------
2023  | Seoul  | 100
2023  | Busan  | 200
2024  | Seoul  | 150
2024  | Busan  | 250
```
#### **📌 ROLLUP 적용**
```sql
SELECT year, region, SUM(sales_amount) AS total_sales
FROM sales
GROUP BY ROLLUP(year, region);
```
✅ **출력 결과**
```
year  | region | total_sales
------+--------+------------
2023  | Seoul  | 100
2023  | Busan  | 200
2023  | NULL   | 300  -- (2023년 전체 합계)
2024  | Seoul  | 150
2024  | Busan  | 250
2024  | NULL   | 400  -- (2024년 전체 합계)
NULL  | NULL   | 700  -- (전체 합계)
```
✔ **특징**
- 연도별(region 포함) 총합이 계산됨
- 연도 전체(region 없이) 합산 값이 추가됨
- 최종적으로 전체 데이터의 합산 값이 출력됨

---

## **2. CUBE**
### **(1) 개념**
- `CUBE`는 `ROLLUP`과 비슷하지만, **가능한 모든 그룹 조합에 대한 부분 합계를 계산**해.
- 즉, `ROLLUP`보다 더 다양한 수준의 집계가 포함됨.

### **(2) 예제**
```sql
SELECT year, region, SUM(sales_amount) AS total_sales
FROM sales
GROUP BY CUBE(year, region);
```
✅ **출력 결과**
```
year  | region | total_sales
------+--------+------------
2023  | Seoul  | 100
2023  | Busan  | 200
2023  | NULL   | 300  -- (2023년 전체 합계)
2024  | Seoul  | 150
2024  | Busan  | 250
2024  | NULL   | 400  -- (2024년 전체 합계)
NULL  | Seoul  | 250  -- (Seoul 전체 합계)
NULL  | Busan  | 450  -- (Busan 전체 합계)
NULL  | NULL   | 700  -- (전체 합계)
```
✔ **특징**
- `ROLLUP`보다 더 다양한 부분 합계가 제공됨.
- **지역(region) 단위의 총합도 추가됨** (`NULL | Seoul | 250`, `NULL | Busan | 450`)
- **연도별(region 없이) 총합도 제공됨** (`2023 | NULL | 300`, `2024 | NULL | 400`)
- **전체 합계도 포함됨** (`NULL | NULL | 700`)

---

## **3. ROLLUP vs CUBE 비교**
| 특징  | ROLLUP | CUBE |
|------|--------|------|
| 부분 합계 개수 | 계층적인 일부만 포함 | 가능한 모든 조합 포함 |
| 주 사용 사례 | 계층적인 요약 데이터 (ex. 연도별, 지역별) | 다차원 분석을 위한 모든 조합 제공 |
| 추가 합계 계산 | 최상위 집계값 (Total Summary) 포함 | 중간 단계 및 전체 합계 포함 |

✔ **ROLLUP은 계층적인 합계를 계산할 때 적합하고, CUBE는 다차원 분석이 필요한 경우 사용하면 좋아!** 😊
