
### ✅ 왜 ResourceBundle을 사용할까?

1. **다국어 지원 (Internationalization)**
   - 같은 프로그램을 영어, 한국어, 일본어 등 여러 언어로 쉽게 번역해서 제공할 수 있게 해줍니다.
   - 예: 버튼에 “확인”이라고 보이게 하거나 “OK”로 보이게 하거나, 사용자의 언어 설정에 따라 자동 변경 가능.

2. **유지보수 용이**
   - 코드와 문자열 리소스를 분리해놓기 때문에, 문자열을 바꾸고 싶을 때 코드를 수정할 필요가 없습니다.
   - 예: `messages.properties` 파일만 수정하면 됨.

3. **Locale에 따라 자동 선택**
   - 사용자의 지역 설정에 맞춰 알맞은 메시지 파일을 자동으로 불러옵니다.
   - 예: `messages_ko.properties`, `messages_en.properties`

---

### ✅ 사용 예시

```java
// 기본 ResourceBundle 사용 예시
ResourceBundle bundle = ResourceBundle.getBundle("messages", Locale.KOREAN);
String greeting = bundle.getString("greeting");
System.out.println(greeting);  // 예: "안녕하세요"
```

`messages_ko.properties` 파일 내용:
```
greeting=안녕하세요
```

`messages_en.properties` 파일 내용:
```
greeting=Hello
```

---

### 📌 정리
- ResourceBundle은 다국어 메시지 처리를 도와주는 유용한 도구입니다.
- 유지보수를 편하게 하고, 국제화된 서비스를 구현할 때 필수입니다.
