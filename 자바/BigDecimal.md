`BigDecimal`은 Java에서 **아주 정밀한 소수 계산**을 할 수 있도록 제공되는 클래스야. 일반적인 `float`이나 `double`은 속도는 빠르지만 **오차가 발생할 수 있어서**, 특히 **돈 계산이나 정밀한 수치 계산**이 필요한 경우에는 `BigDecimal`을 사용하는 게 좋아.

---

### 🔍 왜 필요해?

Java의 `float`이나 `double`은 **부동소수점 방식**으로 저장돼서, 예를 들어 이런 오차가 생겨:

```java
System.out.println(0.1 + 0.2);  // 출력: 0.30000000000000004
```

이런 오차는 금융 계산에서는 치명적이야. 그래서 `BigDecimal`을 사용하면 정확하게 계산할 수 있어:

```java
import java.math.BigDecimal;

BigDecimal a = new BigDecimal("0.1");
BigDecimal b = new BigDecimal("0.2");
BigDecimal c = a.add(b);

System.out.println(c);  // 출력: 0.3
```

---

### ✨ 주요 특징

* **정확도 보장**: 무한소수도 정확하게 다룰 수 있어.
* **불변 객체**: 한 번 생성하면 값이 바뀌지 않아.
* **연산 메서드 사용**: `+`, `-`, `*`, `/` 같은 연산자는 못 쓰고 `add`, `subtract`, `multiply`, `divide` 같은 메서드를 써야 해.

---

### 📌 주의점

* `new BigDecimal(0.1)`처럼 **double로 생성하면 오차**가 생기니까 항상 `"0.1"`처럼 **문자열로 생성**해야 해.
* 성능은 `double`보다 느리지만, **정확도가 중요할 때는 필수**야.

