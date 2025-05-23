`in list iterator`는 주로 **SQL 쿼리에서 `IN` 조건절에 다수의 값을 반복적으로 넣는 방식**을 의미하며, 이 과정에서 옵티마이저가 선택하는 **실행 계획(Execution Plan)** 중 하나입니다. 특히 오라클(Oracle) DB에서 자주 언급됩니다.

---

### 🔍 예시 쿼리

```sql
SELECT * FROM emp WHERE deptno IN (10, 20, 30);
```

위 쿼리는 내부적으로 이렇게 처리될 수 있습니다:

* 각각 `deptno = 10`, `deptno = 20`, `deptno = 30`으로 분리해서 반복 수행하는 방식으로 실행됨
* 이때 사용되는 내부 반복 방식이 `INLIST ITERATOR`입니다

---

### 🧠 INLIST ITERATOR란?

\*\*`INLIST ITERATOR`\*\*는 옵티마이저가 다음과 같이 판단했을 때 사용하는 실행 계획 구성 요소입니다:

* `IN (...)` 리스트 안의 값들을 하나씩 꺼내서
* **같은 인덱스 스캔 방법을 반복적으로 재사용**하면서 실행

이 방식은 특히 **인덱스 조건에 포함된 컬럼에 대해 동일한 방식으로 효율적으로 검색**할 수 있을 때 사용됩니다.

---

### 📋 특징 요약

| 항목    | 설명                                                               |
| ----- | ---------------------------------------------------------------- |
| 목적    | IN 조건의 리스트 값을 하나씩 반복해 실행                                         |
| 성능    | 각 값에 대해 인덱스를 재사용하므로 효율적                                          |
| 사용 시점 | 옵티마이저가 IN 값 개수가 적당하고 인덱스를 활용할 수 있다고 판단할 때                        |
| 장점    | 반복 수행 대신 내부적으로 최적화된 루프 사용                                        |
| 확인 방법 | `EXPLAIN PLAN` 또는 `AUTOTRACE` 결과에서 `INLIST ITERATOR`라는 키워드 확인 가능 |

---

### 🔧 실행 계획 예시 (오라클)

```text
---------------------------------------------
| Id | Operation         | Name      |
---------------------------------------------
|  0 | SELECT STATEMENT  |           |
|  1 | INLIST ITERATOR   |           |
|  2 | INDEX RANGE SCAN  | EMP_IDX01 |
---------------------------------------------
```

`INLIST ITERATOR` 아래에 `INDEX RANGE SCAN`이 있는 걸 보면, 같은 인덱스를 여러 번 재사용해서 IN 조건을 처리한다는 뜻입니다.

