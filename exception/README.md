# 예외 처리와 오류 페이지 / API 예외 처리

> 스프링 MVC 2편 - Section 9, 10 실습 프로젝트

## 기술 스택

![Java](https://img.shields.io/badge/Java_25-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_4.0-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-005F0F?style=flat-square&logo=thymeleaf&logoColor=white)
![Gradle](https://img.shields.io/badge/Gradle-02303A?style=flat-square&logo=gradle&logoColor=white)
![Lombok](https://img.shields.io/badge/Lombok-CA0124?style=flat-square)

## 학습 목차

### Section 9 - 예외 처리와 오류 페이지

| 주제 | 핵심 내용 |
|---|---|
| 서블릿 예외 처리 기본 | `Exception` 전파, `response.sendError()` |
| 오류 화면 제공 | `WebServerFactoryCustomizer`, 오류 페이지 등록 |
| 오류 페이지 작동 원리 | WAS의 오류 페이지 재요청 흐름, `ERROR_EXCEPTION` 등 request attribute |
| DispatcherType과 인터셉터 | `DispatcherType.ERROR`, 필터/인터셉터 오류 요청 제외 처리 |
| 스프링 부트 오류 페이지 | `BasicErrorController`, `/error` 경로, `ErrorPage` 자동 등록 |

### Section 10 - API 예외 처리

| 주제 | 핵심 내용 |
|---|---|
| API 예외 처리 기본 | `Accept: application/json` 에 따른 오류 응답 분기 |
| 스프링 부트 기본 오류 처리 | `BasicErrorController`의 HTML/JSON 응답 분기 |
| HandlerExceptionResolver | `HandlerExceptionResolver` 인터페이스 직접 구현 |
| ExceptionResolver 활용 | 예외를 정상 응답으로 변환, HTTP 상태코드 직접 지정 |
| 스프링 제공 ExceptionResolver | `ResponseStatusExceptionResolver`, `DefaultHandlerExceptionResolver` |
| `@ExceptionHandler` | 컨트롤러 단위 예외 처리, `ErrorResult` DTO 반환 |
| `@ControllerAdvice` | 전역 예외 처리 분리, `@RestControllerAdvice` 활용 |

## 핵심 학습 내용

### 서블릿 오류 처리 흐름

```
Controller 예외 발생
  → WAS까지 예외 전파
    → WAS에서 오류 페이지 경로 확인
      → 오류 페이지 경로로 재요청 (내부 요청)
        → Controller → View 렌더링
```

- WAS는 오류 페이지 재요청 시 `DispatcherType.ERROR`로 요청을 전달
- 필터: `setDispatcherTypes(DispatcherType.REQUEST)`로 오류 요청 제외 가능
- 인터셉터: `excludePathPatterns("/error-page/**")`로 오류 경로 제외

### 스프링 부트 오류 페이지 자동화

- `ErrorMvcAutoConfiguration`이 `BasicErrorController` 자동 등록
- `/error` 경로로 오류 요청을 자동 처리
- `resources/templates/error/` 하위에 `404.html`, `5xx.html` 등 파일 배치만으로 오류 페이지 적용

### API 예외 처리 흐름

```
@ExceptionHandler + @ControllerAdvice 조합 (권장)
  → 예외 발생 시 ExceptionHandler 메서드 호출
    → ErrorResult DTO로 JSON 응답 반환
      → HTTP 상태코드 지정 (@ResponseStatus 또는 ResponseEntity)
```

### ExceptionResolver 우선순위

```
1. ExceptionHandlerExceptionResolver   → @ExceptionHandler 처리
2. ResponseStatusExceptionResolver     → @ResponseStatus, ResponseStatusException 처리
3. DefaultHandlerExceptionResolver     → 스프링 MVC 내부 예외 처리 (TypeMismatch 등)
```

### @ControllerAdvice 적용 범위 지정

```java
// 특정 애노테이션이 붙은 컨트롤러만 적용
@ControllerAdvice(annotations = RestController.class)

// 특정 패키지만 적용
@ControllerAdvice("hello.exception.api")

// 특정 클래스만 적용
@ControllerAdvice(assignableTypes = {ApiExceptionController.class})
```

## 참고

- 강의: [스프링 MVC 2편 - 백엔드 웹 개발 활용 기술 (인프런)](https://www.inflearn.com/course/스프링-mvc-2)
- 강사: 김영한
