### **Early Return (얼리 리턴)**
`Early Return`은 **함수나 메서드에서 불필요한 중첩(Nesting)을 줄이고, 특정 조건을 만족하면 조기에 함수를 종료하는 방식**을 의미합니다.

---

## 1. **Early Return이 필요한 이유**
일반적으로 코드를 작성할 때, 불필요한 **중첩(Nesting)**이 많아지면 **가독성이 떨어지고 유지보수가 어려워집니다.**  
**Early Return을 사용하면** 다음과 같은 장점이 있습니다.

✅ **가독성이 좋아진다** → 코드가 직관적이고 한눈에 이해하기 쉬움  
✅ **불필요한 중첩을 줄인다** → 들여쓰기가 깊어지지 않음  
✅ **유지보수성이 향상된다** → 로직이 간결해짐

---

## 2. **Early Return vs 일반적인 코드**
### **(1) 일반적인 코드 (중첩이 깊음)**
```java
public void process(int number) {
    if (number > 0) {
        if (number % 2 == 0) {
            System.out.println("Even positive number");
        } else {
            System.out.println("Odd positive number");
        }
    } else {
        System.out.println("Number is not positive");
    }
}
```
**⚠ 문제점:**  
- `if-else` 중첩이 많아 **코드 흐름을 한눈에 파악하기 어려움**  
- 들여쓰기가 깊어져서 **유지보수가 어려움**

---

### **(2) Early Return 적용 (중첩 제거)**
```java
public void process(int number) {
    if (number <= 0) {
        System.out.println("Number is not positive");
        return;
    }
    
    if (number % 2 == 0) {
        System.out.println("Even positive number");
    } else {
        System.out.println("Odd positive number");
    }
}
```
**✨ 개선된 점:**  
- `if (number <= 0)`에서 조기에 함수 종료 → **이후 로직은 긍정적인 경우만 처리**  
- 불필요한 중첩이 사라져 **코드가 간결하고 가독성이 좋아짐**

---

## 3. **Early Return 적용 예시**
### **(1) null 체크 (방어적 코드)**
```java
public void printUserName(User user) {
    if (user == null) {
        System.out.println("User is null");
        return;
    }
    
    System.out.println("User name: " + user.getName());
}
```
✅ `user == null`이면 조기에 반환 → 이후 로직에서 `null` 체크 불필요

---

### **(2) 반복문에서 Early Return**
```java
public boolean containsNegative(int[] numbers) {
    for (int num : numbers) {
        if (num < 0) {
            return true; // 음수 발견 시 즉시 종료
        }
    }
    return false; // 모든 요소 검사 후 음수가 없으면 false 반환
}
```
✅ 음수가 발견되면 즉시 `return true` → **불필요한 연산을 줄이고 성능 향상**

---

## 4. **정리**
✔ **불필요한 중첩을 줄이고 코드 가독성을 높이는 패턴**  
✔ 특정 조건을 만족하면 **조기에 `return`**하여 함수 실행을 종료  
✔ `null 체크`, `반복문 최적화`, `예외 처리` 등 다양한 곳에서 활용 가능  
✔ **"긍정적인 흐름"을 유지하며 코드를 작성하는 것이 핵심!** 🚀

\
