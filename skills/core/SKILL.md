# 3F Web 핵심 스킬

## Fast - 빠른 개발
- 컴포넌트 기반 조립식 개발
- 검증된 패턴 재사용
- 슬래시 커맨드로 자동화

## Flexible - 유연한 커스터마이징
- Props 기반 변형
- 테마 시스템으로 일괄 변경
- 업종별 특화 가능

## Frugal - 비용 절감
- 무료 인프라 (GitHub + Vercel + Cloudflare)
- 최소 의존성
- 번들 사이즈 최적화

---

## 기술 스택 (고정)

| 영역 | 기술 | 버전 | 비고 |
|------|------|------|------|
| Framework | Next.js | 14+ | App Router 필수 |
| Language | TypeScript | 5+ | Strict mode |
| Styling | Tailwind CSS | 3.4+ | JIT 모드 |
| UI | Shadcn/ui | latest | 엔터프라이즈급 |
| Animation | String Tune | latest | 웹 only |
| Image | Unsplash API | - | 개발용 더미 |
| Font | 프로젝트별 | - | font.md 기반 |
| Deploy | Vercel | - | 무료 Hobby |
| CDN/DNS | Cloudflare | - | 무료 |
| Repo | GitHub | - | Actions 포함 |

---

## 컴포넌트 퍼스트 원칙

### 작업 순서 (반드시 준수)

```
1. Atom (Button, Input, Badge, Icon...)
     ↓
2. Molecule (Card, FormField, NavItem, SearchBar...)
     ↓
3. Organism (Header, Hero, Footer, Sidebar...)
     ↓
4. Template (PageLayout, SectionLayout...)
     ↓
5. Page (실제 라우트 페이지)
```

### 컴포넌트 파일 구조

```
src/
├── components/
│   ├── ui/                    # Shadcn 기본 (수정 최소화)
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   └── ...
│   │
│   ├── common/                # 공통 재사용
│   │   ├── Header/
│   │   │   ├── index.tsx
│   │   │   ├── Header.types.ts
│   │   │   ├── MobileNav.tsx
│   │   │   └── NavItem.tsx
│   │   ├── Footer/
│   │   └── Layout/
│   │
│   └── sections/              # 페이지 섹션
│       ├── Hero/
│       ├── Services/
│       └── Testimonial/
│
├── lib/
│   ├── utils.ts               # cn() 등 유틸리티
│   └── animation.ts           # String Tune + PWA 감지
│
└── styles/
    └── globals.css            # 글로벌 스타일
```

---

## 코드 작성 규칙

### TypeScript 필수
```tsx
// ❌ any 사용 금지
const data: any = fetchData();

// ✅ 명시적 타입
interface Data {
  id: string;
  title: string;
}
const data: Data = fetchData();
```

### 서버 컴포넌트 우선
```tsx
// 기본: 서버 컴포넌트 (아무 표시 없음)
export default function Page() {
  return <div>서버에서 렌더링</div>;
}

// 클라이언트 필요 시에만 명시
'use client';
export default function InteractiveComponent() {
  const [state, setState] = useState();
  return <button onClick={() => setState(...)}>클릭</button>;
}
```

### Import 정리
```tsx
// 1. React/Next
import { useState, useEffect } from 'react';
import Image from 'next/image';
import Link from 'next/link';

// 2. 외부 라이브러리
import { motion } from 'framer-motion';

// 3. 내부 컴포넌트
import { Button } from '@/components/ui/button';
import { Header } from '@/components/common/Header';

// 4. 유틸리티
import { cn } from '@/lib/utils';

// 5. 타입
import type { PageProps } from './page.types';
```

---

## 성능 최적화 규칙

### 이미지
- 항상 `next/image` 사용
- width, height 명시
- priority는 LCP 이미지에만

### 폰트
- `next/font` 사용
- display: 'swap' 설정
- preload 활성화

### 번들 사이즈
- 동적 import 활용
- 트리 쉐이킹 확인
- 불필요한 의존성 제거

---

## 참조 스킬

- 아키텍처: @skills/core/architecture.md
- 컴포넌트 상세: @skills/core/component-first.md
- UI 가이드: @skills/core/ui-library.md
