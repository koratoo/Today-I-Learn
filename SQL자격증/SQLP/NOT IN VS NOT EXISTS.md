
### 🔍 `NOT IN` vs `NOT EXISTS`의 차이점 요약

| 항목 | NOT IN | NOT EXISTS |
|------|--------|------------|
| **NULL 처리** | NULL이 포함되면 **전혀 결과를 반환하지 않음** | NULL의 영향을 받지 않음 |
| **서브쿼리 평가 방식** | 서브쿼리를 모두 평가한 후 조건 비교 | 조건을 하나씩 평가 (존재 여부만 확인) |
| **성능** | 작은 테이블에서는 괜찮지만, NULL 존재 시 주의 필요 | 일반적으로 더 안전하고 유연함 |
| **추천 사용처** | NULL이 절대 없다고 확신할 때 | NULL이 있을 가능성이 있을 때 |

---

### 🔸 예시로 보는 차이

#### ✅ 정상 작동하는 경우

```sql
-- IN 사용
SELECT * FROM EMP WHERE DEPTNO IN (20, 30);

-- OR 사용
SELECT * FROM EMP WHERE DEPTNO = 20 OR DEPTNO = 30;
```

이 두 쿼리는 동일한 결과를 반환합니다. 문제 없습니다.

---

#### ❌ 문제 상황: NULL이 포함된 경우

```sql
SELECT * FROM EMP WHERE DEPTNO IN (20, 30, NULL); -- ⚠️ 결과 없음

SELECT * FROM EMP WHERE DEPTNO NOT IN (20, 30, NULL); -- ⚠️ 결과 없음
```

위의 쿼리에서 `NULL`이 리스트에 포함되면, **비교 자체가 UNKNOWN**이 되어버려서 결과가 **아예 반환되지 않게 됩니다.** (SQL의 3가지 논리값: TRUE / FALSE / UNKNOWN 때문입니다.)

---

### ✅ `NOT EXISTS`는 어떻게 다를까?

```sql
SELECT * 
FROM EMP E 
WHERE NOT EXISTS (
  SELECT 1 FROM DEPT D 
  WHERE D.DEPTNO = E.DEPTNO AND D.DEPTNO IN (20, 30)
);
```

이 쿼리는 `EMP` 테이블에서 `DEPT` 테이블에 `20`이나 `30` 부서번호가 **존재하지 않는** 사람들을 찾습니다.

**핵심은:** `NOT EXISTS`는 비교 대상에 `NULL`이 있어도 **문제가 없다는 것**입니다.

---

### 💡 결론 요약

- `NOT IN`은 리스트나 서브쿼리 결과에 **NULL이 포함되면 전부 무효**.
- `NOT EXISTS`는 **NULL에 강함**, 조건 만족 여부만 확인하므로 안전함.
- 따라서 실무에서는 **`NOT EXISTS`를 더 자주 사용**하는 것이 좋습니다.
