AWR은 **Automatic Workload Repository**의 약자로, 오라클(Oracle) 데이터베이스에서 성능 데이터를 자동으로 수집하고 저장하는 기능이야.

### 쉽게 말하면?
AWR은 **오라클이 자체적으로 주기적으로 성능 데이터를 모아서 저장하는 보고서**라고 보면 돼. 이걸 보면 언제 어떤 SQL이 느렸고, 어떤 리소스가 많이 사용됐는지 등을 분석할 수 있어서 **성능 튜닝에 아주 유용해**.

---

### AWR의 주요 특징
- 기본적으로 **1시간마다 스냅샷(snapshot)** 을 찍어서 DB 상태를 저장해.
- **최대 8일치(기본 설정 기준)** 데이터를 저장함.
- CPU 사용량, 메모리, 디스크 I/O, Wait Event, 실행된 SQL 등 다양한 정보를 포함함.
- `DBA_HIST_`로 시작하는 뷰들에 저장됨.
- **Enterprise Edition + Diagnostics Pack** 옵션에서만 사용 가능.

---

### AWR 보고서 보는 방법
1. SQL*Plus나 SQL Developer에서 `awrrpt.sql` 스크립트 실행  
   ```sql
   SQL> @$ORACLE_HOME/rdbms/admin/awrrpt.sql
   ```
2. HTML 또는 TEXT 형식의 리포트를 생성할 수 있어.

---

### 비슷한 개념들
| 이름 | 설명 |
|------|------|
| **Statspack** | AWR의 전신. 무료 버전에서 사용 가능. |
| **ASH (Active Session History)** | 세션 단위의 실시간 활동 기록. AWR보다 더 세밀함. |
| **OEM (Oracle Enterprise Manager)** | AWR 보고서를 시각화해서 보여주는 툴. |
