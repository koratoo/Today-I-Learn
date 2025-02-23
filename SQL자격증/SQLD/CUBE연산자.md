### SQL의 `CUBE` 연산자란?

`CUBE`는 **SQL의 그룹화 연산자** 중 하나로, 여러 차원의 데이터를 그룹화하여 **모든 가능한 집계 조합**을 생성하는 기능을 수행합니다.  
주로 **OLAP(Online Analytical Processing)** 환경에서 다차원 분석을 할 때 사용됩니다.

---

## 1. `CUBE`의 기본 개념
`CUBE`는 `GROUP BY`와 유사하지만, 지정한 컬럼들의 **모든 조합에 대한 그룹화된 결과를 생성**합니다.

예를 들어, `GROUP BY A, B`는 `(A, B)`에 대한 그룹만 생성하지만,  
`GROUP BY CUBE(A, B)`는 다음과 같은 조합을 포함한 모든 그룹을 생성합니다.

| A   | B   | 집계 결과 |
|-----|-----|----------|
| A1  | B1  |  값 1   |
| A1  | B2  |  값 2   |
| A2  | B1  |  값 3   |
| A2  | B2  |  값 4   |
| A1  | NULL |  값 5   |  → `A1`에 대한 합계
| A2  | NULL |  값 6   |  → `A2`에 대한 합계
| NULL | B1  |  값 7   |  → `B1`에 대한 합계
| NULL | B2  |  값 8   |  → `B2`에 대한 합계
| NULL | NULL |  값 9   |  → 전체 합계

즉, `CUBE(A, B)`를 실행하면:
- `(A, B)` 각각에 대한 개별 그룹
- `A` 단위로 그룹화된 결과 (`B`가 `NULL`)
- `B` 단위로 그룹화된 결과 (`A`가 `NULL`)
- 전체 합계 (`A`, `B`가 둘 다 `NULL`)

이런 방식으로 **다차원 집계를 쉽게 수행**할 수 있습니다.

---

## 2. `CUBE` 예제

### 2.1 예제 데이터

```sql
CREATE TABLE sales (
    region VARCHAR(50),
    product VARCHAR(50),
    sales_amount INT
);

INSERT INTO sales VALUES 
('서울', 'A', 100),
('서울', 'B', 200),
('부산', 'A', 150),
('부산', 'B', 250);
```

### 2.2 `CUBE`를 사용한 집계

```sql
SELECT 
    region, 
    product, 
    SUM(sales_amount) AS total_sales
FROM sales
GROUP BY CUBE(region, product);
```

### 2.3 실행 결과

| region | product | total_sales |
|--------|---------|------------|
| 서울   | A       | 100        |
| 서울   | B       | 200        |
| 부산   | A       | 150        |
| 부산   | B       | 250        |
| 서울   | NULL    | 300        |  (서울 전체 합계)  
| 부산   | NULL    | 400        |  (부산 전체 합계)  
| NULL   | A       | 250        |  (A 제품 전체 합계)  
| NULL   | B       | 450        |  (B 제품 전체 합계)  
| NULL   | NULL    | 700        |  (전체 합계)  

위 결과에서 `NULL` 값은 해당 차원을 제외한 그룹별 합계를 의미합니다.

---

## 3. `CUBE` vs `ROLLUP` vs 일반 `GROUP BY`

| 연산자    | 기능 |
|-----------|--------------------|
| `GROUP BY` | 지정된 컬럼의 **한 가지 조합만** 그룹화 |
| `ROLLUP`  | 지정된 컬럼의 **계층적 합계를 생성** (부분 합계를 포함하지만 다차원 조합은 생성하지 않음) |
| `CUBE`    | 지정된 컬럼의 **모든 조합에 대해 그룹화된 결과를 생성** |

### 차이점 예제
```sql
SELECT region, product, SUM(sales_amount) 
FROM sales
GROUP BY GROUPING SETS ((region, product), (region), (product), ());
```
위 `GROUPING SETS`는 `CUBE`와 유사하지만, 명시적으로 필요한 조합만 지정하는 방식입니다.

---

## 4. `CUBE`가 유용한 경우
- OLAP 분석에서 **다차원 집계**가 필요할 때
- 데이터를 다양한 기준으로 요약하여 보고서를 생성할 때
- `GROUP BY`만으로는 불가능한 다양한 조합의 집계를 수행해야 할 때

---

### 정리
- `CUBE`는 **다차원 그룹화를 수행**하여 모든 가능한 집계 조합을 자동으로 생성
- `ROLLUP`과 유사하지만, **완전한 조합을 제공**하는 것이 차이점
- OLAP 및 BI(Business Intelligence) 분석에서 **매우 유용한 SQL 기능**

궁금한 점 있으면 질문해 줘! 😊
