# 폰트 시스템

## 핵심 원칙

> **"각 프로젝트별 font.md 작업지시서를 먼저 작성하고, 이를 기반으로 Tailwind에 반영한다"**

폰트 시스템은 다음 순서로 설정합니다:
1. font.md 작업지시서 작성
2. 폰트 파일 다운로드/설정
3. next/font 설정
4. tailwind.config.ts 반영
5. globals.css 글로벌 스타일

---

## font.md 작업지시서

각 프로젝트 루트에 `font.md` 파일을 생성합니다.
템플릿: @skills/fonts/font-template.md

### 필수 포함 항목
- 폰트 패밀리 정의 (Heading, Body, Accent)
- 굵기(weight) 지정
- 크기 시스템 (Desktop/Mobile)
- 행간/자간 설정
- 용도별 사용 가이드

---

## 추천 한글 폰트

### 산스세리프 (본문/제목)

| 폰트명 | 특징 | 용도 | 라이선스 |
|--------|------|------|----------|
| **Pretendard** | 깔끔, 현대적 | 범용 | OFL |
| **Noto Sans KR** | 안정적, 표준 | 범용 | OFL |
| **Spoqa Han Sans** | 기하학적, 심플 | 테크 | OFL |
| **SUITE** | 둥글, 친근 | 서비스 | OFL |

### 세리프 (강조/특별)

| 폰트명 | 특징 | 용도 | 라이선스 |
|--------|------|------|----------|
| **Nanum Myeongjo** | 클래식 | 고급 브랜드 | OFL |
| **KoPub Batang** | 전통적 | 공공/교육 | OFL |

### 디스플레이 (포인트)

| 폰트명 | 특징 | 용도 | 라이선스 |
|--------|------|------|----------|
| **Gmarket Sans** | 굵은 웨이트 | 가격/CTA | 무료 |
| **Black Han Sans** | 매우 굵음 | 타이틀 | OFL |
| **Do Hyeon** | 동그란 | 캐주얼 | OFL |

---

## Next.js 폰트 설정

### 방법 1: 로컬 폰트 (권장)

```tsx
// app/layout.tsx
import localFont from 'next/font/local';

// Variable Font (가변 폰트) - 용량 최적화
const pretendard = localFont({
  src: '../fonts/PretendardVariable.woff2',
  display: 'swap',
  weight: '45 920',
  variable: '--font-pretendard',
});

// 포인트 폰트
const gmarketSans = localFont({
  src: [
    {
      path: '../fonts/GmarketSansBold.woff2',
      weight: '700',
    },
  ],
  display: 'swap',
  variable: '--font-gmarket',
});

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="ko" className={`${pretendard.variable} ${gmarketSans.variable}`}>
      <body className="font-sans">{children}</body>
    </html>
  );
}
```

### 방법 2: Google Fonts

```tsx
// app/layout.tsx
import { Noto_Sans_KR, Nanum_Myeongjo } from 'next/font/google';

const notoSansKr = Noto_Sans_KR({
  subsets: ['latin'],
  weight: ['400', '500', '700'],
  display: 'swap',
  variable: '--font-noto-sans',
});

const nanumMyeongjo = Nanum_Myeongjo({
  subsets: ['latin'],
  weight: ['400', '700'],
  display: 'swap',
  variable: '--font-nanum-myeongjo',
});
```

---

## Tailwind 설정

### tailwind.config.ts

```typescript
import type { Config } from 'tailwindcss';

const config: Config = {
  content: ['./src/**/*.{js,ts,jsx,tsx,mdx}'],
  theme: {
    extend: {
      fontFamily: {
        // 기본 폰트 (본문)
        sans: ['var(--font-pretendard)', 'sans-serif'],
        
        // 제목용 폰트
        heading: ['var(--font-pretendard)', 'sans-serif'],
        
        // 포인트 폰트
        accent: ['var(--font-gmarket)', 'sans-serif'],
        
        // 세리프 (필요시)
        serif: ['var(--font-nanum-myeongjo)', 'serif'],
      },
      
      fontSize: {
        // font.md 기반 커스텀 사이즈
        'display-1': ['4rem', { lineHeight: '1.1', letterSpacing: '-0.02em' }],
        'display-2': ['3rem', { lineHeight: '1.2', letterSpacing: '-0.02em' }],
        'heading-1': ['2.25rem', { lineHeight: '1.3', letterSpacing: '-0.01em' }],
        'heading-2': ['1.875rem', { lineHeight: '1.3', letterSpacing: '-0.01em' }],
        'heading-3': ['1.5rem', { lineHeight: '1.4' }],
        'body-lg': ['1.125rem', { lineHeight: '1.7' }],
        'body': ['1rem', { lineHeight: '1.7' }],
        'body-sm': ['0.875rem', { lineHeight: '1.6' }],
        'caption': ['0.75rem', { lineHeight: '1.5' }],
      },
      
      letterSpacing: {
        korean: '-0.02em',
        tight: '-0.01em',
      },
      
      lineHeight: {
        korean: '1.7',
        tight: '1.3',
        relaxed: '1.8',
      },
    },
  },
  plugins: [],
};

export default config;
```

---

## 글로벌 스타일

### globals.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    font-family: var(--font-pretendard), sans-serif;
    letter-spacing: -0.02em;
    line-height: 1.7;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  
  /* 제목 스타일 */
  h1, h2, h3, h4, h5, h6 {
    @apply font-heading font-bold tracking-tight;
    line-height: 1.3;
  }
  
  h1 {
    @apply text-4xl md:text-5xl lg:text-6xl;
  }
  
  h2 {
    @apply text-3xl md:text-4xl;
  }
  
  h3 {
    @apply text-2xl md:text-3xl;
  }
  
  /* 본문 스타일 */
  p {
    @apply text-base md:text-lg leading-relaxed;
  }
  
  /* 포인트 텍스트 */
  .accent-text {
    @apply font-accent;
  }
  
  /* 가격/숫자 */
  .price {
    @apply font-accent font-bold tracking-tight;
  }
}
```

---

## 사용 예시

```tsx
// 제목
<h1 className="font-heading text-display-1">
  아름다움을 디자인합니다
</h1>

// 본문
<p className="font-sans text-body text-muted">
  20년 경력의 전문의가 함께합니다
</p>

// 가격/포인트
<span className="font-accent text-2xl font-bold text-primary">
  ₩1,500,000
</span>

// CTA 버튼
<button className="font-heading font-semibold">
  지금 예약하기
</button>
```

---

## 체크리스트

- [ ] font.md 작업지시서 작성됨
- [ ] 폰트 파일 다운로드됨 (로컬 폰트 사용 시)
- [ ] app/layout.tsx에 폰트 설정됨
- [ ] CSS 변수 (--font-*) 정의됨
- [ ] tailwind.config.ts에 fontFamily 설정됨
- [ ] globals.css에 기본 타이포그래피 설정됨
- [ ] 반응형 크기 조정 적용됨
