# 실시간 주식 시장 애플리케이션 개발 가이드

## I. 초기 설정 및 환경 구축

### 1. 필수 도구 설정
- **Ingest 설정**
  - 실시간 워크플로, 트리거 및 AI 기반 알림 처리를 위한 계정 생성 및 설정
- **Code Rabbit 설정**
  - AI 기반 코드 검토 및 PR 자동화를 위한 계정 생성 및 연결

### 2. Next.js 프로젝트 생성 및 구성
```bash
npx create-next-app
npm run dev
```
- TypeScript, ESLint, Tailwind CSS, App Router, Turbo Pack 설정
- 애플리케이션 실행으로 설치 확인

### 3. 디자인 시스템 및 테마 설정
```bash
npx shadcn@latest init
```
- Shadcn 디자인 시스템 설치
- 다크 모드를 기본 테마로 설정 (차트 및 데이터 시각화에 적합)
- 커스텀 색상 테마 설정 및 `globals.css` 업데이트

### 4. 애셋 및 메타데이터 관리
- 로고, 아이콘, 이미지 등을 `public` 폴더에 추가
- 파비콘(favicon) 및 애플리케이션 제목(title) 업데이트

### 5. 버전 관리 시스템 설정
- GitHub 저장소 생성 및 초기 커밋 푸시

---

## II. 공통 레이아웃 및 내비게이션 구현

### 1. 경로 그룹(Route Group) 설정
- `app` 폴더 내에 `(root)` 경로 그룹 폴더 생성
- 기존 레이아웃과 홈 페이지 이동
- 인증 페이지와 구분되는 루트 레이아웃 정의

### 2. 루트 레이아웃 및 헤더 구현
- `root` 폴더 내 `layout.tsx` 생성
- 메인 태그로 자식 컴포넌트(`children`) 렌더링
- `Header` 컴포넌트 생성 및 레이아웃에 추가

### 3. 내비게이션 컴포넌트 개발
- 재사용 가능한 `Nav Items` 컴포넌트 생성
  - 대시보드, 검색, 관심 목록 등의 링크 정의
- 클라이언트 훅으로 현재 활성 링크 표시
- `Drop-down Menu` 및 `Avatar` Shadcn 컴포넌트 설치
- `User Dropdown` 컴포넌트 구현
  - 사용자 정보 및 로그아웃 기능 제공

### 4. 코드 검토 및 병합
- `header` 브랜치 생성
- Code Rabbit으로 AI 기반 코드 검토
- 메인 브랜치 병합

---

## III. 홈페이지 UI 구현

### 1. Trading View 위젯 통합 준비
- Trading View 무료 위젯 활용 (차트, 히트 맵, 시세 등)
- 위젯 구성을 상수로 추출
- 재사용 가능한 커스텀 훅 `useTradingViewWidget` 생성
  - 위젯 렌더링 및 정리 로직 추상화
- 재사용 가능한 `TradingViewWidget` 컴포넌트 구현

### 2. 홈페이지 레이아웃 완성
CSS 그리드를 사용하여 4가지 Trading View 위젯 배치:
- **시장 개요(Market Overview)** 차트
- **주식 히트 맵(Stock Heat Map)**
- **주요 뉴스(Top Stories)**
- **시장 시세(Market Quotes)**

변경 사항 커밋 및 병합

---

## IV. 인증 UI 및 양식 구현

### 1. 인증 경로 및 레이아웃 설정
- `(auth)` 경로 그룹 폴더 및 `layout.tsx` 생성
- `sign-in/page.tsx` 및 `sign-up/page.tsx` 파일 구조 생성
- 분할 화면 레이아웃 구현 (좌측 폼, 우측 이미지/사용 후기)

### 2. 폼 구성 요소 개발
- `React Hook Form` 라이브러리 설치
- Shadcn의 `input`, `label`, `select` 컴포넌트 설치
- `global.d.ts` 생성으로 폼 데이터 타입 관리
- 재사용 가능한 컴포넌트 생성:
  - `InputField`: React Hook Form 통합, 유효성 검사 및 오류 메시지 표시
  - `SelectField`: `Controller`를 사용한 React Hook Form 제어
  - `CountrySelectField`: 국가 선택 필드
  - `FooterLink`: 폼 간 전환 링크

### 3. 로그인/회원가입 페이지 완성
**회원가입 페이지 필드:**
- 이름
- 이메일
- 비밀번호
- 투자 목표
- 리스크 허용 범위
- 선호 산업

**로그인 페이지:**
- 이메일
- 비밀번호

변경 사항 커밋 및 병합

---

## V. 데이터베이스 및 인증 로직 설정

### 1. 데이터베이스 연결
- MongoDB Atlas에서 클러스터 및 사용자 생성
- 연결 문자열 확보
- Mongoose 및 MongoDB 라이브러리 설치
- `mongoose.ts` 파일에 데이터베이스 연결 함수 구현
  - 싱글톤 패턴으로 중복 연결 방지
  - 전역 캐시 사용

### 2. 인증 로직 구현 (Better Auth)
- Better Auth 라이브러리 설치
- 환경 변수 설정 (SECRET, BASE_URL)
- `getAuth` 싱글톤 함수 구현:
  - MongoDB 어댑터
  - 이메일/비밀번호 인증
  - 세션 및 쿠키 처리 플러그인

### 3. 백그라운드 워크플로 설정 (Ingest)
- Ingest CLI 및 패키지 설치
- Gemini API 키를 환경 변수에 설정
- Ingest 클라이언트 생성
- Next.js API 경로 설정으로 Ingest 워크플로 노출
- `send_signup_email` 백그라운드 함수 정의
  - `app/user.created` 이벤트 트리거
  - AI(Gemini 2.5 flash light)로 개인화된 환영 메시지 생성

### 4. 이메일 전송 (Node Mailer)
- Node Mailer 라이브러리 설치
- Google 앱 비밀번호 설정
- Transporter 구성
- 이메일 템플릿 정의
- `send_welcome_email` 함수 구현
  - 동적 데이터(이름, 소개 문구) 대체

### 5. 인증 흐름 완성
- 서버 액션 구현:
  - `signup_with_email`: Better Auth로 사용자 생성 및 세션 관리, Ingest 이벤트 트리거
  - `sign_in_with_email`: 로그인 처리
  - `sign_out`: 로그아웃 처리
- 경로 보호(Route Protection) 설정
  - Next.js 미들웨어 및 세션 확인
  - 인증되지 않은 사용자의 홈 페이지 접근 차단

---

## VI. 일일 뉴스 요약 시스템 구현

### 1. Ingest Cron 워크플로 설정
- `send_daily_news_summary` Ingest 함수 생성
- Cron 표현식 `0 12 * * *` 사용
- 매일 UTC 12:00에 자동 실행

### 2. 뉴스 배달 단계 정의 및 구현

**1단계: 사용자 목록 가져오기**
- `get_all_users_for_news_email` 서버 액션 구현
- 뉴스 수신을 원하는 모든 사용자 데이터베이스에서 조회

**2단계: 관심 종목 및 뉴스 가져오기**
- `WatchListModel` 스키마 정의 (사용자 ID, 종목 심볼 등)
- FinHub API 키 설정
- `get_news_by_symbol` 서버 액션 구현
  - 사용자 관심 종목 또는 일반 시장 뉴스 가져오기

**3단계: AI 뉴스 요약**
- Ingest AI의 `infer` 기능 사용
- 사용자별 간결하고 개인화된 요약 다이제스트 생성

**4단계: 이메일 발송**
- `send_news_summary_email` Node Mailer 함수 구현
- Ingest 워크플로 마지막 단계에서 요약 뉴스 발송

### 3. 워크플로 등록 및 테스트
- 새로운 Ingest 함수를 API 경로에 등록
- Cron 일정 조정 또는 수동 실행으로 테스트
- 이메일 발송 성공 확인

---

## VII. 검색 및 주식 상세 페이지 구현

### 1. 검색 커맨드 구현
- Shadcn Command UI 활용
- `search-command.tsx` 컴포넌트 구현
- `useDebounce` 커스텀 훅 구현
  - 사용자 입력이 멈춘 후 API 호출
- `search_stocks` 서버 액션 구현
  - FinHub API로 주식 심볼 또는 회사 이름 검색
  - `useDebounce`와 연결

### 2. 주식 상세 페이지 구현
- 동적 경로 생성: `app/root/stocks/[symbol]/page.tsx`
- Trading View 위젯 컴포넌트 재사용
- 반응형 레이아웃 구성:
  - 주식별 차트
  - 기술적 분석
  - 회사 재무 정보

---

## VIII. 최종 배포

### 1. 코드 정리 및 커밋
- 모든 변경 사항 커밋
- GitHub에 푸시

### 2. Vercel 배포
- Vercel에 프로젝트 임포트
- 환경 변수 설정
- 빌드 에러 방지 설정:
  - ESLint 검사 무시
  - TypeScript 검사 무시

### 3. Ingest Production 동기화
- Vercel 통합 또는 수동 동기화
- Ingest 워크플로를 배포된 프로덕션 애플리케이션에 연결
- 프로덕션 환경 테스트:
  - 인증 정상 작동 확인
  - 환영 이메일 발송 확인

### 4. 다음 단계
**최종 과제: 관심 목록(Watch List) 기능 구현**
- 검색 결과에 별표 버튼 추가
- 관심 종목만 요약하는 기능을 Daily Summary에 반영

---

## 주요 기술 스택

### Frontend
- Next.js (App Router)
- TypeScript
- Tailwind CSS
- Shadcn UI
- React Hook Form
- TradingView 위젯

### Backend
- MongoDB (Mongoose)
- Better Auth
- Ingest (워크플로 & Cron)
- Node Mailer

### API & Services
- FinHub API (주식 데이터 및 뉴스)
- Gemini AI (개인화된 메시지 생성)
- TradingView (실시간 금융 데이터)

### DevOps
- GitHub (버전 관리)
- Vercel (배포)
- Code Rabbit (AI 코드 검토)