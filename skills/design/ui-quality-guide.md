# UI 품질 가이드 - AI Slop 방지 원칙

> **버전**: 1.0  
> **최종 수정**: 2025-01-14  
> **위치**: `skills/design/ui-quality.md`

---

## 개요

이 문서는 3F Web 템플릿이 "AI가 만든 것 같은" 저품질 느낌을 피하고, 신뢰감 있는 프로페셔널 UI를 구현하기 위한 디자인 원칙입니다.

---

## 핵심 원칙 4가지

### 1. 둥근 모서리 통일 (Consistent Rounded Corners)

**규칙**
- 모든 카드, 버튼, 컨테이너에 일관된 border-radius 적용
- 최소 `rounded-xl` (12px) 이상 권장
- 프로젝트 내 통일성 유지

**적용 예시**
```tsx
// ✅ 권장
<Card className="rounded-2xl">
<Button className="rounded-xl">
<div className="rounded-2xl overflow-hidden">

// ❌ 금지
<Card className="rounded">        // 너무 작음
<Card className="rounded-sm">     // 일관성 없음
<Card>                            // 기본값 사용 금지
```

**Tailwind 클래스 기준**
| 클래스 | 값 | 용도 |
|--------|-----|------|
| `rounded-lg` | 8px | 작은 요소 (태그, 뱃지) |
| `rounded-xl` | 12px | 버튼, 입력 필드 |
| `rounded-2xl` | 16px | 카드, 섹션 (기본 권장) |
| `rounded-3xl` | 24px | 히어로 이미지, 대형 카드 |
| `rounded-full` | 9999px | 아바타, 원형 버튼 |

---

### 2. 미세한 호버 마이크로 인터랙션 (Subtle Hover Interactions)

**규칙**
- 모든 클릭 가능 요소에 호버 효과 적용
- 과하지 않게, 미세하게 (subtle)
- transform + shadow 조합 권장

**적용 예시**
```tsx
// ✅ 권장: 카드 호버
<Card className="
  transition-all duration-300 ease-out
  hover:translate-y-[-4px] 
  hover:shadow-lg
">

// ✅ 권장: 버튼 호버
<Button className="
  transition-all duration-200
  hover:scale-[1.02]
  hover:shadow-md
">

// ✅ 권장: 링크/텍스트 호버
<a className="
  transition-colors duration-200
  hover:text-primary
">
```

**금지 사항**
```tsx
// ❌ 금지: 과한 효과
hover:translate-y-[-20px]    // 너무 큰 이동
hover:scale-150              // 너무 큰 확대
hover:rotate-12              // 불필요한 회전

// ❌ 금지: 효과 없음
// 클릭 가능 요소에 호버 효과 미적용
```

**표준 호버 프리셋**
```css
/* 카드 호버 */
.card-hover {
  @apply transition-all duration-300 ease-out;
  @apply hover:translate-y-[-4px] hover:shadow-lg;
}

/* 버튼 호버 */
.btn-hover {
  @apply transition-all duration-200;
  @apply hover:scale-[1.02] hover:shadow-md;
}

/* 이미지 호버 */
.img-hover {
  @apply transition-transform duration-300;
  @apply hover:scale-105;
}
```

---

### 3. 넉넉한 여백 활용 (Generous Whitespace)

**규칙**
- 여백은 항상 넉넉하게 (cramped 느낌 방지)
- 섹션 간 간격 통일
- 콘텐츠 밀도보다 가독성 우선

**섹션 간격 기준**
```tsx
// ✅ 권장: 섹션 간격
<section className="py-20 md:py-32">  // 기본 섹션
<section className="py-24 md:py-40">  // 히어로/CTA 섹션

// ❌ 금지
<section className="py-4">   // 너무 좁음
<section className="py-8">   // 빈약함
```

**컨테이너 패딩 기준**
```tsx
// ✅ 권장
<div className="px-6 md:px-8 lg:px-12">  // 컨테이너
<Card className="p-6 md:p-8">            // 카드 내부

// ❌ 금지
<div className="px-2">       // 너무 좁음
<Card className="p-2">       // 답답함
```

**요소 간 간격 기준**
| 관계 | 간격 | Tailwind |
|------|------|----------|
| 관련 요소 (제목-설명) | 8-12px | `space-y-2`, `space-y-3` |
| 독립 요소 (카드-카드) | 16-24px | `gap-4`, `gap-6` |
| 섹션 내 그룹 | 32-48px | `space-y-8`, `space-y-12` |
| 섹션 간 | 80-128px | `py-20`, `py-32` |

---

### 4. 타이포그래피 강조 (Typography Emphasis)

**규칙**
- 히어로 헤드라인은 크게, 임팩트 있게
- 자간(letter-spacing) 조절로 세련됨 추가
- 폰트 굵기 대비로 계층 구조 명확히

**히어로 타이포 기준**
```tsx
// ✅ 권장: 히어로 헤드라인
<h1 className="
  text-4xl md:text-5xl lg:text-6xl
  font-bold
  tracking-tight
  leading-[1.1]
">

// ✅ 권장: 섹션 헤드라인
<h2 className="
  text-3xl md:text-4xl
  font-bold
  tracking-tight
">

// ✅ 권장: 서브헤드라인
<p className="
  text-lg md:text-xl
  text-muted-foreground
  leading-relaxed
">
```

**자간 (Letter Spacing) 가이드**
| 용도 | Tailwind | 효과 |
|------|----------|------|
| 대형 헤드라인 | `tracking-tight` | 응집력, 임팩트 |
| 본문 | `tracking-normal` | 가독성 |
| 작은 캡션/라벨 | `tracking-wide` | 가독성 향상 |
| 버튼 텍스트 | `tracking-wide` | 명확함 |

**폰트 굵기 계층**
```tsx
// 계층 구조 예시
<h1 className="font-bold">      // 700 - 메인 헤드라인
<h2 className="font-semibold">  // 600 - 섹션 헤드라인
<h3 className="font-medium">    // 500 - 서브헤드라인
<p className="font-normal">     // 400 - 본문
<span className="font-light">   // 300 - 보조 텍스트 (주의해서 사용)
```

---

## AI Slop 방지 체크리스트

### ❌ 금지 패턴

| 패턴 | 문제점 | 대안 |
|------|--------|------|
| 그라디언트 히어로 배경 | AI 생성 느낌, 식상함 | 단색 또는 이미지 배경 |
| 보라-핑크 그라디언트 | v0/Framer 기본 스타일 | 브랜드 컬러 사용 |
| 무의미한 blur 원형 배경 | 클리셰, 산만함 | 깔끔한 단색 배경 |
| 3카드 균등 배치 반복 | 자동 수렴 패턴 | 비대칭 또는 벤토 그리드 |
| Inter/Roboto 무조건 사용 | 개성 없음 | 업종별 폰트 선정 |

### ✅ 권장 패턴

| 패턴 | 효과 |
|------|------|
| 단색 배경 + 강한 타이포 | 세련됨, 집중도 |
| 실제 사진/이미지 활용 | 신뢰감, 현실감 |
| 여백으로 고급스러움 연출 | 프리미엄 느낌 |
| 미세한 인터랙션 | 반응성, 디테일 |
| 일관된 둥근 모서리 | 현대적, 친근함 |

---

## 업종별 적용 가이드

### 병의원 (medical)
- 둥근 모서리: `rounded-2xl` 기본
- 색상: 블루/그린 계열 (신뢰, 청결)
- 여백: 넉넉하게 (답답함 금지)
- 톤: 깔끔하고 신뢰감 있게

### 법률/세무 (professional)
- 둥근 모서리: `rounded-xl` (살짝 보수적)
- 색상: 다크 네이비, 골드 포인트
- 타이포: 강한 대비, 권위감
- 톤: 전문적, 신뢰

### 카페/레스토랑 (fnb)
- 둥근 모서리: `rounded-2xl` ~ `rounded-3xl`
- 색상: 따뜻한 톤 (베이지, 브라운, 테라코타)
- 이미지: 실제 음식/공간 사진 필수
- 톤: 따뜻하고 감성적

### 교육/학원 (education)
- 둥근 모서리: `rounded-xl` ~ `rounded-2xl`
- 색상: 밝고 활기찬 톤 (오렌지, 그린, 블루)
- 타이포: 명확하고 가독성 높게
- 톤: 신뢰 + 활기

---

## 검증 체크리스트

코드 리뷰 시 아래 항목 확인:

```
□ 모든 카드/버튼에 rounded-xl 이상 적용되었는가?
□ 클릭 가능 요소에 호버 효과가 있는가?
□ 섹션 간격이 py-20 이상인가?
□ 히어로 헤드라인이 text-4xl 이상인가?
□ 그라디언트 히어로 배경을 사용하지 않았는가?
□ 여백이 충분히 넉넉한가?
□ 타이포그래피 계층이 명확한가?
```

---

## Hooks 연동

`hooks.json`에 아래 검증 로직 추가 권장:

```json
{
  "event": "PreToolUse",
  "script": "ui-quality-check.sh",
  "rules": [
    "그라디언트 히어로 배경 사용 시 경고",
    "rounded-sm, rounded 사용 시 rounded-xl 이상 권장",
    "py-4, py-8 섹션 간격 사용 시 py-16 이상 권장",
    "호버 효과 없는 버튼/카드 감지 시 경고"
  ]
}
```

---

## 참고 자료

- [Google Gemini Visual Design](https://design.google/library/gemini-ai-visual-design) - 참고하되 그라디언트는 제외
- 3F Web 디자인 시스템: `@skills/design/SKILL.md`
- 한국 UI/UX 트렌드: `@skills/design/korea-trends.md`

---

**Studio DROX**  
*Driven Relationships, Open eXchange*
