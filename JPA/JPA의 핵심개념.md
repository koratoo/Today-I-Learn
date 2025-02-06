JPA(Java Persistence API)를 제대로 활용하려면 아래의 핵심 개념들을 이해해야 합니다.

---

## **1. 기본 개념**
- **JPA란 무엇인가?**  
  - Java 객체와 관계형 데이터베이스를 매핑하는 기술
  - SQL 대신 객체 중심으로 데이터베이스를 다룰 수 있도록 도와줌

- **ORM(Object-Relational Mapping)**
  - 객체와 테이블을 매핑하여 자동으로 SQL을 생성하는 방식
  - JPA는 ORM 기술을 제공하는 표준 API

---

## **2. 주요 어노테이션**
- **@Entity**: 클래스를 JPA 엔티티로 선언  
- **@Table**: 테이블명 매핑 (기본적으로 클래스명과 동일)  
- **@Id**: 기본 키 설정  
- **@GeneratedValue**: 기본 키 자동 생성 전략 설정  
- **@Column**: 컬럼 정보 설정 (nullable, length 등)  
- **@Transient**: 매핑하지 않을 필드 지정  
- **@Enumerated**: Enum 타입 매핑  

---

## **3. 연관관계 매핑**
### **(1) 기본 연관관계**
- **@OneToOne**: 1대1 관계 (예: 회원 - 주소)  
- **@OneToMany**: 1대N 관계 (예: 회원 - 주문)  
- **@ManyToOne**: N대1 관계 (예: 주문 - 회원)  
- **@ManyToMany**: N대N 관계 (보통 `@JoinTable`로 중간 테이블을 생성해야 함)

### **(2) 연관관계 매핑의 핵심 개념**
- **단방향 vs. 양방향 관계**  
  - `@ManyToOne` 단방향은 단순하지만, `@OneToMany(mappedBy)`를 추가하면 양방향 매핑 가능  
- **연관관계의 주인(Owner)**  
  - FK가 있는 쪽(`@ManyToOne`)이 주인, 반대쪽(`@OneToMany`)에서는 `mappedBy` 사용  
- **지연 로딩(LAZY)과 즉시 로딩(EAGER)**  
  - `fetch = FetchType.LAZY` (권장, 필요한 순간에 데이터 로드)  
  - `fetch = FetchType.EAGER` (즉시 로딩, 성능 문제 발생 가능)  

---

## **4. JPQL (Java Persistence Query Language)**
- **객체를 대상으로 하는 SQL 문법**  
  - `SELECT m FROM Member m WHERE m.name = '반달'`
- **JPQL과 Native Query 차이점**  
  - JPQL: 객체 중심의 쿼리, 데이터베이스 독립적  
  - Native Query: 특정 DB에 종속된 SQL 사용 가능 (`@Query(nativeQuery = true)`)  

---

## **5. 영속성 컨텍스트 (Persistence Context)**
- **1차 캐시**: 같은 트랜잭션 내에서 같은 엔티티를 조회하면 DB에 쿼리를 날리지 않고 캐시 사용  
- **트랜잭션을 종료하면 flush 후 반영됨**  
- **쓰기 지연 (Write-behind)**: `persist()` 호출 시 바로 DB에 저장되지 않고, 트랜잭션이 끝날 때 한꺼번에 저장  
- **변경 감지 (Dirty Checking)**: `flush()` 시점에 엔티티 변경을 감지하여 자동 업데이트  

---

## **6. 트랜잭션과 동작 방식**
- **영속성 컨텍스트는 트랜잭션 단위로 관리됨**  
- **Spring과 함께 사용할 때 @Transactional 필수**  
  ```java
  @Transactional
  public void saveMember(Member member) {
      entityManager.persist(member);
  }
  ```
- **flush()와 commit()의 차이**  
  - `flush()`: 변경 내용을 DB에 반영하지만, 트랜잭션은 유지  
  - `commit()`: 트랜잭션을 종료하며 flush() 호출  

---

## **7. 성능 최적화**
- **N+1 문제**
  - 연관된 엔티티를 가져올 때 추가적인 쿼리가 너무 많이 발생하는 문제
  - 해결 방법:
    - `fetch join`: `SELECT m FROM Member m JOIN FETCH m.orders`
    - `EntityGraph`: `@EntityGraph(attributePaths = {"orders"})`
  
- **Batch Size 조정**
  - `@BatchSize(size = 100)` 또는 `hibernate.default_batch_fetch_size` 설정  

---

## **8. 스프링 데이터 JPA 활용**
- **기본적인 Repository 인터페이스 제공**
  ```java
  public interface MemberRepository extends JpaRepository<Member, Long> {
      List<Member> findByName(String name);
  }
  ```
- **Query Methods**: `findBy`, `findFirstBy`, `findTop3By`  
- **@Query로 복잡한 JPQL 작성 가능**  
  ```java
  @Query("SELECT m FROM Member m WHERE m.name = :name")
  List<Member> findByNameCustom(@Param("name") String name);
  ```
- **Specification 사용 가능 (동적 쿼리)**  
- **QueryDSL 연동 가능**  

---

## **9. JPA vs. MyBatis**
| 특징 | JPA | MyBatis |
|------|----|------|
| SQL 작성 | 자동 (JPQL) | 직접 SQL 작성 |
| 성능 최적화 | fetch join, Lazy Loading 필요 | SQL 최적화 가능 |
| 유지보수성 | 객체 중심이라 유리 | SQL 변경 시 유지보수 비용 증가 |
| 복잡한 쿼리 | 다소 어려움 | SQL 직접 작성 가능 |

**✅ JPA는 기본적인 CRUD와 연관관계 처리가 편리하지만, 복잡한 쿼리는 MyBatis가 유리**  

---

## **10. 실무에서 JPA를 적용할 때 주의할 점**
1. **즉시 로딩(EAGER) 사용 지양**
   - `LAZY`를 기본값으로 설정하여 불필요한 쿼리 발생 방지
2. **연관관계 매핑 시 양방향 관계 최소화**
   - 단방향 매핑을 기본으로 하고, 필요한 경우에만 양방향 사용
3. **N+1 문제 확인 및 최적화**
   - `JOIN FETCH`, `EntityGraph`, `BatchSize` 설정 활용
4. **트랜잭션 관리**
   - `@Transactional`을 정확한 범위에서 사용하여 데이터 일관성 유지
5. **JPA와 MyBatis를 혼합 사용 고려**
   - 기본 CRUD는 JPA, 복잡한 조회 쿼리는 MyBatis를 병행할 수도 있음

---

### **🔥 JPA를 잘 활용하려면?**
- 기본적인 개념 (영속성 컨텍스트, 트랜잭션, Fetch 전략) 숙지  
- Spring Data JPA를 적극 활용하여 생산성 높이기  
- 성능 이슈 (N+1 문제, Batch Fetch Size) 대비  
- 필요하면 JPA와 MyBatis를 적절히 조합  
