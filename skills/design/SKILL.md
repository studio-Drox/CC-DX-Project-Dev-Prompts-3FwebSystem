# 디자인 스킬

## 개요

3F Web의 디자인 원칙과 가이드라인을 정의합니다.
모든 프로젝트는 이 가이드라인을 따라 일관된 디자인 품질을 유지합니다.

---

## 핵심 원칙

### 1. 한국 트렌드 기반
- 현재 시점 대한민국 웹 디자인 트렌드 반영
- 업종별 경쟁사 분석 기반 설계
- @skills/design/korea-trends.md 참조

### 2. 3색 테마 시스템
- Primary / Secondary / Accent
- 최대 3색으로 제한
- @skills/design/color-system.md 참조

### 3. 인터랙션 & 애니메이션
- String Tune 활용 (웹 전용)
- PWA에서는 비활성화
- @skills/design/string-tune.md 참조

---

## 디자인 토큰

### 간격 (Spacing)
```
4px  → space-1  (최소 간격)
8px  → space-2
12px → space-3
16px → space-4  (기본 간격)
24px → space-6
32px → space-8
48px → space-12
64px → space-16
96px → space-24
```

### 둥글기 (Border Radius)
```
4px  → rounded-sm
8px  → rounded     (기본)
12px → rounded-md
16px → rounded-lg
24px → rounded-xl
9999px → rounded-full
```

### 그림자 (Shadow)
```
shadow-sm   → 미묘한 구분
shadow      → 일반 카드
shadow-md   → 호버 상태
shadow-lg   → 모달/드롭다운
shadow-xl   → 플로팅 요소
```

---

## 레이아웃 시스템

### 컨테이너
```tsx
// 최대 너비 + 센터 정렬
<div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
  {children}
</div>

// 풀 너비 섹션 내 컨테이너
<section className="w-full bg-primary">
  <div className="max-w-7xl mx-auto px-4 py-16">
    {children}
  </div>
</section>
```

### 그리드
```tsx
// 반응형 그리드
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
  {items.map(item => <Card key={item.id} />)}
</div>

// 비대칭 그리드
<div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
  <div className="lg:col-span-2">메인 콘텐츠</div>
  <div>사이드바</div>
</div>
```

---

## 반응형 브레이크포인트

| 이름 | 최소 너비 | 용도 |
|------|-----------|------|
| sm | 640px | 작은 태블릿 |
| md | 768px | 태블릿 |
| lg | 1024px | 작은 데스크탑 |
| xl | 1280px | 데스크탑 |
| 2xl | 1536px | 큰 데스크탑 |

### Mobile First 작성
```tsx
// ❌ 데스크탑 먼저
<div className="flex-row md:flex-col">

// ✅ 모바일 먼저
<div className="flex-col md:flex-row">
```

---

## 다크 모드 (선택)

### CSS 변수 기반
```css
:root {
  --background: 0 0% 100%;
  --foreground: 222.2 84% 4.9%;
}

.dark {
  --background: 222.2 84% 4.9%;
  --foreground: 210 40% 98%;
}
```

### 토글 구현
```tsx
'use client';

import { useTheme } from 'next-themes';
import { Moon, Sun } from 'lucide-react';

export function ThemeToggle() {
  const { theme, setTheme } = useTheme();
  
  return (
    <Button
      variant="ghost"
      size="icon"
      onClick={() => setTheme(theme === 'dark' ? 'light' : 'dark')}
    >
      <Sun className="h-5 w-5 rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0" />
      <Moon className="absolute h-5 w-5 rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100" />
    </Button>
  );
}
```

---

## 참조 스킬

- 한국 트렌드: @skills/design/korea-trends.md
- 컬러 시스템: @skills/design/color-system.md
- 인터랙션: @skills/design/string-tune.md
