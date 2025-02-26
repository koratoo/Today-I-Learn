### 📌 JOIN 시 인덱스 사용 여부가 달라지는 현상 정리  

#### ✅ 문제 상황  
1. `JOIN A`, `JOIN B`, `JOIN C`를 수행할 때 **B가 인덱스를 타지 않음**  
2. 하지만 **`JOIN A`를 주석 처리하면 B가 인덱스를 탐**  

#### 🛠 원인 가능성  

1. **조인 순서(Join Order) 최적화 영향**  
   - PostgreSQL은 **조인 순서를 최적화**하면서 비용이 낮은 실행 계획을 선택합니다.  
   - `JOIN A`가 포함되면 **B의 접근 방식이 변경**될 수 있음.  
   - A가 먼저 처리되면서 `JOIN B` 시 인덱스를 사용할 필요가 없다고 판단할 수 있음.

2. **결과 집합 크기의 변화**  
   - `JOIN A`가 들어가면 결과 집합이 커질 수 있음.  
   - PostgreSQL은 **결과가 클 경우, 전체 스캔(Seq Scan)이 더 효율적**이라고 판단할 수 있음.  
   - 반대로 `JOIN A`가 제거되면 결과 집합이 작아지고, 인덱스 스캔이 유리하다고 판단할 가능성이 있음.

3. **통계 정보(ANALYZE) 및 실행 계획 차이**  
   - PostgreSQL은 테이블의 데이터 분포를 보고 최적화함.  
   - `JOIN A`가 있을 때는 B 테이블을 **다른 방식(예: Hash Join)**으로 접근하는 것이 비용이 낮다고 판단할 수 있음.  
   - `EXPLAIN ANALYZE` 실행 시, A가 포함될 때와 아닐 때의 실행 계획을 비교해보는 것이 필요.

4. **인덱스 선택 문제**  
   - `JOIN A`가 들어가면서 `JOIN B`에 필요한 인덱스를 PostgreSQL이 선택하지 않을 수 있음.  
   - PostgreSQL은 때때로 인덱스를 **사용하지 않는 것이 빠르다고 판단**할 수도 있음.

---

### 🛠 해결 방법  

✔ **1. `EXPLAIN ANALYZE` 확인**  
   - `EXPLAIN (ANALYZE, BUFFERS)`로 A 포함 여부에 따라 실행 계획을 비교  
   ```sql
   EXPLAIN (ANALYZE, BUFFERS)
   SELECT ... FROM B
   JOIN C ON ...
   WHERE ...;
   ```

✔ **2. 조인 순서 강제 (`JOIN` 순서 힌트 없음, but 서브쿼리 활용 가능)**  
   - A를 먼저 처리하는 것이 문제라면 **서브쿼리로 조인 순서 조정 가능**  
   ```sql
   SELECT * 
   FROM (SELECT * FROM B WHERE 조건) B
   JOIN C ON ...
   ```

✔ **3. `ANALYZE`로 통계 정보 갱신**  
   - PostgreSQL이 잘못된 실행 계획을 선택할 가능성이 있으므로 **통계 정보 최신화**  
   ```sql
   ANALYZE B;
   ```

✔ **4. `SET enable_seqscan = OFF` 테스트**  
   - 강제로 인덱스를 사용하게 설정 후 실행 계획 확인  
   ```sql
   SET enable_seqscan = OFF;
   ```

✔ **5. 인덱스 재구성 (`REINDEX`)**  
   - 인덱스가 비효율적으로 동작할 가능성이 있다면 재구성  
   ```sql
   REINDEX INDEX index_name;
   ```

✔ **6. `FORCE INDEX` 대체 방법 (`INDEX ONLY SCAN` 가능 여부 확인)**  
   - PostgreSQL은 `FORCE INDEX` 같은 힌트가 없지만, `INDEX ONLY SCAN`이 가능한지 체크  

---

### 📌 결론  
- `JOIN A`가 포함될 때 B가 인덱스를 타지 않는 이유는 **조인 순서, 실행 계획, 결과 집합 크기 차이, 통계 정보 문제 등**이 원인일 가능성이 큼.  
- `EXPLAIN ANALYZE`로 확인 후 **조인 순서 변경, 통계 갱신, 실행 계획 조정**을 시도하면 해결할 수 있음! 🚀
