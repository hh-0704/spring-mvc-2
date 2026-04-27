# 섹션 5. 검증 1 - Validation

> 김영한의 스프링 MVC 2편 - 백엔드 웹 개발 활용 기술  
> [인프런 강의 링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2/dashboard?cid=327260)

---

## 📌 학습 목표

- 클라이언트로부터 넘어오는 데이터를 서버에서 검증하는 이유와 필요성을 이해한다.
- 직접 검증 로직을 작성하는 방식(V1)에서 출발해, Spring이 제공하는 `BindingResult` 기반 검증(V2~)으로 점진적으로 발전하는 흐름을 익힌다.
- 검증 실패 시 입력 폼으로 되돌아가 에러 메시지를 보여주는 UX를 구현한다.

---

## 🧠 핵심 개념 정리

### 검증이 필요한 이유

클라이언트 측 검증(JavaScript)은 조작 가능하므로, **서버 측 검증은 필수**다.  
잘못된 데이터가 DB에 저장되지 않도록 컨트롤러 진입 전 또는 진입 시점에 반드시 검증해야 한다.

### V1 — 직접 검증 로직 작성

`Map<String, String> errors`를 직접 만들어 에러를 수집하고, 모델에 담아 뷰로 전달하는 방식이다.

```java
Map<String, String> errors = new HashMap<>();

if (!StringUtils.hasText(item.getItemName())) {
    errors.put("itemName", "상품 이름은 필수입니다.");
}
if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
    errors.put("price", "가격은 1,000 ~ 1,000,000 까지 허용합니다.");
}
if (item.getQuantity() == null || item.getQuantity() > 9999) {
    errors.put("quantity", "수량은 최대 9,999 까지 허용합니다.");
}

// 복합 룰 검증 (글로벌 오류)
if (item.getPrice() != null && item.getQuantity() != null) {
    int resultPrice = item.getPrice() * item.getQuantity();
    if (resultPrice < 10000) {
        errors.put("globalError", "가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재 값 = " + resultPrice);
    }
}

// 검증 실패 시 폼으로 되돌아감
if (!errors.isEmpty()) {
    model.addAttribute("errors", errors);
    return "validation/v1/addForm";
}
```

**뷰(Thymeleaf)에서 에러 출력:**

```html
<!-- 글로벌 오류 -->
<div th:if="${errors?.containsKey('globalError')}">
    <p th:text="${errors['globalError']}"></p>
</div>

<!-- 필드 오류 -->
<div th:if="${errors?.containsKey('itemName')}">
    <p th:text="${errors['itemName']}"></p>
</div>
```

> `errors?.` → `errors`가 null일 때 예외 발생을 막기 위한 Safe Navigation Operator

### V1 방식의 한계

| 문제 | 설명 |
|---|---|
| 타입 오류 처리 불가 | `int` 필드에 문자를 입력하면 컨트롤러 진입 전에 이미 예외 발생 |
| 입력값 유지 불가 | 타입 바인딩 실패 시 사용자가 입력한 값이 사라짐 |
| 반복 코드 과다 | 검증 로직이 컨트롤러에 섞여 유지보수 어려움 |

→ 이러한 한계를 해결하기 위해 Spring은 `BindingResult`를 제공한다. (V2부터 적용)

### BindingResult (V2 이후 핵심 개념)

`BindingResult`는 Spring이 제공하는 검증 오류 보관 객체로, `@ModelAttribute` 바로 뒤에 선언해야 한다.

```java
@PostMapping("/add")
public String addItem(@ModelAttribute Item item, BindingResult bindingResult, ...) {

    // 필드 오류
    if (!StringUtils.hasText(item.getItemName())) {
        bindingResult.addError(new FieldError("item", "itemName", "상품 이름은 필수입니다."));
    }

    // 글로벌 오류
    bindingResult.addError(new ObjectError("item", "가격 * 수량의 합은 10,000원 이상이어야 합니다."));

    if (bindingResult.hasErrors()) {
        return "validation/v1/addForm";
    }
    ...
}
```

**뷰에서 `#fields`와 `th:errors`로 간편하게 출력:**

```html
<!-- 글로벌 오류 -->
<div th:if="${#fields.hasGlobalErrors()}">
    <p th:each="err : ${#fields.globalErrors()}" th:text="${err}"></p>
</div>

<!-- 필드 오류 -->
<input type="text" th:field="*{itemName}"
       th:errorclass="field-error">
<div class="field-error" th:errors="*{itemName}"></div>
```

---

## 🛠 기술 스택

| 분류 | 기술 |
|---|---|
| Language | Java 25 |
| Framework | Spring Boot 4.0.3 |
| View | Thymeleaf |
| Build | Gradle |
| 기타 | Lombok |
