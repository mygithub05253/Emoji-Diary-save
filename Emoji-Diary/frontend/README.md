# 📝 Emoji-Diary
> 오늘의 감정을 기록하면 AI가 그림을 그려주는 일기장

![Project Status](https://img.shields.io/badge/status-completed-success)
![React](https://img.shields.io/badge/React-18.3-blue)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue)
![Vite](https://img.shields.io/badge/Vite-6.3-purple)

---

## 🎯 프로젝트 개요

**Emoji-Diary**는 사용자의 일기를 AI가 분석하여 그림과 코멘트를 생성하고, 감정 통계를 시각화하여 제공하는 모바일 웹 애플리케이션입니다.

### 핵심 가치

- ✨ **AI 기반 감정 분석**: KoBERT 모델을 활용한 자동 감정 분류 (7가지 감정)
- 🎨 **AI 이미지 생성**: 일기 내용을 바탕으로 그림일기 자동 생성
- 💬 **맞춤형 AI 코멘트**: 6가지 페르소나(베프, 부모님, 전문가, 멘토, 상담사, 시인) 선택 가능
- 📊 **감정 통계 시각화**: 캘린더, 타임라인, 차트 뷰를 통한 감정 추이 분석
- ⚠️ **위험 신호 감지**: 연속된 부정 감정 패턴을 감지하여 상담 기관 추천
- 🔐 **분리된 관리자 시스템**: 서비스 통계, 공지사항 관리, 시스템 설정 등

---

## 🛠 기술 스택 & 의사결정

### Core Technologies

| 기술 | 버전 | 선택 이유 |
|------|------|----------|
| **React** | 18.3 | 컴포넌트 기반 아키텍처로 재사용성과 유지보수성 확보 |
| **TypeScript** | 5.0 | 정적 타입 검사로 런타임 에러 방지, IDE 자동완성 및 리팩토링 지원 |
| **Vite** | 6.3 | 빠른 HMR(Hot Module Replacement)로 개발 생산성 향상, 번들 크기 최적화 |
| **React Router** | 7.9 | 클라이언트 사이드 라우팅, 사용자/관리자 앱 분리 관리 |

### UI/UX

- **Tailwind CSS 4.1**: 유틸리티 퍼스트 CSS 프레임워크로 빠른 스타일링
- **Shadcn UI (Radix UI)**: 접근성(A11y)을 고려한 컴포넌트 라이브러리, 커스터마이징 용이
- **Framer Motion**: 부드러운 애니메이션으로 사용자 경험 향상
- **Recharts**: 감정 통계를 시각화하기 위한 차트 라이브러리

### State Management

- **Context API**: 전역 인증 상태 관리 (사용자/관리자 분리)
- **Custom Hooks**: 비즈니스 로직 캡슐화 (`useAsync`, `useDiary`, `useUser` 등)
- **Local Storage**: JWT 토큰, 사용자 정보, 페르소나 설정 등 영구 저장

### API & Network

- **Axios**: HTTP 클라이언트, 인터셉터를 통한 토큰 자동 주입 및 에러 처리
- **Kakao Maps SDK**: 일기 내용 기반 장소 추천 기능
- **React Hook Form**: 폼 상태 관리 및 유효성 검증

### Build & Tools

- **Vite + SWC**: 빠른 번들링 속도 (CRA 대비 10배 이상 빠른 빌드)
- **ESLint + Prettier**: 코드 품질 유지

---

## ⚙️ 주요 기능 구현

### 1. AI 일기 생성 플로우

```typescript
// 일기 작성 시 순차적 비동기 처리
1. KoBERT 감정 분석 (일기 본문 분석)
2. AI 이미지 생성 (NanoVana API)
3. 일기 데이터 저장
4. AI 코멘트 생성 (Gemini API + 페르소나)
5. 음식 추천 생성 (Gemini API)
```

**구현 포인트:**
- ✨ **로딩 상태 관리**: 각 단계별 로딩 인디케이터 표시
- 🖼️ **이미지 Fallback 처리**: `ImageWithFallback` 컴포넌트로 로딩 실패 시 기본 이미지 표시
- ⚡ **에러 핸들링**: 네트워크 오류, API 오류 시 사용자 친화적 메시지 제공

### 2. 감정 분석 대시보드

**3가지 뷰 모드 지원:**
- 📅 **캘린더 뷰**: 월별 감정 히트맵 (Recharts 활용)
- 📊 **차트 뷰**: 파이/막대 차트를 통한 감정 분포 시각화
- 📝 **타임라인 뷰**: 날짜순 감정 추이 타임라인

**기능:**
- 기간 필터링 (주간/월간/연간)
- 감정별 통계 집계 및 시각화
- 반응형 차트 (모바일 최적화)

### 3. Kakao Map 장소 추천

**구현 방식:**
```typescript
// 일기 내용 분석 → 카테고리 매핑 → 카카오맵 장소 검색
- 일기 내용 기반 키워드 추출
- 카테고리별 장소 검색 (카페, 공원, 식당 등)
- 지도 마커 및 인포윈도우 표시
```

**기술적 고려사항:**
- 카카오맵 SDK 비동기 로딩 (`window.kakao` 체크)
- 지도 인스턴스 메모리 관리 (컴포넌트 언마운트 시 정리)

### 4. 커스텀 페르소나 시스템

**6가지 AI 페르소나:**
- 👫 베프 (친근하고 공감적)
- 👨‍👩‍👧 부모님 (따뜻하고 지지적)
- 💼 전문가 (전문적이고 분석적)
- 🎯 멘토 (동기부여하는 성장 코치)
- 🧠 상담사 (심리 분석 중심 치유자)
- 📝 시인 (감성적이고 철학적)

**구현:**
- 초기 회원가입 시 페르소나 선택 (필수)
- localStorage에 페르소나 설정 저장
- AI 코멘트 생성 시 페르소나 스타일 적용
- 마이페이지에서 페르소나 변경 가능

### 5. 관리자 대시보드

**주요 기능:**
- 📊 **서비스 통계**: DAU/WAU/MAU, 일기 작성 추이, 위험 레벨 분포
- 📢 **공지사항 관리**: CRUD, 고정 공지사항, 공개/비공개 설정
- ⚙️ **시스템 설정**: 위험 감지 기준 설정, 상담 기관 리소스 관리
- 📋 **에러 로그 조회**: 필터링, 검색, 상세 조회 (Stack Trace)

**구현 특징:**
- React Router로 사용자/관리자 앱 완전 분리
- 보호된 라우트 (Protected Routes) 구현
- JWT 토큰 기반 인증 및 자동 갱신

---

## 🐛 트러블 슈팅 & 성능 최적화

### 1. AI 이미지 로딩 최적화

**문제:**
- AI 이미지 생성 시간이 길어 사용자 이탈 가능성
- 네트워크 오류 시 빈 화면 표시

**해결:**
```typescript
// ImageWithFallback 컴포넌트 구현
- 이미지 로딩 실패 시 onError 핸들러로 fallback 이미지 표시
- Base64 인코딩된 기본 이미지 사용 (추가 네트워크 요청 없음)
- 로딩 중 스켈레톤 UI 표시
```

### 2. 대량 데이터 렌더링 최적화

**문제:**
- 캘린더 뷰에서 한 달치 감정 데이터 렌더링 시 성능 저하

**해결:**
```typescript
// React.memo 및 useMemo 활용
- 감정 데이터 메모이제이션
- 불필요한 리렌더링 방지
- 가상 스크롤링 고려 (향후 개선)
```

### 3. API 에러 처리 전략

**문제:**
- 네트워크 오류, 401 Unauthorized 등 다양한 에러 상황 처리

**해결:**
```typescript
// Axios Interceptor 구현
- Request Interceptor: JWT 토큰 자동 주입
- Response Interceptor: 
  - 401 에러 시 토큰 재발급 자동 시도
  - 재발급 실패 시 로그인 페이지로 리다이렉트
  - 네트워크 오류 시 사용자 친화적 메시지 표시
```

### 4. 상태 관리 최적화

**문제:**
- 복잡한 상태 로직으로 인한 코드 중복

**해결:**
```typescript
// Custom Hooks 패턴 적용
- useAsync: 비동기 작업 상태 관리 통합
- useDiary: 일기 관련 로직 캡슐화
- useUser: 사용자 정보 관리 로직 분리
- 코드 재사용성 향상 및 테스트 용이성 확보
```

### 5. 메모리 누수 방지

**문제:**
- Kakao Map 인스턴스, 이벤트 리스너 등 정리되지 않는 리소스

**해결:**
```typescript
// useEffect Cleanup 함수 활용
useEffect(() => {
  const mapInstance = new kakao.maps.Map(...);
  
  return () => {
    // 지도 인스턴스 정리
    mapInstance.destroy();
  };
}, []);
```

---

## 📁 프로젝트 구조

```
src/
├── app/                          # 앱 진입점
│   ├── App.tsx                   # 메인 라우터 (사용자/관리자 분기)
│   ├── UserApp.tsx              # 사용자 앱 진입점
│   └── main.tsx                 # React DOM 렌더링
│
├── features/                     # 기능별 모듈 분리 (Feature-Based Architecture)
│   ├── user/                     # 사용자 기능
│   │   ├── auth/                 # 인증 관련
│   │   │   ├── api/              # 인증 API 클라이언트
│   │   │   ├── components/       # 로그인, 회원가입, 비밀번호 찾기
│   │   │   └── hooks/            # 인증 관련 커스텀 훅
│   │   ├── diary/                # 일기 기능
│   │   │   ├── api/              # 일기 API 클라이언트
│   │   │   ├── components/       # 캘린더, 일기 작성/조회, 장소 추천
│   │   │   └── hooks/            # 일기 관련 커스텀 훅
│   │   ├── analysis/             # 감정 분석 기능
│   │   │   ├── api/              # 통계 API 클라이언트
│   │   │   └── components/       # 감정 통계 페이지, 차트 뷰
│   │   ├── profile/              # 프로필 기능
│   │   └── support/              # 지원 리소스
│   │
│   └── admin/                    # 관리자 기능
│       ├── AdminApp.tsx          # 관리자 앱 라우터
│       ├── api/                  # 관리자 API 클라이언트 (기능별 분리)
│       ├── pages/                # 페이지 컴포넌트 (페이지 기반 구조)
│       │   ├── dashboard/        # 대시보드
│       │   ├── notices/          # 공지사항 관리
│       │   ├── settings/         # 시스템 설정
│       │   └── logs/             # 에러 로그
│       ├── components/           # 재사용 컴포넌트
│       │   ├── ui/               # 공통 UI 컴포넌트
│       │   ├── layout/           # 레이아웃 컴포넌트
│       │   └── common/           # 공통 유틸리티 컴포넌트
│       ├── contexts/             # React Context (인증 상태)
│       ├── hooks/                # 커스텀 훅
│       ├── types/                # TypeScript 타입 정의
│       └── utils/                # 유틸리티 함수
│
├── shared/                       # 공통 코드
│   ├── api/                      # 공통 API 클라이언트
│   │   ├── client.ts             # Axios 인스턴스 (apiClient, adminApiClient)
│   │   ├── uploadApi.ts          # 이미지 업로드 API
│   │   ├── riskDetection.ts      # 위험 신호 감지 API
│   │   └── imageGenerator.ts     # AI 이미지 생성 API
│   ├── components/               # 공통 컴포넌트
│   │   ├── layout/               # MobileFrame, BottomNav 등
│   │   ├── ui/                   # Radix UI 기반 공통 UI
│   │   └── ImageWithFallback.tsx # 이미지 Fallback 컴포넌트
│   ├── hooks/                    # 공통 훅
│   │   ├── use-async.ts          # 비동기 작업 관리
│   │   ├── use-mobile.ts         # 모바일 화면 감지
│   │   └── use-modal.ts          # 모달 상태 관리
│   ├── lib/                      # 라이브러리 설정
│   │   └── utils.ts              # 유틸리티 함수 (cn, etc.)
│   ├── types/                    # 공통 타입 정의
│   └── utils/                    # 공통 유틸리티
│       ├── personaConverter.ts   # 페르소나 변환 (한글 ↔ enum)
│       └── riskCalculation.ts    # 위험 점수 계산
│
└── assets/                       # 정적 자산 (이미지, 아이콘)
```

### 아키텍처 설계 원칙

1. **Feature-Based Structure**: 기능별로 모듈 분리하여 관심사 분리
2. **재사용 가능한 컴포넌트**: `shared/components`에 공통 컴포넌트 배치
3. **API 클라이언트 분리**: 기능별 API 파일로 분리하여 유지보수성 향상
4. **타입 안정성**: TypeScript로 모든 API 응답 타입 정의
5. **커스텀 훅 패턴**: 비즈니스 로직을 훅으로 캡슐화하여 재사용성 확보

---

## 🚀 실행 방법

### 설치

```bash
npm install
```

### 개발 서버 실행

```bash
npm run dev
```

개발 서버는 `http://localhost:3000`에서 실행됩니다.

### 프로덕션 빌드

```bash
npm run build
```

---

## 📊 주요 성과

- ✅ **53개의 API 엔드포인트** 연동 완료
- ✅ **사용자/관리자 앱 완전 분리** 구조로 확장성 확보
- ✅ **타입 안정성**: TypeScript로 모든 데이터 모델 타입 정의
- ✅ **재사용 가능한 컴포넌트**: 50개 이상의 UI 컴포넌트 구현
- ✅ **성능 최적화**: 이미지 Fallback, 메모이제이션, 코드 스플리팅 적용

---

## 📝 주요 학습 내용

- 🏗️ **Feature-Based Architecture**: 확장 가능한 폴더 구조 설계
- 🎣 **Custom Hooks 패턴**: 비즈니스 로직 재사용 및 테스트 용이성 확보
- 🔐 **JWT 인증 구현**: 토큰 자동 주입, 재발급, 만료 처리
- 📱 **반응형 디자인**: 모바일 퍼스트 접근 방식
- 🎨 **컴포넌트 라이브러리 구축**: Shadcn UI 기반 디자인 시스템 구축
- ⚡ **성능 최적화**: React.memo, useMemo, 이미지 최적화 등

---

## 🔗 관련 링크

- [Figma 디자인](https://www.figma.com/design/2sHr7QgQ6syCbv5tKS8Jya)
- [API 명세서](./reference/API%20명세서.md)
- [ERD 설계서](./reference/ERD%20설계서.md)

---

## 📄 라이선스

이 프로젝트는 교육 목적으로 제작되었습니다.

---

**개발 기간**: 2025년 11월 ~ 12월  
**역할**: 프론트엔드 개발 리드 (Frontend Developer)
  