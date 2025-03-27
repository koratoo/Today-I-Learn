`TRIM()`을 사용했을 때 공백이 제거된 결과가 `NULL`이 되는 경우는, 원래 값이 **공백만 존재하거나 NULL**인 경우입니다. 이걸 해결하려면 `TRIM()` 이후에도 `NULL`이 아닌 기본값으로 대체하거나, 그대로 `NULL` 여부를 체크해서 적절히 처리하는 방식이 있습니다.

### 1. `NVL`, `COALESCE`로 기본값 처리 (Oracle 기준)
```sql
NVL(TRIM(column_name), '기본값')
-- 또는
COALESCE(TRIM(column_name), '기본값')
```

### 2. 공백만 있을 경우를 `NULL`과 동일하게 처리하고 싶지 않을 때
```sql
CASE 
    WHEN TRIM(column_name) IS NULL THEN '기본값'
    ELSE TRIM(column_name)
END
```

### 3. 아예 공백 제거 후 길이로 판단
```sql
CASE 
    WHEN LENGTH(TRIM(column_name)) = 0 THEN '기본값'
    ELSE TRIM(column_name)
END
```

---

예를 들어, `'   '` 같은 문자열이 들어왔을 때 `TRIM()` 결과가 `''` (빈 문자열)이 되고, 이걸 `NULL`로 간주하고 싶지 않다면 위처럼 길이 체크로 처리하는 게 좋아요.
