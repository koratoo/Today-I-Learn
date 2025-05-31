MVC 패턴(Model-View-Controller)은 소프트웨어 디자인 패턴 중 하나로, **애플리케이션을 세 가지 역할로 나누어 구조화**하여 개발 효율성과 유지보수성을 높이는 방식입니다. 주로 웹 개발이나 GUI 프로그램에서 사용됩니다.

---

## 🔷 MVC 패턴이란?

MVC는 다음의 세 가지 구성요소로 나뉩니다:

| 구성요소                  | 역할 설명                                                     |
| --------------------- | --------------------------------------------------------- |
| **Model (모델)**        | 데이터를 관리하고, 비즈니스 로직을 처리함. <br>예: DB와의 연결, 데이터 저장/조회        |
| **View (뷰)**          | 사용자에게 보여지는 UI를 담당함. <br>예: HTML, JSP, 화면 출력               |
| **Controller (컨트롤러)** | 사용자의 입력을 받아서 모델과 뷰를 연결해주는 역할. <br>예: URL 매핑, 요청 처리, 결과 전달 |

---

## 🔄 흐름 예시 (웹 애플리케이션 기준)

1. 사용자가 웹페이지에서 버튼 클릭 → **요청 (Request) 발생**
2. **Controller**가 요청을 받아 어떤 처리를 할지 판단
3. **Model**에게 데이터 요청 또는 저장 지시
4. **Model**이 DB 등에서 필요한 데이터를 조회하거나 처리
5. Controller가 결과를 받아 **View**에 전달
6. **View**는 사용자에게 결과를 보여줌

---

## 🧠 예제 (Spring MVC 기준)

```java
// Controller
@GetMapping("/user")
public String getUser(Model model) {
    User user = userService.getUserInfo();  // Model
    model.addAttribute("user", user);
    return "userView";  // View 이름 (예: userView.jsp)
}
```

---

## ✅ MVC 패턴의 장점

* **관심사 분리(SoC)**: 역할이 명확해서 유지보수와 협업이 쉬움
* **유연성**: 화면(View)을 바꿔도 비즈니스 로직(Model)은 그대로 사용 가능
* **테스트 용이**: 각각을 독립적으로 테스트할 수 있음

---

## ⚠️ 주의할 점

* **Controller에 로직이 몰리면** 'Fat Controller'가 되므로, 모델로 비즈니스 로직을 옮기는 것이 좋음
* 프로젝트가 커지면 MVC만으로는 한계가 있어 추가 아키텍처(ex. MVVM, MVP 등)를 고려하기도 함
