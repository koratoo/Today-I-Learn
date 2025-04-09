좋아요! Java에서 `ThreadLocal`은 멀티스레드 환경에서 **각 스레드마다 고유한 데이터를 저장**할 수 있게 해주는 유틸리티 클래스입니다. 즉, **스레드 간에 데이터를 공유하지 않고 각각 독립적으로 관리할 수 있게** 해줘요.

---

## ✅ Java에서 `ThreadLocal` 예제

```java
public class ThreadLocalExample {
    // ThreadLocal 변수 선언
    private static ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 0);

    public static void main(String[] args) {
        Runnable task = () -> {
            // 현재 스레드 이름 출력
            String threadName = Thread.currentThread().getName();
            
            // 현재 스레드의 ThreadLocal 값 설정
            threadLocal.set((int) (Math.random() * 100));
            
            System.out.println(threadName + "의 ThreadLocal 값: " + threadLocal.get());
        };

        // 여러 스레드 생성
        Thread t1 = new Thread(task, "스레드-1");
        Thread t2 = new Thread(task, "스레드-2");
        Thread t3 = new Thread(task, "스레드-3");

        t1.start();
        t2.start();
        t3.start();
    }
}
```

### 🔍 결과 예시 (실행마다 다름)

```
스레드-1의 ThreadLocal 값: 42
스레드-2의 ThreadLocal 값: 75
스레드-3의 ThreadLocal 값: 13
```

---

### 📌 요약

- `ThreadLocal`은 **스레드마다 독립된 값을 저장**하고 가져오도록 도와주는 클래스입니다.
- `get()`과 `set()`으로 값에 접근하며, `withInitial()`을 통해 초기값 설정이 가능합니다.
- DB 커넥션, 로그인 사용자 정보 등 **스레드마다 고유한 정보를 저장할 때 유용**하게 쓰여요.

---
