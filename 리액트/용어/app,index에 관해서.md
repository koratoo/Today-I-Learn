리액트에서 `App.js`(또는 `App.tsx`)와 `index.js`(또는 `index.tsx`)는 애플리케이션의 핵심 구조를 담당하는 파일입니다. 각각의 역할과 특징을 정리해보겠습니다.

---

## **1️⃣ `App.js` (또는 `App.tsx`)**
📌 **앱의 메인 컴포넌트 역할을 하는 파일**

### ✅ 주요 역할
- 리액트 애플리케이션의 **최상위 컴포넌트**
- 여러 개의 컴포넌트를 불러와서 조합하는 역할
- 보통 `<App />` 안에서 페이지의 레이아웃을 설정

### ✅ 예제 코드 (`App.js`)
```jsx
import React from 'react';
import Header from './Header';
import Footer from './Footer';

function App() {
  return (
    <div>
      <Header />
      <h1>리액트 애플리케이션</h1>
      <Footer />
    </div>
  );
}

export default App;
```
📌 `App.js`는 여러 개의 컴포넌트 (`Header`, `Footer`)를 조합하여 하나의 페이지를 구성하는 역할을 합니다.

---

## **2️⃣ `index.js` (또는 `index.tsx`)**
📌 **리액트 애플리케이션을 웹 브라우저에 연결하는 파일**

### ✅ 주요 역할
- `ReactDOM.render()` 또는 `createRoot().render()`를 사용하여 리액트 컴포넌트를 HTML의 `<div id="root">`에 렌더링
- `index.js`가 실행되면서 `App.js`를 최상위 컴포넌트로 설정하고 화면에 표시
- 일반적으로 `index.js`에서 `App.js`를 불러와서 렌더링

### ✅ 예제 코드 (`index.js`)
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

### 📝 `index.js`의 동작 과정
1. `ReactDOM.createRoot(document.getElementById('root'))` – HTML에서 `<div id="root">`를 찾아서 React가 제어하도록 함.
2. `root.render(<App />)` – `App.js` 컴포넌트를 화면에 렌더링함.
3. `<React.StrictMode>` – 개발 환경에서 잠재적인 문제를 감지하는 도구(선택 사항).

---

## **📌 `index.js`와 `App.js`의 관계**
1. **`index.js`** → React 앱을 웹 페이지에 연결하는 역할 (`root`에 렌더링)
2. **`App.js`** → 실제 애플리케이션의 메인 컴포넌트로, 여러 컴포넌트를 조합하여 UI를 구성

📌 **HTML과의 연결 구조**
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <title>React App</title>
</head>
<body>
    <div id="root"></div>  <!-- React 앱이 여기에 렌더링됨 -->
</body>
</html>
```
🔹 `index.js`는 이 `id="root"`에 `App.js`를 렌더링하여 웹페이지에 React를 표시합니다.

---

## **💡 결론**
| 파일명 | 역할 |
|--------|------|
| `App.js` | 최상위 컴포넌트 (메인 UI를 구성) |
| `index.js` | 리액트 애플리케이션을 웹 브라우저에 연결 |

**👉 `index.js`가 `App.js`를 감싸서 웹 브라우저에서 화면을 출력하는 역할을 한다!** 🚀
