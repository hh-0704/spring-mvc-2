# Spring MVC 2편 - 백엔드 웹 개발 활용 기술

> 김영한의 스프링 MVC 2편 강의 실습 레포지토리입니다.

## 기술 스택

![Java](https://img.shields.io/badge/Java_25-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_4.0-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-005F0F?style=flat-square&logo=thymeleaf&logoColor=white)
![Gradle](https://img.shields.io/badge/Gradle-02303A?style=flat-square&logo=gradle&logoColor=white)
![Lombok](https://img.shields.io/badge/Lombok-CA0124?style=flat-square)

## 학습 목차

| 디렉토리 | 주제 | 핵심 내용 |
|---|---|---|
| [`thymeleaf-basic`](./thymeleaf-basic) | 타임리프 기본 | 표현식, 반복, 조건, 템플릿 조각, 레이아웃 |
| [`form`](./form) | 타임리프 폼 | 체크박스, 라디오버튼, 셀렉트박스, th:object |
| [`message`](./message) | 메시지 · 국제화 | MessageSource, messages.properties, LocaleResolver |
| [`validation`](./validation) | 검증 | BindingResult, FieldError, Bean Validation, @Valid |
| [`login`](./login) | 로그인 · 보안 | 쿠키/세션, 서블릿 필터, 스프링 인터셉터, ArgumentResolver |
| [`exception`](./exception) | 예외 처리 | 서블릿 오류 처리, BasicErrorController, @ExceptionHandler, @ControllerAdvice |

## 디렉토리 구조

```
spring-mvc-2/
├── thymeleaf-basic/   # 타임리프 기본 문법
├── form/              # 타임리프 폼 처리
├── message/           # 메시지, 국제화
├── validation/        # 검증 (BindingResult, Bean Validation)
├── login/             # 로그인, 필터, 인터셉터
└── exception/         # 예외 처리와 오류 페이지, API 예외 처리
```

## 학습 내용 요약

### Thymeleaf Basic
- `th:text`, `th:utext`, `th:each`, `th:if`, `th:switch` 등 기본 표현식
- URL 표현식 `@{...}`, 변수 표현식 `${...}`, 메시지 표현식 `#{...}`
- 템플릿 조각(`th:fragment`)과 레이아웃 패턴

### Form
- `th:object`, `th:field`를 활용한 폼 바인딩
- 체크박스(단일/멀티), 라디오버튼, 셀렉트박스 처리
- Hidden 필드 처리(`_checkbox` 트릭)

### Message
- `MessageSource` 빈 등록 및 `messages.properties` 관리
- 국제화(i18n) 처리 및 `LocaleResolver` 설정

### Validation
- `BindingResult`와 `FieldError` / `ObjectError` 직접 처리
- `rejectValue()`, `reject()`를 통한 오류 코드 관리
- Bean Validation(`@NotNull`, `@NotBlank`, `@Range` 등) 적용
- `@Valid` / `@Validated` 차이 및 groups 기능

### Login
- 쿠키와 세션을 이용한 로그인 처리
- `HttpSession` 직접 사용 및 `@SessionAttribute`
- 서블릿 필터로 공통 관심사 처리
- 스프링 인터셉터(`HandlerInterceptor`) 구현 및 적용
- `ArgumentResolver`를 활용한 `@Login` 애노테이션 제작

### Exception
- 서블릿 예외 처리 흐름 (`Exception` 전파, `sendError`, WAS 재요청)
- 스프링 부트 오류 페이지 자동화 (`BasicErrorController`, `/error`)
- `HandlerExceptionResolver` 직접 구현 및 스프링 제공 구현체 활용
- `@ExceptionHandler`로 컨트롤러 단위 API 예외 처리
- `@ControllerAdvice`로 전역 예외 처리 분리

## 참고

- 강의: [스프링 MVC 2편 - 백엔드 웹 개발 활용 기술 (인프런)](https://www.inflearn.com/course/스프링-mvc-2)
- 강사: 김영한
