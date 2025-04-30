`display: flex`와 `display: block`은 **HTML 요소가 화면에 어떻게 배치될지 결정하는 CSS 속성 값**입니다. 각각의 특징과 차이를 아래에 정리해드릴게요.

---

### ✅ `display: block`
- 요소가 **한 줄 전체를 차지**합니다.
- 아래 요소는 **자동으로 줄바꿈** 됩니다.
- 예시: `<div>`, `<p>`, `<h1>` 등은 기본적으로 `display: block`.

#### 특징
- 내부 요소가 기본적으로 **위에서 아래로 세로 배치**됩니다.
- `width`, `height`, `margin`, `padding`을 자유롭게 지정할 수 있습니다.

```html
<div style="display: block; background: lightblue;">
  Block 요소
</div>
```

---

### ✅ `display: flex`
- 요소를 **flex 컨테이너**로 만들어 **자식 요소들을 유연하게 배치**할 수 있게 합니다.
- 자식 요소는 **기본적으로 가로 방향(row)** 으로 배치됩니다.

#### 특징
- **가로 또는 세로 방향**으로 자식 정렬 가능 (`flex-direction`)
- 공간 분배 (`justify-content`)나 정렬 (`align-items`)이 쉬움
- 반응형 레이아웃 구성에 탁월

```html
<div style="display: flex; background: lightgray;">
  <div style="margin: 5px;">Flex 1</div>
  <div style="margin: 5px;">Flex 2</div>
</div>
```

---

### 📌 주요 차이점 요약

| 항목            | `block`                   | `flex`                             |
|-----------------|---------------------------|------------------------------------|
| 자식 배치 방향  | 수직(위→아래)              | 기본 수평(왼→오), 변경 가능        |
| 줄바꿈          | 항상 줄바꿈됨              | 자동 줄바꿈 안됨 (`flex-wrap` 필요)|
| 레이아웃 제어   | 제한적                    | 매우 유연, 정렬/공간 분배 가능    |
| 사용 목적       | 기본 블록 레이아웃        | 복잡한 정렬, 반응형 구조          |
