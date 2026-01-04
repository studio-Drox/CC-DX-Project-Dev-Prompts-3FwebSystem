# PAGE_SPEC 운영 규칙

## 개요

PAGE_SPEC은 페이지 생성의 **필수 입력 사양**입니다.
모든 `/gen-page` 커맨드는 PAGE_SPEC Front-matter를 요구합니다.

---

## PAGE_SPEC 표준 템플릿

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

### 필수 항목
| 항목 | 설명 | 예시 |
|------|------|------|
| category | 업종 카테고리 | `clinic`, `realestate`, `travel` |
| page | 페이지 유형 | `home`, `landing`, `detail` |
| language | 언어 | `ko`, `vi`, `en`, `ko+vi` |
| goal | 페이지 목적 | `lead-gen`, `info`, `booking` |

### 선택 항목
| 항목 | 설명 | 예시 |
|------|------|------|
| subcategory | 세부 업종 | `dental`, `plastic`, `derma` |
| tone | 디자인 톤 | `clean`, `premium`, `friendly` |

---

## Front-matter 강제 규칙

### PAGE_SPEC 미제공 시 동작

1. **페이지 생성을 진행하지 않는다**
2. PAGE_SPEC 템플릿을 출력한다
3. 사용자에게 먼저 작성하도록 요청한다

```
⚠️ PAGE_SPEC이 제공되지 않았습니다.

아래 템플릿을 작성해주세요:

[PAGE_SPEC]
category:
page:
language:
goal:
[/PAGE_SPEC]
```

### 값 해석 규칙

- `category` / `subcategory` 값은 **임의 해석 없이 그대로 사용**한다
- 정의되지 않은 값이 입력되면 확인 질문을 한다
- 누락된 필수 항목이 있으면 생성을 거부한다

---

## 카테고리 추측 금지 규칙

> **Claude는 업종(category)을 절대 추측하지 않는다**

### 금지 행위

- PAGE_SPEC에 명시되지 않은 업종 추측
- 임의로 목적(goal) 보완
- 톤(tone) 가정

### 허용 행위

- 필요한 경우 **질문으로 되돌린다**
- 명시된 값만 사용한다

```
❌ "clinic이니까 의료 관련이겠군요"
✅ "subcategory가 지정되지 않았습니다. dental, plastic, derma 중 선택해주세요."
```

---

## Front-matter와 Skill의 역할 분리

| 구분 | 역할 | 담당 |
|------|------|------|
| PAGE_SPEC | **무엇을 만들지** 정의 | 사용자 입력 |
| Skill | **어떻게 만들지** 정의 | 시스템 규칙 |

### 원칙

- Front-matter 내용을 Skill로 이동하지 않는다
- Skill 내용을 Front-matter에 혼합하지 않는다
- 각각의 역할을 명확히 분리한다

---

## 기본 로딩 Skill 규칙

### /gen-page 실행 시 자동 적용

```
@skills/core/SKILL.md           # 핵심 원칙
@skills/core/component-first.md # 컴포넌트 퍼스트
@skills/design/SKILL.md         # 디자인 가이드
@skills/design/color-system.md  # 3색 테마
@skills/fonts/SKILL.md          # 폰트 시스템
```

### 기본 적용 항목

- layout / typography / color / motion 관련 규칙
- fonts / accessibility / responsive 규칙

### ⚠️ 특수 연출은 자동 적용 금지

---

## 특수 케이스(옵션) 스킬 규칙

### 명시적 호출 필요 항목

| 스킬 | 용도 |
|------|------|
| `special/before-after` | 시술 전·후 비교, 리뉴얼 비교, 마케팅 연출 컷 |

### 사용 방법

```
@skill special/before-after
```

### 호출 없는 경우

- 관련 레이아웃을 생성하지 않는다
- 관련 카피를 작성하지 않는다
- 관련 섹션을 추가하지 않는다

---

## 디자인 품질 기준 (AI Slop 방지)

### 금지 패턴

| 패턴 | 문제점 |
|------|--------|
| 무근거한 Inter / Roboto 고정 | 프로젝트 font.md 무시 |
| 의미 없는 보라색 그라데이션 | 브랜드 컬러 무시 |
| 3카드 랜딩 구조 반복 | 업종 특성 무시 |
| 기본 히어로 + 특징 + CTA 패턴 | 차별화 없음 |

### 디자인 판단 우선순위

```
1. PAGE_SPEC (최우선)
   ↓
2. frontend-design skill
   ↓
3. 업종(category) 맥락
   ↓
4. 글로벌·한국 기준 UX 관행
```

### 원칙

- 기본적이고 흔한 UI 패턴에 자동 수렴하지 않는다
- 업종 특성을 반영한 차별화된 디자인을 추구한다
- colors.json, font.md 등 프로젝트 설정을 우선한다

---

## 출력 규칙 요약

### 생성 범위

- PAGE_SPEC에 정의된 범위**만** 생성한다
- 정의되지 않은 기능, 섹션, 연출은 추가하지 않는다

### 우선 순위

- 항상 **실사용 가능한 페이지 구조**를 우선한다
- 데모용 더미보다 실제 운영 가능한 구조를 목표로 한다

---

## 운영 원칙 요약

```
✅ PAGE_SPEC 없으면 생성하지 말 것
✅ 추측하지 말 것
✅ 특수 연출은 호출 없으면 쓰지 말 것
```

---

## 사용 예시

### 올바른 사용

```
[PAGE_SPEC]
category: clinic
subcategory: dental
page: home
language: ko
goal: booking
tone: clean
[/PAGE_SPEC]

위 사양으로 치과 홈페이지를 만들어주세요.
```

### 잘못된 사용 (거부됨)

```
치과 홈페이지 만들어주세요.
```

→ PAGE_SPEC 템플릿 출력 후 작성 요청

---

## 참조

- 핵심 스킬: @skills/core/SKILL.md
- 디자인 스킬: @skills/design/SKILL.md
- 카테고리: @skills/categories/SKILL.md
