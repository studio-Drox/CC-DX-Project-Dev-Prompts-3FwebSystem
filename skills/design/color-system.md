# 3색 테마 시스템

## 핵심 원칙

> **"테마 컬러는 3개를 넘지 않는다"**

3색 제한은 디자인 일관성과 브랜드 아이덴티티 강화를 위한 규칙입니다.

---

## 3색 구성

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  PRIMARY (주요색)                                    │
│  - 브랜드 대표 색상                                   │
│  - CTA 버튼, 헤더, 주요 강조                          │
│  - 사용 비율: 60%                                    │
│                                                     │
├─────────────────────────────────────────────────────┤
│                                                     │
│  SECONDARY (보조색)                                  │
│  - Primary를 보완하는 색상                            │
│  - 배경, 카드, 보조 요소                              │
│  - 사용 비율: 30%                                    │
│                                                     │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ACCENT (강조색)                                     │
│  - 특별한 강조, 알림, 포인트                          │
│  - 배지, 알림, 특별 이벤트                            │
│  - 사용 비율: 10%                                    │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## colors.json 형식

각 프로젝트 루트에 `colors.json` 파일 생성:

```json
{
  "name": "aesthetic-clinic-theme",
  "description": "피부과 클리닉 테마 - 청결/신뢰/전문성",
  
  "colors": {
    "primary": {
      "name": "Royal Purple",
      "hex": "#8B5CF6",
      "hsl": "258 90% 66%",
      "usage": "CTA 버튼, 헤더 로고, 주요 강조"
    },
    "secondary": {
      "name": "Soft Gray",
      "hex": "#F1F5F9",
      "hsl": "210 40% 96%",
      "usage": "배경, 카드, 섹션 구분"
    },
    "accent": {
      "name": "Rose Gold",
      "hex": "#F43F5E",
      "hsl": "347 77% 60%",
      "usage": "할인 배지, 특별 프로모션, 알림"
    }
  },
  
  "semantic": {
    "background": "#FFFFFF",
    "foreground": "#0F172A",
    "muted": "#64748B",
    "border": "#E2E8F0"
  }
}
```

---

## 업종별 추천 팔레트

### 의료/피부과
```json
{
  "primary": "#8B5CF6",    // 보라 - 고급스러움
  "secondary": "#F1F5F9",  // 연한 회색 - 청결
  "accent": "#EC4899"      // 핑크 - 뷰티
}
```

### 가정의학과/내과
```json
{
  "primary": "#0EA5E9",    // 하늘색 - 신뢰
  "secondary": "#F0FDF4",  // 연한 초록 - 건강
  "accent": "#22C55E"      // 초록 - 생명력
}
```

### 치과
```json
{
  "primary": "#06B6D4",    // 청록 - 청결/신뢰
  "secondary": "#F8FAFC",  // 화이트 계열
  "accent": "#F59E0B"      // 골드 - 전문성
}
```

### 카페
```json
{
  "primary": "#78350F",    // 커피 브라운
  "secondary": "#FEF3C7",  // 크림
  "accent": "#059669"      // 민트
}
```

### 피트니스
```json
{
  "primary": "#DC2626",    // 레드 - 에너지
  "secondary": "#18181B",  // 다크 그레이
  "accent": "#FACC15"      // 옐로우 - 활력
}
```

---

## Tailwind 설정 반영

### tailwind.config.ts
```typescript
import type { Config } from 'tailwindcss';

const config: Config = {
  content: ['./src/**/*.{js,ts,jsx,tsx,mdx}'],
  theme: {
    extend: {
      colors: {
        // 시맨틱 컬러 (CSS 변수 연결)
        primary: {
          DEFAULT: 'hsl(var(--primary))',
          foreground: 'hsl(var(--primary-foreground))',
        },
        secondary: {
          DEFAULT: 'hsl(var(--secondary))',
          foreground: 'hsl(var(--secondary-foreground))',
        },
        accent: {
          DEFAULT: 'hsl(var(--accent))',
          foreground: 'hsl(var(--accent-foreground))',
        },
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        muted: {
          DEFAULT: 'hsl(var(--muted))',
          foreground: 'hsl(var(--muted-foreground))',
        },
        border: 'hsl(var(--border))',
      },
    },
  },
  plugins: [],
};

export default config;
```

### globals.css
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    /* colors.json에서 가져온 값 */
    --primary: 258 90% 66%;           /* #8B5CF6 */
    --primary-foreground: 0 0% 100%;  /* white */
    
    --secondary: 210 40% 96%;         /* #F1F5F9 */
    --secondary-foreground: 222 47% 11%; /* dark */
    
    --accent: 347 77% 60%;            /* #F43F5E */
    --accent-foreground: 0 0% 100%;   /* white */
    
    --background: 0 0% 100%;
    --foreground: 222 47% 11%;
    
    --muted: 215 16% 47%;
    --muted-foreground: 215 16% 47%;
    
    --border: 214 32% 91%;
  }
  
  .dark {
    --primary: 258 90% 66%;
    --primary-foreground: 0 0% 100%;
    
    --secondary: 217 33% 17%;
    --secondary-foreground: 210 40% 98%;
    
    --background: 222 47% 11%;
    --foreground: 210 40% 98%;
  }
}
```

---

## 사용 예시

```tsx
// 올바른 사용
<button className="bg-primary text-primary-foreground">
  예약하기
</button>

<div className="bg-secondary p-4 rounded-lg">
  <p className="text-foreground">카드 내용</p>
</div>

<span className="bg-accent text-accent-foreground px-2 py-1 rounded">
  특가
</span>

// ❌ 금지: 하드코딩 색상
<button className="bg-[#8B5CF6]">
<button className="bg-purple-500">
```

---

## 체크리스트

- [ ] colors.json 파일 생성됨
- [ ] 3색 이하로 구성됨
- [ ] Primary/Secondary/Accent 역할 정의됨
- [ ] HSL 형식으로 변환됨
- [ ] tailwind.config.ts에 반영됨
- [ ] globals.css에 CSS 변수 정의됨
- [ ] 다크 모드 대응 (선택)
