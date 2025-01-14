`Optional`은 Java 8에서 추가된 기능으로, 값이 `null`일 가능성을 명시적으로 처리할 수 있도록 돕는 도구입니다. 하지만 모든 상황에서 무조건 사용하는 것이 좋은 것은 아닙니다. 아래는 `Optional`을 사용할 때와 사용하지 말아야 할 경우에 대한 가이드를 정리한 내용입니다.

---

### **`Optional`을 사용하기 적합한 경우**
1. **메서드 반환값으로 사용**  
   - 특정 메서드가 값이 없을 수도 있다는 것을 명시적으로 표현하고 싶을 때.  
     ```java
     public Optional<User> findUserById(String id) {
         // 데이터가 없을 가능성을 Optional로 표현
         return Optional.ofNullable(userRepository.findById(id));
     }
     ```
   - 클라이언트 코드에서 `isPresent()`, `orElse()`, `ifPresent()` 등을 사용해 안전하게 처리할 수 있음.
   
2. **NullPointerException 방지**  
   - 값을 사용할 때 항상 null 체크가 필요하거나, 이를 깜빡하는 실수를 방지하고자 할 때.  
     ```java
     Optional<String> name = Optional.ofNullable(getUserName());
     name.ifPresent(System.out::println); // 안전하게 처리
     ```

3. **함수형 프로그래밍 스타일 활용**  
   - `map`, `filter`, `flatMap` 등 함수형 메서드와 조합하여 코드 가독성을 높이고 싶을 때.  
     ```java
     Optional<User> user = userService.findUserById(id);
     String email = user.map(User::getEmail).orElse("default@example.com");
     ```

---

### **`Optional` 사용을 피해야 하는 경우**
1. **엔티티 클래스의 필드**  
   - `Optional`은 메모리 및 성능에 오버헤드를 추가할 수 있기 때문에, 엔티티 필드에 사용하는 것은 지양해야 함.
     ```java
     // Bad Practice
     public class User {
         private Optional<String> name; // 필드에 Optional 사용 X
     }
     ```
   - 대신 기본 자료형 또는 `@NotNull`과 같은 제약 조건을 사용.

2. **메서드 매개변수로 사용**  
   - `Optional`은 매개변수로 사용하도록 설계되지 않았음. 대신 오버로드된 메서드를 사용하거나, 명시적으로 `null`을 처리해야 함.
     ```java
     // Avoid this
     public void process(Optional<String> value) { ... }
     
     // Prefer this
     public void process(String value) {
         if (value == null) {
             // null 처리
         }
     }
     ```

3. **성능이 중요한 경우**  
   - `Optional`은 박싱, 언박싱, 래핑 등으로 인해 불필요한 오버헤드가 발생할 수 있음. 성능이 중요한 코드에서는 대신 직접 null 체크를 수행하는 것이 나을 수 있음.

---

### **결론**
- `Optional`은 값이 없을 수 있음을 명시적으로 표현하고, 안전하게 코드를 작성할 수 있도록 돕는 도구입니다.
- 하지만 지나치게 사용하면 코드가 복잡해지고 성능 문제가 발생할 수 있으므로, 반환값에서 주로 사용하며, 필드와 매개변수에서는 적절히 대체 방식을 활용해야 합니다.

