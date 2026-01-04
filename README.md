# 3F Web System - Claude Code 프로젝트 개발 프롬프트

> **Fast, Flexible, Frugal** - 빠르고 유연하며 비용 효율적인 웹 개발 시스템

Claude Code와 함께 사용하는 한국형 웹사이트 개발 스킬/프롬프트 시스템입니다.
업종별 최적화된 템플릿과 디자인 가이드라인을 제공합니다.

---

## 핵심 철학

### Fast - 빠른 개발
- 컴포넌트 기반 조립식 개발
- 검증된 패턴 재사용
- 슬래시 커맨드로 자동화

### Flexible - 유연한 커스터마이징
- Props 기반 변형
- 테마 시스템으로 일괄 변경
- 업종별 특화 가능

### Frugal - 비용 절감
- 무료 인프라 (GitHub + Vercel + Cloudflare)
- 최소 의존성
- 번들 사이즈 최적화

---

## 기술 스택

| 영역 | 기술 | 버전 | 비고 |
|------|------|------|------|
| Framework | Next.js | 14+ | App Router 필수 |
| Language | TypeScript | 5+ | Strict mode |
| Styling | Tailwind CSS | 3.4+ | JIT 모드 |
| UI | Shadcn/ui | latest | 엔터프라이즈급 |
| Animation | String Tune | latest | 웹 only (PWA 비활성화) |
| Image | Unsplash API | - | 개발용 더미 |
| Font | 프로젝트별 | - | font.md 기반 |
| Deploy | Vercel | - | 무료 Hobby |
| CDN/DNS | Cloudflare | - | 무료 |
| Repo | GitHub | - | Actions 포함 |

---

## 프로젝트 구조

```
.
├── hooks/
│   └── hooks.json          # Claude Code 훅 설정
│
└── skills/
    ├── core/               # 핵심 개발 원칙
    │   ├── SKILL.md        # 3F Web 핵심 스킬
    │   ├── architecture.md # 배포 아키텍처
    │   ├── component-first.md  # 컴포넌트 퍼스트 원칙
    │   ├── ui-library.md   # UI 라이브러리 가이드
    │   └── page-spec.md    # PAGE_SPEC 운영 규칙
    │
    ├── design/             # 디자인 시스템
    │   ├── SKILL.md        # 디자인 스킬 개요
    │   ├── color-system.md # 3색 테마 시스템
    │   ├── korea-trends.md # 한국 UI/UX 트렌드
    │   └── string-tune.md  # 인터랙션/애니메이션
    │
    ├── fonts/              # 폰트 시스템
    │   ├── SKILL.md        # 폰트 시스템 가이드
    │   └── font-template.md # font.md 템플릿
    │
    ├── deploy/             # 배포 가이드
    │   ├── SKILL.md        # 배포 스킬 개요
    │   ├── vercel.md       # Vercel 배포 가이드
    │   └── cloudflare.md   # Cloudflare 설정 가이드
    │
    └── categories/         # 업종별 카테고리
        └── SKILL.md        # 카테고리 정의
```

---

## 지원 업종 카테고리

### medical (의료)
- `aesthetic`: 피부과, 성형외과, 에스테틱
- `family-med`: 가정의학과, 내과
- `dental`: 치과
- `oriental`: 한의원

### fnb (요식업)
- `cafe`: 카페, 디저트
- `restaurant`: 레스토랑, 식당
- `bar`: 바, 펍

### beauty (뷰티)
- `hair-salon`: 헤어샵, 미용실
- `nail`: 네일샵
- `spa`: 스파, 마사지

### fitness (피트니스)
- `gym`: 헬스장, PT
- `yoga`: 요가, 필라테스
- `pilates`: 필라테스 전문

### education (교육)
- `academy`: 학원, 교습소
- `kindergarten`: 유치원, 어린이집
- `tutoring`: 과외, 입시

### professional (전문직)
- `law`: 법률사무소, 변호사
- `accounting`: 회계사, 세무사
- `consulting`: 컨설팅

---

## 디자인 원칙

### 3색 테마 시스템
- **Primary (60%)**: 브랜드 대표 색상, CTA 버튼
- **Secondary (30%)**: 배경, 카드, 보조 요소
- **Accent (10%)**: 특별 강조, 알림, 포인트

### 컴포넌트 퍼스트
```
Atom → Molecule → Organism → Template → Page
```
- 페이지를 만들지 말고, 컴포넌트를 조립하라

### 한국 트렌드 반영
- 풀스크린 히어로
- 벤토 그리드
- 스크롤 스토리텔링
- 오버사이즈 타이포그래피

---

## Hooks 시스템

`hooks/hooks.json`에 정의된 Claude Code 훅:

### PreToolUse (코드 작성 전 검증)
- 하드코딩 색상 금지 (테마 변수만 사용)
- 인라인 스타일 금지
- 재사용 가능한 컴포넌트 구조
- font.md 규칙 준수
- PWA 감지 로직 포함

### PostToolUse (작성 후 확인)
- TypeScript 타입 에러 확인
- import 경로 검증 (@/ alias)
- 접근성 속성 포함

### SessionStart (세션 시작 시)
- font.md 파일 존재 여부
- colors.json 설정 확인 (3색 이하)
- tailwind.config.ts 테마 설정

### Stop (작업 완료 전)
- 빌드 에러 확인
- 타입/린트 에러 확인
- 반응형 적용 확인

---

## 배포 아키텍처

```
GitHub (소스) → Vercel (호스팅) → Cloudflare (CDN/DNS)
   Free            Hobby (Free)         Free
```

### 무료 티어 제한

| 서비스 | 항목 | 제한 |
|--------|------|------|
| Vercel | Bandwidth | 100GB/월 |
| Vercel | Build 시간 | 6,000분/월 |
| Cloudflare | 요청/대역폭 | 무제한 |
| GitHub | Actions | 2,000분/월 |

---

## 사용 방법

### 1. 스킬 참조
프로젝트 시작 시 관련 스킬 문서를 참조합니다:
```
@skills/core/SKILL.md        # 핵심 원칙
@skills/design/SKILL.md      # 디자인 가이드
@skills/categories/SKILL.md  # 업종별 설정
```

### 2. 프로젝트 설정 파일 생성
```
font.md       # 폰트 가이드
colors.json   # 3색 테마 정의
```

### 3. 개발 시작
컴포넌트 퍼스트 원칙에 따라 Atom부터 시작합니다.

---

## 주요 규칙

### 코드 작성
- TypeScript 필수 (any 사용 금지)
- 서버 컴포넌트 우선
- `'use client'`는 필요한 경우에만

### 스타일링
- 하드코딩 색상 금지 (`bg-[#xxx]` 금지)
- 인라인 스타일 금지 (`style={{}}` 금지)
- 테마 변수 사용 (`bg-primary`, `text-secondary`)

### 이미지
- 항상 `next/image` 사용
- width, height 명시
- priority는 LCP 이미지에만

### 폰트
- `next/font` 사용
- display: 'swap' 설정
- 프로젝트별 font.md 기반

### 애니메이션
- String Tune 사용 (웹 전용)
- PWA에서는 비활성화 필수
- `shouldUseAnimation()` 체크 포함

---

## PAGE_SPEC 운영 규칙

### PAGE_SPEC이란?
페이지 생성의 **필수 입력 사양**입니다. `/gen-page` 커맨드 사용 시 반드시 제공해야 합니다.

### 표준 템플릿
```
[PAGE_SPEC]
category: realestate | travel | clinic
subcategory: (optional) dental | plastic | derma | general
page: home | landing | detail | pricing | contact
language: ko | vi | en | ko+vi
goal: lead-gen | info | booking
tone: clean | premium | friendly
[/PAGE_SPEC]
```

### 필수/선택 항목
| 구분 | 항목 | 설명 |
|------|------|------|
| 필수 | category | 업종 카테고리 |
| 필수 | page | 페이지 유형 |
| 필수 | language | 언어 |
| 필수 | goal | 페이지 목적 |
| 선택 | subcategory | 세부 업종 |
| 선택 | tone | 디자인 톤 |

### 운영 원칙

```
✅ PAGE_SPEC 없으면 생성하지 말 것
✅ 업종(category)을 추측하지 말 것
✅ 특수 연출은 명시적 호출 없으면 쓰지 말 것
```

### 역할 분리
| 구분 | 역할 |
|------|------|
| PAGE_SPEC | **무엇을** 만들지 정의 |
| Skill | **어떻게** 만들지 정의 |

### AI Slop 방지 규칙
- 무근거한 Inter / Roboto 고정 사용 금지
- 의미 없는 보라색 그라데이션 반복 금지
- 3카드 랜딩 구조 자동 수렴 금지
- 디자인 판단 우선순위: `PAGE_SPEC → Skill → 업종 맥락 → UX 관행`

### 특수 스킬 호출
Before/After 등 특수 연출은 명시적 호출 필요:
```
@skill special/before-after
```

상세 규칙: `@skills/core/page-spec.md` 참조

---

## 라이선스

이 프로젝트는 내부 사용 목적으로 제작되었습니다.
