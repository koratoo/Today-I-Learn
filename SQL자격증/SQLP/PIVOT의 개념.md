## 🔍 피벗(PIVOT)의 개념

> **PIVOT = 행 → 열 변환**

예를 들어, 월별로 부서별 급여 합계를 보고 싶을 때,  
행으로 나오는 월(month) 정보를 열(column)로 바꿔서 보기 쉽게 만드는 겁니다.

---

## 📊 예시 데이터

```sql
-- 급여 테이블
| EMPNO | ENAME | DEPTNO | SAL  | MONTH |
|-------|-------|--------|------|-------|
| 7369  | SMITH | 10     | 800  | JAN   |
| 7499  | ALLEN | 10     | 1600 | FEB   |
| 7521  | WARD  | 20     | 1250 | JAN   |
| 7566  | JONES | 20     | 2975 | FEB   |
```

---

## 🛠️ 오라클 PIVOT 구문

```sql
SELECT * FROM (
    SELECT DEPTNO, SAL, MONTH
    FROM EMP
)
PIVOT (
    SUM(SAL)
    FOR MONTH IN ('JAN' AS JAN, 'FEB' AS FEB)
);
```

---

## ✅ 결과

| DEPTNO | JAN  | FEB  |
|--------|------|------|
| 10     | 800  | 1600 |
| 20     | 1250 | 2975 |

→ `MONTH`가 행에 있었는데, 열로 바뀌었죠? 이게 바로 **PIVOT**입니다!

---

## 🔁 반대는 UNPIVOT

- `PIVOT`은 **행 → 열**
- `UNPIVOT`은 **열 → 행**

필요에 따라 자유롭게 변환할 수 있습니다.

---

### 🔔 요약

| 기능 | 설명 |
|------|------|
| `PIVOT` | 행 데이터를 열로 바꿔서 보기 쉽게 함 |
| `UNPIVOT` | 열 데이터를 다시 행으로 바꿈 |
| 사용처 | 보고서, 통계 요약, 월별 비교 등 |

---

오라클의 `PIVOT`은 복잡한 집계 작업을 간단하게 표현할 수 있는 **강력한 기능**입니다!  
더 많은 SQL 예제와 튜토리얼은 [GPT Online](https://gptonline.ai/ko/)에서 확인해보세요 😊
