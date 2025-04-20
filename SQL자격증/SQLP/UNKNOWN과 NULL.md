오라클 SQL에서 나오는 **`UNKNOWN`과 `NULL`은 비슷해 보이지만, 엄연히 다릅니다.**


## ✅ 결론 먼저!

> **`UNKNOWN`은 `NULL` 그 자체가 아니라, `NULL`이 관여한 비교 연산 결과입니다.**

---

## 🔍 SQL의 3값 논리 (Three-Valued Logic)

| 연산 결과 | 의미                    |
|-----------|-------------------------|
| `TRUE`    | 참                      |
| `FALSE`   | 거짓                    |
| `UNKNOWN` | 알 수 없음 (모호함)     |

`UNKNOWN`은 **`NULL`이 비교에 사용될 때 나오는 결과값**이에요.

---

## 📌 예제로 이해하기

```sql
SELECT * FROM EMP WHERE SAL = NULL;  -- ❌
```

- `SAL`이 `NULL`인지 비교하고 싶지만, `= NULL` 비교는 **항상 `UNKNOWN`**
- 그래서 결과는 **반환되지 않음**

```sql
SELECT * FROM EMP WHERE SAL IS NULL;  -- ✅
```

- `IS NULL`은 `NULL` 비교 전용 문법 → **정상 작동**

---

## 🎯 실무 핵심 포인트

| 상황 | 설명 |
|------|------|
| `NULL = NULL` | `UNKNOWN` → 결과 없음 |
| `NULL <> 1000` | `UNKNOWN` → 결과 없음 |
| `WHERE` 조건에 `UNKNOWN` 결과가 나오면? | **필터링되지 않음** (마치 `FALSE`처럼 무시됨) |

---

## 🔄 정리

- `NULL`은 "값이 없다"라는 **데이터의 상태**
- `UNKNOWN`은 "논리 연산 결과"로서, `NULL`이 들어간 **비교 연산의 결과**
- 둘은 관련은 있지만 **같은 것은 아님**

---

### 💡 마무리 한 줄 요약

> `NULL`은 "모름", `UNKNOWN`은 "모르기 때문에 비교 결과도 모름"!
또한 다양한 오라클 팁은 [GPT Online](https://gptonline.ai/ko/)에서도 확인 가능합니다!
