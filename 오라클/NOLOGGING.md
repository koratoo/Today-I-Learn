`nologging`은 함수(function)가 아니라, **Oracle 데이터베이스의 테이블이나 인덱스에 설정할 수 있는 저장 옵션**이에요.

### `NOLOGGING`이란?
Oracle에서는 데이터 조작 시 기본적으로 **Redo 로그**를 남기는데, `NOLOGGING` 옵션을 사용하면 **Redo 로그 생성을 최소화**해서 **성능을 향상**시킬 수 있어요. 다만, 장애 발생 시 **복구 불가능한 데이터 손실**이 생길 수 있기 때문에 신중하게 써야 해요.

### 사용 예시
```sql
CREATE TABLE my_table (
  id NUMBER,
  name VARCHAR2(100)
) NOLOGGING;
```

또는 데이터 삽입 시에도:
```sql
INSERT /*+ APPEND */ INTO my_table
SELECT * FROM another_table;
-- 위 작업 전에 my_table이 NOLOGGING 설정되어 있으면 로그 최소화
```

### 주의할 점
- `NOLOGGING`은 **DDL 작업이나 Direct-path Insert**에만 적용돼요.
- `ARCHIVELOG` 모드에서는 복구 문제가 생길 수 있어 백업 전략을 잘 짜야 해요.
- 데이터베이스 장애 이후 **복구가 필요한 경우 문제가 될 수 있어** 주의가 필요해요.
