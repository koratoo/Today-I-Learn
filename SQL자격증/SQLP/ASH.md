### Oracle Database 기준으로 설명할게.
**ASH (Active Session History)**는  
오라클 데이터베이스에서 **활성 세션**에 대한 정보를 주기적으로 샘플링해서 메모리에 저장하는 기능이야.

- "활성 세션"이란?  
  SQL을 실행 중이거나 리소스(CPU, 디스크, 락 등)를 기다리고 있는 세션을 말해.
  
- ASH는 어떤 데이터를 수집하냐면
  - 세션 ID
  - 어떤 SQL을 실행 중인지 (SQL_ID)
  - 어떤 오브젝트(테이블, 인덱스 등)를 액세스 중인지
  - 어떤 이벤트(락 대기, IO 대기 등)로 대기 중인지
  - 대기 시간이나 상태 (RUNNING, WAITING)
  - 현재 머물러 있는 코드 위치 (P1, P2, P3 파라미터)
  등을 주기적으로 메모리에 저장해.

- 저장된 데이터는 `v$active_session_history` 뷰로 볼 수 있어.
  그리고 AWR (Automatic Workload Repository) 스냅샷을 통해 디스크에 저장되기도 해.
  
---

### 왜 필요할까?
- 시스템이 느려졌을 때, **"지금 이 순간"** 무슨 일이 벌어지는지 분석할 수 있어.
- 어떤 SQL이 병목을 일으키는지, 어떤 자원 때문에 대기하고 있는지를 실시간처럼 파악할 수 있어.
- 과거 문제도, AWR에 남은 데이터를 기반으로 어느 정도 추적이 가능해.

---

### 간단한 예시
```sql
SELECT
  sample_time,
  session_id,
  session_state,
  event,
  sql_id
FROM
  v$active_session_history
WHERE
  session_state = 'WAITING'
ORDER BY
  sample_time DESC;
```
이런 식으로 조회하면, "어떤 세션이 어떤 이벤트로 대기하고 있었는지"를 최근 순으로 볼 수 있어.

---

### 특징 요약
| 항목 | 설명 |
|:-----|:-----|
| 주기 | 보통 1초마다 샘플링 |
| 위치 | 메모리(메모리 부족하면 오래된 데이터 삭제) |
| 저장 | AWR 스냅샷 시 디스크에도 저장 가능 |
| 용도 | 시스템 튜닝, 성능 분석, 병목 파악 |
