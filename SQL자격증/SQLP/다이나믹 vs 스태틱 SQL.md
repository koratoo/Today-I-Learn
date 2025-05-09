좋은 질문이야. 간단하게 정리하면:

| 구분 | Static SQL (정적 SQL) | Dynamic SQL (동적 SQL) |
|:----|:--------------------|:---------------------|
| 정의 | 컴파일 시 SQL 문장이 확정되는 것 | 실행 시점에 SQL 문장이 만들어지는 것 |
| 작성 방법 | 코드에 SQL이 **고정**되어 있음 | 코드에서 SQL을 **조립하거나 생성**함 |
| 장점 | 빠름, 안정적, 최적화 용이 | 유연성 높음, 다양한 조건 처리 가능 |
| 단점 | 조건이 다양할 때 불편 | 성능 튜닝 어려움, SQL Injection 위험성 |
| 예시 | `SELECT * FROM EMP WHERE EMPNO = 1234;` | `SELECT * FROM EMP WHERE EMPNO = :empNo;`  (empNo는 실행 시 결정) |

---

**조금 더 부연하면**,  
- **Static SQL**은 애초에 쿼리가 정해져 있으니까, 데이터베이스가 미리 실행 계획(Execution Plan)을 최적화해서 저장할 수 있어.
- **Dynamic SQL**은 실행할 때마다 조건에 따라 쿼리 자체가 달라질 수 있어서, 매번 실행 계획을 새로 짜거나 캐시를 잘 활용해야 해.

예를 들어  
"조건이 3개일 때는 이 쿼리, 조건이 5개일 때는 저 쿼리"처럼  
**조건이 유동적**이라면 **Dynamic SQL**을 써야 하고,  
항상 **정해진 로직**이면 **Static SQL**이 더 좋아.

---
