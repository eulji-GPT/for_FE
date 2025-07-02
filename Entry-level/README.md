# HTML/CSS 학습 예제 모음

이 폴더에는 HTML과 CSS를 체계적으로 학습할 수 있는 실습 예제들이 포함되어 있습니다. 각 파일은 단계별로 구성되어 있으며, 실무에서 자주 사용되는 기법들을 다룹니다.

## 📁 파일 구성

### 1. `01_html_basics.html`
**HTML 기초 - 문서 구조와 기본 태그**
- **학습 목표**: HTML의 기본 구조와 핵심 태그들 이해
- **주요 내용**:
  - HTML 문서의 기본 구조 (`<!DOCTYPE>`, `<html>`, `<head>`, `<body>`)
  - 제목 태그 (h1~h6)
  - 단락과 줄바꿈 (`<p>`, `<br>`)
  - 텍스트 서식 (`<strong>`, `<em>`, `<u>`, `<del>`, `<mark>`)
  - 목록 (`<ul>`, `<ol>`, `<li>`)
  - 링크와 이미지 (`<a>`, `<img>`)
  - HTML 주석
- **실습 포인트**: 시맨틱 HTML의 중요성과 올바른 태그 사용법

### 2. `02_css_basics.html`
**CSS 기초 - 선택자와 스타일 속성**
- **학습 목표**: CSS 선택자와 기본 스타일 속성 마스터
- **주요 내용**:
  - CSS 선택자 (태그, 클래스, ID, 전체 선택자)
  - 텍스트 스타일 (색상, 크기, 정렬, 폰트)
  - 박스 모델 (width, height, padding, margin, border)
  - 배경 스타일 (단색, 그라디언트)
  - 위치 속성 (position: relative, absolute, fixed)
  - Flexbox 기초
  - CSS Grid 기초
  - 호버 효과와 트랜지션
  - 반응형 디자인 (@media queries)
- **실습 포인트**: 실제 웹사이트에서 사용되는 스타일링 기법

### 3. `03_forms_tables.html`
**폼과 테이블 - HTML 실습**
- **학습 목표**: 사용자 입력과 데이터 표시 요소 완전 정복
- **주요 내용**:
  - 폼의 기본 구조 (`<form>`, `action`, `method`)
  - 다양한 입력 타입들 (text, email, password, number, date, etc.)
  - 선택 요소들 (checkbox, radio, select)
  - 폼 검증 (required, pattern, min/max)
  - 테이블 구조 (`<table>`, `<thead>`, `<tbody>`, `<tr>`, `<th>`, `<td>`)
  - 복잡한 테이블 (rowspan, colspan)
  - 반응형 테이블 처리
- **실습 포인트**: 실무에서 사용되는 완전한 회원가입 폼과 데이터 테이블

### 4. `04_layout_flexbox_grid.html`
**CSS 레이아웃 마스터 - Flexbox와 Grid**
- **학습 목표**: 현대적인 CSS 레이아웃 기법 완전 정복
- **주요 내용**:
  - **Flexbox 완전 가이드**:
    - flex-direction (row, column, reverse)
    - justify-content (정렬 옵션들)
    - align-items (교차축 정렬)
    - flex-grow, flex-shrink, flex-basis
  - **CSS Grid 완전 가이드**:
    - grid-template-columns/rows
    - grid-areas를 이용한 레이아웃
    - repeat(), minmax(), auto-fit/auto-fill
  - **실제 레이아웃 예제**:
    - 네비게이션 바
    - 카드 그리드 시스템
    - 댓글 시스템
    - 전체 페이지 레이아웃
- **실습 포인트**: Flexbox vs Grid 언제 무엇을 사용할지 판단 기준

### 5. `05_responsive_design.html`
**반응형 웹 디자인 - 모든 기기에 최적화**
- **학습 목표**: 완벽한 반응형 웹사이트 제작 능력 습득
- **주요 내용**:
  - **반응형 기초**:
    - Viewport 메타 태그
    - 미디어 쿼리 (@media)
    - 브레이크포인트 설정
  - **고급 반응형 기법**:
    - CSS clamp() 함수
    - CSS 변수와 미디어 쿼리
    - Container Queries
    - 화면 방향 대응
  - **실용적 예제**:
    - 반응형 그리드 시스템
    - 자동 조절 카드 레이아웃
    - 반응형 네비게이션
    - 반응형 폼
    - 반응형 테이블
  - **접근성과 사용성**:
    - 터치 친화적 디자인
    - 다크 모드 대응
    - 성능 최적화
- **실습 포인트**: 모바일 퍼스트 접근법과 크로스 브라우저 호환성

## 🎯 학습 순서 추천

### 초급자 (HTML/CSS 처음)
1. `01_html_basics.html` → HTML 기본 구조 이해
2. `02_css_basics.html` → CSS 기초 스타일링
3. `03_forms_tables.html` → 사용자 입력 요소들

### 중급자 (기본 지식 보유)
1. `04_layout_flexbox_grid.html` → 현대적 레이아웃 기법
2. `05_responsive_design.html` → 반응형 디자인 완성

### 고급자 (실무 적용)
- 모든 파일을 참고하여 종합적인 프로젝트 진행
- 코드를 분석하고 개선점 찾기
- 새로운 기법 실험해보기

## 💡 활용 방법

### 1. **단계별 학습**
- 각 파일을 순서대로 열어보며 코드와 결과를 비교
- 직접 코드를 수정해가며 변화 확인
- 주석을 참고하여 각 속성의 의미 파악

### 2. **실습 중심 학습**
- 브라우저 개발자 도구로 실시간 코드 수정
- 예제를 참고하여 자신만의 프로젝트 제작
- 다양한 브라우저와 기기에서 테스트

### 3. **문제 해결 연습**
- 의도적으로 코드를 깨뜨려보고 수정하기
- 새로운 기능 추가해보기
- 성능 최적화 시도하기

## 🔧 개발 환경 설정

### 필수 도구
- **텍스트 에디터**: VS Code, Sublime Text, Atom 등
- **브라우저**: Chrome (개발자 도구), Firefox, Safari
- **확장 프로그램**: Live Server, Prettier, Auto Rename Tag

### 추천 확장 프로그램 (VS Code)
```
- Live Server: 실시간 미리보기
- Prettier: 코드 자동 정렬
- Auto Rename Tag: 태그 자동 수정
- CSS Peek: CSS 정의 빠른 확인
- HTML CSS Support: 자동완성 지원
```

## 📚 추가 학습 자료

### 공식 문서
- [MDN Web Docs](https://developer.mozilla.org/ko/) - HTML/CSS 완전 가이드
- [W3C Standards](https://www.w3.org/) - 웹 표준 문서
- [Can I Use](https://caniuse.com/) - 브라우저 호환성 확인

### 유용한 사이트
- [CSS-Tricks](https://css-tricks.com/) - CSS 고급 기법
- [Flexbox Froggy](https://flexboxfroggy.com/) - Flexbox 게임으로 학습
- [Grid Garden](https://cssgridgarden.com/) - CSS Grid 게임으로 학습
- [CodePen](https://codepen.io/) - 코드 실험과 공유

### 디자인 참고
- [Dribbble](https://dribbble.com/) - 웹 디자인 영감
- [Behance](https://www.behance.net/) - UI/UX 디자인 포트폴리오
- [Awwwards](https://www.awwwards.com/) - 우수한 웹사이트 모음

## 🎨 프로젝트 아이디어

### 초급 프로젝트
- 개인 소개 페이지
- 간단한 블로그 레이아웃
- 레스토랑 메뉴 페이지

### 중급 프로젝트
- 반응형 포트폴리오 사이트
- 온라인 쇼핑몰 레이아웃
- 뉴스 사이트 클론

### 고급 프로젝트
- 관리자 대시보드
- 소셜 미디어 피드
- 복합 웹 애플리케이션 UI

## ❓ 자주 묻는 질문

### Q: 어떤 순서로 학습해야 하나요?
A: HTML 기초 → CSS 기초 → 폼/테이블 → 레이아웃 → 반응형 순서를 추천합니다.

### Q: 실무에서 가장 중요한 부분은?
A: Flexbox/Grid 레이아웃과 반응형 디자인이 현재 실무에서 가장 중요합니다.

### Q: 브라우저 호환성은 어떻게 확인하나요?
A: Can I Use 사이트에서 각 CSS 속성의 브라우저 지원 현황을 확인할 수 있습니다.

### Q: 성능 최적화는 어떻게 하나요?
A: 불필요한 CSS 제거, 이미지 최적화, 미디어 쿼리 효율화 등이 중요합니다.

---

## 📞 피드백 및 문의

이 학습 자료에 대한 피드백이나 질문이 있으시면 언제든지 연락해 주세요. 
더 나은 학습 자료 제작을 위해 여러분의 의견을 소중히 받겠습니다.

**Happy Coding! 🚀**
