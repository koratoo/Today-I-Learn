# JavaScript에서 `$` 기호의 의미

## **`$` 기호란?**
`$` 기호는 JavaScript 코드에서 **DOM 요소를 가리키는 변수 이름**으로 흔히 사용되는 관례(convention)입니다. 
이 기호 자체에는 특별한 의미가 있는 것은 아니지만, 개발자들이 아래와 같은 이유로 사용합니다:

---

## **`$`를 사용하는 이유**

### 1. **DOM 요소 변수임을 명확히 하기 위해**
- 코드에서 변수가 DOM 요소인지 일반 데이터인지 구분하기 쉽게 하기 위한 관례입니다.
- 예를 들어:
  ```javascript
  const $input = document.querySelector("input");
  const username = "반달";
  ```
  - `$input`: DOM에서 가져온 `<input>` 요소.
  - `username`: 일반 데이터.

### 2. **jQuery와의 전통적인 관례**
- 과거 jQuery 라이브러리가 널리 사용되던 시기에 `$`는 jQuery 객체를 가리키는 데 사용되었습니다.
- 이 전통이 이어져 DOM 요소를 다룰 때 `$`를 붙이는 관습이 생겼습니다.

---

## **`$`를 사용하는 예**
```javascript
const $button = document.querySelector("button");
$button.addEventListener("click", () => {
  console.log("Button clicked!");
});
```
- `$button`: 버튼 DOM 요소를 나타냅니다.

## **`$`를 사용하지 않아도 되는 경우**
```javascript
const button = document.querySelector("button");
button.addEventListener("click", () => {
  console.log("Button clicked!");
});
```
- `$` 없이도 동일하게 동작합니다. 이 경우 변수 이름은 개발자의 스타일에 따라 결정됩니다.

---

## **결론**
- **`$`는 단지 코드 가독성을 높이기 위한 관례일 뿐이며, 필수는 아닙니다.**
- DOM 요소를 다룰 때 `$`를 붙이면 협업 시 다른 개발자들에게 "이 변수는 DOM 요소를 다룬다"는 의도를 전달하는 데 유용할 수 있습니다.
