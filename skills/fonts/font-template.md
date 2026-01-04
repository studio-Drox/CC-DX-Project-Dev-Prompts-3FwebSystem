# [프로젝트명] 폰트 가이드

> 이 파일은 프로젝트의 타이포그래피 규칙을 정의합니다.
> 모든 텍스트 스타일은 이 가이드를 따릅니다.

---

## 폰트 패밀리

### 제목용 (Heading)
- **이름**: Pretendard
- **굵기**: 700 (Bold), 600 (SemiBold)
- **용도**: h1~h3, 네비게이션, CTA 버튼
- **특징**: 깔끔하고 현대적인 느낌

### 본문용 (Body)
- **이름**: Pretendard
- **굵기**: 400 (Regular), 500 (Medium)
- **용도**: p, span, 일반 텍스트, 설명
- **특징**: 가독성 우수

### 포인트용 (Accent) - 선택
- **이름**: Gmarket Sans
- **굵기**: 700 (Bold)
- **용도**: 가격, 할인율, 특별 배지, 숫자 강조
- **특징**: 굵고 임팩트 있는 느낌

---

## 크기 시스템

### 제목 (Heading)

| 용도 | Desktop | Mobile | Tailwind Class |
|------|---------|--------|----------------|
| Display 1 | 64px / 4rem | 40px / 2.5rem | `text-display-1` |
| Display 2 | 48px / 3rem | 32px / 2rem | `text-display-2` |
| H1 | 36px / 2.25rem | 28px / 1.75rem | `text-heading-1` |
| H2 | 30px / 1.875rem | 24px / 1.5rem | `text-heading-2` |
| H3 | 24px / 1.5rem | 20px / 1.25rem | `text-heading-3` |

### 본문 (Body)

| 용도 | Desktop | Mobile | Tailwind Class |
|------|---------|--------|----------------|
| Large | 18px / 1.125rem | 16px / 1rem | `text-body-lg` |
| Base | 16px / 1rem | 15px / 0.9375rem | `text-body` |
| Small | 14px / 0.875rem | 13px / 0.8125rem | `text-body-sm` |
| Caption | 12px / 0.75rem | 11px / 0.6875rem | `text-caption` |

---

## 행간 (Line Height)

| 용도 | 값 | Tailwind |
|------|-----|----------|
| 제목 | 1.2 ~ 1.3 | `leading-tight` |
| 본문 | 1.6 ~ 1.8 | `leading-relaxed` |
| 캡션 | 1.4 ~ 1.5 | `leading-normal` |

### 한글 권장 행간
- 본문: **1.7** (가독성 최적)
- 제목: **1.3** (응집력)

---

## 자간 (Letter Spacing)

| 용도 | 값 | Tailwind |
|------|-----|----------|
| 한글 제목 | -0.02em | `tracking-korean` |
| 한글 본문 | -0.01em | `tracking-tight` |
| 영문 | 0 ~ 0.01em | `tracking-normal` |

---

## 굵기 (Font Weight)

| 용도 | 값 | Tailwind |
|------|-----|----------|
| 제목 | 700 | `font-bold` |
| 강조 | 600 | `font-semibold` |
| 본문 | 400 | `font-normal` |
| 캡션 | 400 | `font-normal` |

---

## 용도별 스타일 가이드

### 히어로 타이틀
```
폰트: Heading
크기: Display 1 (64px → 40px)
굵기: Bold (700)
행간: 1.1
자간: -0.02em
```

### 섹션 타이틀
```
폰트: Heading
크기: H1 (36px → 28px)
굵기: Bold (700)
행간: 1.3
자간: -0.02em
```

### 카드 타이틀
```
폰트: Heading
크기: H3 (24px → 20px)
굵기: SemiBold (600)
행간: 1.3
```

### 본문 텍스트
```
폰트: Body
크기: Base (16px → 15px)
굵기: Regular (400)
행간: 1.7
자간: -0.01em
```

### 가격 표시
```
폰트: Accent (Gmarket Sans)
크기: H2 (30px → 24px)
굵기: Bold (700)
자간: -0.02em
```

### 버튼 텍스트
```
폰트: Heading
크기: Base (16px)
굵기: SemiBold (600)
자간: 0
```

### 네비게이션
```
폰트: Body
크기: Base (16px)
굵기: Medium (500)
```

### 메타 정보 (날짜, 태그 등)
```
폰트: Body
크기: Caption (12px)
굵기: Regular (400)
색상: Muted
```

---

## Tailwind 반영 코드

### tailwind.config.ts
```typescript
fontSize: {
  'display-1': ['4rem', { lineHeight: '1.1', letterSpacing: '-0.02em' }],
  'display-2': ['3rem', { lineHeight: '1.2', letterSpacing: '-0.02em' }],
  'heading-1': ['2.25rem', { lineHeight: '1.3', letterSpacing: '-0.02em' }],
  'heading-2': ['1.875rem', { lineHeight: '1.3', letterSpacing: '-0.01em' }],
  'heading-3': ['1.5rem', { lineHeight: '1.4' }],
  'body-lg': ['1.125rem', { lineHeight: '1.7' }],
  'body': ['1rem', { lineHeight: '1.7' }],
  'body-sm': ['0.875rem', { lineHeight: '1.6' }],
  'caption': ['0.75rem', { lineHeight: '1.5' }],
},
fontFamily: {
  sans: ['var(--font-pretendard)', 'sans-serif'],
  heading: ['var(--font-pretendard)', 'sans-serif'],
  accent: ['var(--font-gmarket)', 'sans-serif'],
},
```

---

## 사용 예시

```tsx
// 히어로 타이틀
<h1 className="font-heading text-display-1 md:text-display-1 text-[40px] font-bold tracking-korean">
  아름다움을 디자인합니다
</h1>

// 섹션 타이틀
<h2 className="font-heading text-heading-1 font-bold tracking-korean">
  대표 시술 안내
</h2>

// 본문
<p className="font-sans text-body leading-relaxed text-muted-foreground">
  전문의가 1:1 맞춤 상담을 진행합니다.
</p>

// 가격
<span className="font-accent text-heading-2 font-bold text-primary">
  ₩1,500,000
</span>

// 버튼
<button className="font-heading text-body font-semibold">
  예약하기
</button>
```
