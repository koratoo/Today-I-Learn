`compareTo()`는 `BigDecimal`을 **정확하게 비교**하기 위해 사용하는 메서드야. `==`이나 `.equals()`를 쓰면 **의도한 비교가 안 될 수 있어서**, 수치 비교에는 `compareTo()`를 쓰는 게 정석이야.

---

### ✅ `compareTo()`의 동작 원리

```java
int result = a.compareTo(b);
```

* `result < 0` → `a < b`
* `result == 0` → `a == b`
* `result > 0` → `a > b`

---

### ❗ 왜 `equals()` 대신 써야 할까?

`BigDecimal`은 **값뿐 아니라 소수점 자리수까지** 비교해.

```java
BigDecimal a = new BigDecimal("1.0");
BigDecimal b = new BigDecimal("1.00");

System.out.println(a.equals(b));        // false
System.out.println(a.compareTo(b));     // 0
```

* `equals()`는 **scale**(자릿수)까지 비교하므로 `false`
* `compareTo()`는 **숫자 크기**만 비교하므로 `0` (같다고 판단)

---

### 💡 활용 예시

```java
BigDecimal price = new BigDecimal("100.00");

if (price.compareTo(BigDecimal.ZERO) > 0) {
    System.out.println("금액이 0보다 큽니다.");
}
```

---

### 요약

| 메서드            | 비교 방식      | 언제 사용?                        |
| -------------- | ---------- | ----------------------------- |
| `==`           | 참조 비교      | 절대 사용 ❌                       |
| `.equals()`    | 값 + 자릿수 비교 | 자릿수까지 동일해야 할 때                |
| `.compareTo()` | 값(크기) 비교   | **수치적으로 같거나 크고 작음을 비교할 때 ✔️** |

