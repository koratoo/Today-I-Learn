`v$` 뷰는 Oracle에서 동적 성능 뷰(Dynamic Performance View)를 의미하며, 시스템 상태, 세션 정보, 메모리 사용 현황 등을 조회할 때 사용됩니다.  
여기서는 오라클 기본 테이블인 `EMP`, `DEPT`와 함께 사용할 수 있는 대표적인 `v$` 뷰 몇 가지와 예제를 소개할게요.

---

### ✅ 1. `v$session` : 현재 접속 중인 세션 정보

```sql
SELECT s.sid, s.serial#, s.username, s.status, e.ename, d.dname
FROM v$session s
JOIN emp e ON s.username = e.ename
JOIN dept d ON e.deptno = d.deptno
WHERE s.username IS NOT NULL;
```

- `s.username`은 오라클 유저 계정 이름 (예: SCOTT)
- `emp.ename`이 실제 유저명과 매칭된다는 가정 하에 작성된 예제입니다 (실제 유저명이 `ename`과 일치한다는 보장은 없음)

---

### ✅ 2. `v$sql` : 공유 SQL 영역에서 캐싱된 SQL 문장 정보

```sql
SELECT sql_text, executions, parsing_schema_name
FROM v$sql
WHERE sql_text LIKE '%EMP%';
```

- EMP 테이블을 사용한 SQL 문장을 조회
- 얼마나 자주 실행되었는지 (`executions`)도 함께 확인 가능

---

### ✅ 3. `v$locked_object` + `dba_objects` : 잠금 발생 정보

```sql
SELECT lo.session_id, o.object_name, o.object_type
FROM v$locked_object lo
JOIN dba_objects o ON lo.object_id = o.object_id
WHERE o.object_name IN ('EMP', 'DEPT');
```

- EMP나 DEPT 테이블에 잠금이 걸려있는지 확인
- `DBA 권한`이 필요할 수 있음

---

### ✅ 4. `v$process` + `v$session` : OS 프로세스와 오라클 세션 매핑

```sql
SELECT s.sid, s.serial#, p.spid, s.username
FROM v$session s
JOIN v$process p ON s.paddr = p.addr
WHERE s.username IS NOT NULL;
```

- 오라클 세션과 OS 상의 프로세스 ID(`spid`) 연결

---

### ✅ 5. `v$open_cursor` : 오픈된 커서 정보

```sql
SELECT sid, user_name, sql_text
FROM v$open_cursor
WHERE sql_text LIKE '%EMP%';
```

- EMP 테이블을 포함한 커서가 열려 있는 세션 확인
