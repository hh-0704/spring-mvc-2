# Thymeleaf Basic

> 인프런 **[스프링 MVC 2편 - 백엔드 웹 개발 활용 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2)** (김영한) **섹션 2 - 타임리프 - 기본 기능** 을 수강하며 직접 구현한 학습용 코드베이스입니다.

<br>

## 🛠 Tech Stack

| 항목 | 버전 |
|------|------|
| Java | 25 |
| Spring Boot | 4.0.3 |
| Thymeleaf | Spring Boot 관리 버전 |
| Lombok | - |
| Build Tool | Gradle |

<br>

## 📚 학습 내용

### 1. 텍스트 출력

| 경로 | 설명 |
|------|------|
| `/basic/text-basic` | `th:text`를 이용한 기본 텍스트 출력 |
| `/basic/text-unescaped` | `th:text`(이스케이프) vs `th:utext`(언이스케이프) 비교 |

- HTML 엔티티(`<`, `>`, `&` 등)의 이스케이프 처리 원리 이해
- 언이스케이프 사용 시 XSS 취약점 주의

---

### 2. 표준 표현식 구문 (Standard Expression Syntax)

| 경로 | 설명 |
|------|------|
| `/basic/variable` | **SpringEL** - `${...}` 변수 표현식 |
| `/basic/basic-objects` | 기본 내장 객체 (`#request`, `#session`, `#locale` 등) |
| `/basic/date` | 유틸리티 객체와 날짜 (`#temporals`) |
| `/basic/link` | 링크 URL 표현식 `@{...}` |
| `/basic/literal` | 리터럴 (텍스트, 숫자, 불린, null, 리터럴 토큰) |
| `/basic/operation` | 산술·비교·조건(삼항/Elvis) 연산 |

- `${user.name}`, `${user['name']}`, `${userMap['userA'].name}` 등 다양한 접근 방식 실습
- `@{/basic/items/{itemId}(itemId=${item.id}, query='test')}` 형태의 URL 파라미터 처리

---

### 3. 속성 값 설정

| 경로 | 설명 |
|------|------|
| `/basic/attribute` | `th:*` 속성 재정의, `th:attrappend`, `th:classappend`, `th:checked` 등 |

---

### 4. 반복

| 경로 | 설명 |
|------|------|
| `/basic/each` | `th:each`를 이용한 리스트·맵 반복, 반복 상태 변수 (`iterStat`) 활용 |

- `index`, `count`, `size`, `first`, `last`, `odd`, `even` 등 반복 상태 변수 실습

---

### 5. 조건부 평가

| 경로 | 설명 |
|------|------|
| `/basic/condition` | `th:if`, `th:unless`, `th:switch` / `th:case` |

---

### 6. 주석 및 블록

| 경로 | 설명 |
|------|------|
| `/basic/comments` | HTML 주석, Thymeleaf 파서 주석, 프로토타입 주석 비교 |
| `/basic/block` | `th:block` - 렌더링 시 제거되는 범위 태그 |

---

### 7. 자바스크립트 인라인

| 경로 | 설명 |
|------|------|
| `/basic/javascript` | `th:inline="javascript"` - 자바스크립트 내 Thymeleaf 표현식, 내추럴 템플릿, `each` 인라인 |

---

### 8. 템플릿 레이아웃

| 경로 | 설명 |
|------|------|
| `/template/fragment` | `th:fragment`, `th:insert`, `th:replace`를 이용한 템플릿 조각 재사용 |
| `/template/layout` | 조각에 마크업을 전달하는 **유연한 레이아웃** 패턴 |
| `/template/layoutExtend` | 공통 레이아웃을 상속받아 각 페이지에서 내용을 채우는 **레이아웃 상속** 패턴 |

<br>

## 🔗 참고

- 강의: [스프링 MVC 2편 - 백엔드 웹 개발 활용 기술 (김영한)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2)
- 공식 문서: [Thymeleaf Documentation](https://www.thymeleaf.org/documentation.html)
