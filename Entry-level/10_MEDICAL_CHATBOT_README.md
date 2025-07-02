# 🏥 을지대학교 의료IT 챗봇 프로젝트

## 📚 프로젝트 개요
HTML, CSS, JavaScript를 활용한 실무형 의료 챗봇 개발 실습 프로젝트입니다.
을지대학교 을GPT 팀을 위한 단계별 학습 자료입니다.

## 🎯 학습 목표
- CSS 변수와 의료 시스템 디자인
- Flexbox를 활용한 반응형 레이아웃
- DOM 조작과 이벤트 처리
- 의료 데이터 기반 응답 시스템 구현

## 📁 파일 구조

### 📖 단계별 실습 파일
```
Entry-level/
├── 10_medical_chatbot_project_part1.html    # Part 1: 기본 구조와 CSS 변수
├── 10_medical_chatbot_project_part2.html    # Part 2: 챗봇 UI 구조
├── 10_medical_chatbot_project_part3.html    # Part 3: JavaScript 기능
├── 10_medical_chatbot_project_part4.html    # Part 4: 고급 기능
└── 10_medical_chatbot_project_complete.html # 완성본
```

## 🚀 실습 진행 순서

### Part 1: 기본 구조와 CSS 변수
**학습 내용:**
- CSS Custom Properties (:root, var())
- 의료 시스템 색상 팔레트 설계
- 기본 리셋과 박스 모델
- 컨테이너 시스템과 타이포그래피

**실습 과제:**
1. CSS 변수를 수정해서 다른 색상 테마 만들기
2. --spacing 값들을 조정해서 간격 변화 확인
3. 새로운 CSS 변수 추가 (예: --color-accent)
4. 카드 컴포넌트에 호버 효과 추가

### Part 2: 챗봇 UI 구조
**학습 내용:**
- Flexbox를 활용한 챗봇 레이아웃
- 메시지 컴포넌트 설계 (사용자/봇 구분)
- CSS 애니메이션 (fadeInUp, pulse)
- 반응형 입력 폼과 버튼 상호작용

**실습 과제:**
1. 챗봇 UI에서 메시지 입력하고 전송해보기
2. 빠른 응답 버튼 클릭해서 메시지 추가하기
3. CSS에서 메시지 색상 변경해보기
4. 새로운 애니메이션 효과 추가하기

### Part 3: JavaScript 기능
**학습 내용:**
- DOM 요소 선택과 조작
- 이벤트 리스너 (form submit, keydown)
- 의료 키워드 기반 응답 시스템
- 타이핑 인디케이터와 비동기 처리

## 🎨 디자인 시스템

### 색상 팔레트
```css
--color-medical-primary: #006ba6;    /* 의료 블루 */
--color-medical-secondary: #0496c7;  /* 밝은 의료 블루 */
--color-medical-success: #2e8b57;    /* 의료 그린 */
--color-medical-error: #dc2626;      /* 응급 레드 */
--color-chat-user: #4f46e5;          /* 사용자 메시지 */
--color-chat-bot: #10b981;           /* 봇 메시지 */
```

### 타이포그래피
```css
--font-family-medical: 'Malgun Gothic', sans-serif;
--font-size-4xl: 2.25rem;  /* 제목 */
--font-size-2xl: 1.5rem;   /* 부제목 */
--font-size-base: 1rem;    /* 본문 */
```

## 💻 사용법

### 기본 사용
1. HTML 파일을 브라우저에서 열기
2. 텍스트 입력창에 증상이나 질문 입력
3. Enter 키 또는 전송 버튼으로 메시지 전송
4. 빠른 응답 버튼으로 자주 묻는 질문 선택

### 키보드 단축키
- `Alt + I`: 입력 필드 포커스
- `Alt + S`: 최신 메시지로 스크롤
- `Enter`: 메시지 전송
- `Shift + Enter`: 줄바꿈

### 테스트 키워드
- "두통", "머리 아픔" → 두통 관련 정보
- "복용약", "약물" → 약물 복용 안내
- "응급상황", "응급처치" → 응급상황 대처법
- "건강검진", "검진" → 건강검진 가이드

## 🔍 학습 포인트

### CSS 기법
- **CSS 변수 활용**: 유지보수성과 일관성 향상
- **Flexbox 레이아웃**: 반응형 챗봇 UI 구성
- **애니메이션**: @keyframes와 transition 활용
- **의료 테마**: 신뢰감 있는 색상 시스템

### JavaScript 기법
- **DOM 조작**: createElement, appendChild
- **이벤트 처리**: addEventListener 활용
- **배열과 객체**: 의료 데이터 구조화
- **비동기 처리**: setTimeout으로 실제 챗봇 경험

## ⚠️ 주의사항

### 기술적 한계
- 클라이언트 사이드 JavaScript로 구현
- 실제 AI가 아닌 키워드 기반 응답
- 데이터베이스 연동 없음 (로컬 저장)

## 🛠️ 개발 환경
- **언어**: HTML5, CSS3, JavaScript (ES6+)
- **브라우저**: Chrome, Firefox, Safari, Edge (최신 버전)
- **반응형**: 모바일, 태블릿, 데스크톱 지원
- **접근성**: 키보드 탐색, 스크린 리더 호환

## 📝 확장 가능성

### 기능 확장
- 서버 사이드 연동 (Node.js, Python)
- 실제 AI/ML 모델 통합
- 데이터베이스 연동 (MySQL, MongoDB)
- 사용자 인증 및 이력 관리

### UI/UX 개선
- 음성 입력/출력 기능
- 다국어 지원
- 다크 모드 구현
- PWA (Progressive Web App) 변환

## 🎓 제작 정보
- **제작자**: 안건 (을지대학교 의료IT 전공)
- **대상**: 을GPT팀원 및 의료IT 전공 학생
- **목적**: HTML, CSS, JavaScript 종합 실습
- **버전**: 1.0.0 (2024년 실습용)

##  문의 및 지원
실습 과정에서 궁금한 점이나 오류가 발생하면 언제든지 문의해주세요.

---
**⚕️ 을지대학교 의료IT 전공 실습 프로젝트**  
