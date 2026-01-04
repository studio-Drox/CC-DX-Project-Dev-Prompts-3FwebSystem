# 카테고리 스킬

## 개요

업종별 웹사이트 템플릿 카테고리를 정의합니다.
각 카테고리는 해당 업종에 최적화된 UI/UX, 컬러, 기능을 제공합니다.

---

## 지원 카테고리

### medical (의료)
- **aesthetic**: 피부과, 성형외과, 에스테틱
- **family-med**: 가정의학과, 내과
- **dental**: 치과
- **oriental**: 한의원

### fnb (요식업)
- **cafe**: 카페, 디저트
- **restaurant**: 레스토랑, 식당
- **bar**: 바, 펍

### beauty (뷰티)
- **hair-salon**: 헤어샵, 미용실
- **nail**: 네일샵
- **spa**: 스파, 마사지

### fitness (피트니스)
- **gym**: 헬스장, PT
- **yoga**: 요가, 필라테스
- **pilates**: 필라테스 전문

### education (교육)
- **academy**: 학원, 교습소
- **kindergarten**: 유치원, 어린이집
- **tutoring**: 과외, 입시

### professional (전문직)
- **law**: 법률사무소, 변호사
- **accounting**: 회계사, 세무사
- **consulting**: 컨설팅

---

## 카테고리별 공통 요소

### 필수 페이지
| 카테고리 | 필수 페이지 |
|----------|-------------|
| medical | home, about, services, booking, contact |
| fnb | home, menu, about, contact, location |
| beauty | home, services, gallery, booking, contact |
| fitness | home, programs, trainers, pricing, contact |
| education | home, courses, teachers, schedule, contact |
| professional | home, services, team, cases, contact |

### 필수 컴포넌트
- Header (로고, 네비게이션, CTA)
- Hero Section
- Service/Product Grid
- Contact/Booking Form
- Footer

### 카테고리별 특화 컴포넌트
| 카테고리 | 특화 컴포넌트 |
|----------|---------------|
| medical | BeforeAfter Slider, Doctor Profile, Treatment FAQ |
| fnb | Menu Card, Instagram Feed, Map |
| beauty | Gallery Grid, Booking Calendar, Price Table |
| fitness | Program Card, Trainer Profile, Schedule Table |
| education | Course Card, Curriculum Timeline, Teacher Bio |
| professional | Case Study, Team Grid, Service Accordion |

---

## 카테고리 설정 파일

각 카테고리 폴더에 다음 파일 필요:

```
skills/categories/[category]/
├── SKILL.md          # 카테고리 상세 가이드
├── config.json       # 기본 설정
└── examples/         # 예시 코드
```

### config.json 예시

```json
{
  "category": "medical",
  "subcategory": "aesthetic",
  "name": "피부과/에스테틱",
  
  "colors": {
    "primary": "#8B5CF6",
    "secondary": "#F1F5F9",
    "accent": "#EC4899"
  },
  
  "fonts": {
    "heading": "Pretendard",
    "body": "Pretendard",
    "accent": "Gmarket Sans"
  },
  
  "pages": [
    "home",
    "about",
    "services",
    "gallery",
    "booking",
    "contact"
  ],
  
  "features": {
    "booking": true,
    "gallery": true,
    "beforeAfter": true,
    "faq": true,
    "blog": false
  },
  
  "seo": {
    "keywords": ["피부과", "피부 시술", "강남 피부과"],
    "schema": "MedicalBusiness"
  }
}
```

---

## 새 카테고리 추가

### 1. 폴더 생성
```bash
mkdir -p skills/categories/[new-category]
```

### 2. SKILL.md 작성
- 업종 특성 분석
- 필수 페이지/컴포넌트 정의
- 컬러/폰트 권장사항
- SEO 키워드

### 3. config.json 작성
- 기본 설정값 정의

### 4. 템플릿 생성
```bash
mkdir -p templates/[new-category]
```

---

## 참조

- 의료: @skills/categories/medical/SKILL.md
- 요식업: @skills/categories/fnb/SKILL.md
- 뷰티: @skills/categories/beauty/SKILL.md
- 피트니스: @skills/categories/fitness/SKILL.md
