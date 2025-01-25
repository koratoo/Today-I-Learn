### SQL CASE WHEN 구문: 조건 순서의 중요성과 잘못된 사례

SQL에서 `CASE WHEN` 구문은 조건에 따라 다양한 결과를 반환할 수 있는 강력한 도구입니다. 하지만 조건 순서가 잘못되면 의도와 다른 결과를 초래할 수 있습니다. 이 글에서는 잘못된 조건 순서로 인해 발생할 수 있는 문제를 살펴보고, 이를 올바르게 수정하는 방법을 제시합니다.

#### 잘못된 SQL 예제
다음은 `CASE WHEN` 구문의 조건 순서가 잘못된 예제입니다:

```sql
SELECT
    student_name,
    score,
    CASE
        WHEN score >= 60 THEN 'D'
        WHEN score >= 70 THEN 'C'
        WHEN score >= 80 THEN 'B'
        WHEN score >= 90 THEN 'A'
        ELSE 'F'
    END AS grade
FROM student_scores;
```

이 코드에서 점수가 90 이상인 학생은 "A"가 아닌 "D"로 분류됩니다. 이는 `CASE WHEN` 구문이 첫 번째로 만족하는 조건을 반환하기 때문에 발생합니다. 위 코드에서는 `score >= 60` 조건이 먼저 작성되어 있어, 점수가 90 이상이더라도 "D"로 처리됩니다.

#### 올바른 SQL 예제
조건을 높은 점수부터 낮은 점수 순으로 정렬하여 문제를 해결할 수 있습니다:

```sql
SELECT
    student_name,
    score,
    CASE
        WHEN score >= 90 THEN 'A'
        WHEN score >= 80 THEN 'B'
        WHEN score >= 70 THEN 'C'
        WHEN score >= 60 THEN 'D'
        ELSE 'F'
    END AS grade
FROM student_scores;
```

이제 점수가 90 이상인 경우 "A"로 올바르게 분류됩니다. 높은 점수 조건이 먼저 평가되기 때문에 의도한 대로 결과가 나옵니다.

#### 핵심 요약
1. `CASE WHEN` 구문은 첫 번째로 만족하는 조건을 반환합니다.
2. 조건은 우선순위에 따라 정렬해야 합니다.
3. 높은 우선순위를 가지는 조건을 먼저 작성

SQL 쿼리를 작성할 때 조건 순서에 주의하면, 논리적 오류를 방지하고 원하는 결과를 얻을 수 있습니다. 

