`CASE WHEN A`와 `CASE A WHEN` 둘 다 사용 가능하지만, **용도가 다릅니다.** 

### 1. `CASE WHEN A` (조건식 CASE)
- 여러 개의 조건을 평가할 때 사용
- `WHEN` 뒤에 **논리 조건**이 들어감
- `ELSE`를 통해 기본값 설정 가능

```sql
SELECT 
    CASE 
        WHEN age >= 18 THEN 'Adult'
        WHEN age >= 13 THEN 'Teenager'
        ELSE 'Child'
    END AS age_group
FROM users;
```
👉 **논리적 조건을 기반으로 값을 반환**할 때 사용

---

### 2. `CASE A WHEN` (값 기반 CASE)
- 특정 컬럼이나 표현식의 값에 따라 분기
- `CASE` 뒤에 **컬럼명이나 값**이 오고, `WHEN` 뒤에는 **매칭할 값**이 들어감

```sql
SELECT 
    CASE age 
        WHEN 18 THEN 'Adult'
        WHEN 13 THEN 'Teenager'
        ELSE 'Child'
    END AS age_group
FROM users;
```
👉 **값을 직접 비교할 때 사용** (즉, `age = 18` 같은 형태)

---

### 차이점 정리

| 유형 | `CASE WHEN A` | `CASE A WHEN` |
|------|--------------|--------------|
| 사용 목적 | 논리 조건 평가 | 값 비교 |
| `WHEN` 뒤 | 논리 조건 (`age >= 18`) | 값 (`18`, `13`) |
| `CASE` 뒤 | 비워둠 | 비교할 대상 지정 |

### 언제 어떤 걸 써야 할까?
✅ **조건이 여러 개이고 복잡한 논리라면** → `CASE WHEN A`  
✅ **단순 값 비교라면** → `CASE A WHEN`

**예제 비교**
```sql
-- 조건식 CASE
SELECT CASE WHEN score >= 90 THEN 'A' WHEN score >= 80 THEN 'B' ELSE 'C' END FROM students;

-- 값 비교 CASE
SELECT CASE score WHEN 90 THEN 'A' WHEN 80 THEN 'B' ELSE 'C' END FROM students;
```

🚀 **결론**:  
둘 다 사용 가능하지만, **조건 평가**할 때는 `CASE WHEN A`  
값을 **직접 비교**할 때는 `CASE A WHEN`이 적절합니다!
