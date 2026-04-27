# 섹션 4. 메시지, 국제화

> 김영한의 스프링 MVC 2편 - 백엔드 웹 개발 활용 기술  
> [인프런 강의 링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2/dashboard?cid=327260)

---

## 📌 학습 목표

- **메시지(Message)** 기능을 통해 화면에 표시되는 문자열을 한 곳에서 관리하는 방법을 이해한다.
- **국제화(Internationalization, i18n)** 를 통해 언어/지역별로 다른 메시지를 제공하는 방법을 학습한다.
- Spring의 `MessageSource`와 Thymeleaf 메시지 표현식(`#{...}`)의 연동 방식을 익힌다.

---

## 🧠 핵심 개념 정리

### 메시지 (Message)

화면에 하드코딩된 문자열(`상품명`, `가격` 등)을 `.properties` 파일로 분리해 관리하는 기능이다.  
파일 한 곳만 수정하면 여러 화면에 적용되어 유지보수성이 크게 향상된다.

```properties
# messages.properties
label.item=상품
label.item.id=상품 ID
label.item.itemName=상품명
label.item.price=가격
label.item.quantity=수량
```

### 국제화 (Internationalization)

메시지 파일을 언어별로 분리하면 국제화가 가능하다.  
HTTP 요청의 `Accept-Language` 헤더 또는 사용자가 선택한 Locale을 기준으로 적절한 파일이 선택된다.

| 파일명 | 적용 대상 |
|---|---|
| `messages.properties` | 기본값 (한국어) |
| `messages_en.properties` | 영어 |

### Spring MessageSource 설정

Spring Boot는 `MessageSource`를 자동으로 등록하며, `application.properties`에서 basename을 지정할 수 있다.

```properties
spring.messages.basename=messages
```

직접 빈을 등록할 경우:

```java
@Bean
public MessageSource messageSource() {
    ResourceBundleMessageSource ms = new ResourceBundleMessageSource();
    ms.setBasenames("messages");
    ms.setDefaultEncoding("utf-8");
    return ms;
}
```

### Thymeleaf 메시지 표현식

Thymeleaf에서는 `#{ }` 표현식으로 메시지를 간편하게 사용할 수 있다.

```html
<!-- 메시지 적용 전 -->
<label>상품명</label>

<!-- 메시지 적용 후 -->
<label th:text="#{label.item.itemName}">상품명</label>
```

파라미터도 지원한다:

```html
<p th:text="#{hello.name(${item.itemName})}"></p>
```

```properties
hello.name=안녕하세요 {0}
```

### LocaleResolver

Spring은 Locale 결정 방식을 `LocaleResolver` 인터페이스로 추상화한다.

| 구현체 | 설명 |
|---|---|
| `AcceptHeaderLocaleResolver` | 기본값. HTTP `Accept-Language` 헤더 사용 |
| `SessionLocaleResolver` | 세션에 Locale 저장 |
| `CookieLocaleResolver` | 쿠키에 Locale 저장 |

`LocaleChangeInterceptor`와 함께 사용하면 쿼리 파라미터(`?lang=en`)로 언어를 전환할 수 있다.

---

## 🛠 기술 스택

| 분류 | 기술 |
|---|---|
| Language | Java 25 |
| Framework | Spring Boot 4.0.3 |
| View | Thymeleaf |
| Build | Gradle |
| 기타 | Lombok |
