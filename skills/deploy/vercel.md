# Vercel 배포 가이드

## ⚠️ 주의

> **배포 전 반드시 `/check-docs vercel` 실행하여 최신 문서 확인!**

---

## 초기 설정

### 1. Vercel CLI 설치

```bash
# 전역 설치
pnpm add -g vercel

# 로그인
vercel login
```

### 2. 프로젝트 연결

```bash
# 프로젝트 디렉토리에서 실행
vercel

# 질문에 답변
? Set up and deploy? Yes
? Which scope? [your-account]
? Link to existing project? No
? What's your project's name? [project-name]
? In which directory is your code located? ./
```

---

## vercel.json 설정

```json
{
  "framework": "nextjs",
  "regions": ["icn1"],
  
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "X-Content-Type-Options",
          "value": "nosniff"
        },
        {
          "key": "X-Frame-Options",
          "value": "DENY"
        },
        {
          "key": "X-XSS-Protection",
          "value": "1; mode=block"
        },
        {
          "key": "Referrer-Policy",
          "value": "strict-origin-when-cross-origin"
        }
      ]
    },
    {
      "source": "/fonts/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    },
    {
      "source": "/_next/static/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    }
  ],
  
  "rewrites": [
    {
      "source": "/sitemap.xml",
      "destination": "/api/sitemap"
    }
  ]
}
```

---

## 환경 변수

### 설정 방법

```bash
# CLI로 추가
vercel env add NEXT_PUBLIC_SITE_URL production
vercel env add NEXT_PUBLIC_GA_ID production

# 값 입력 후 Enter
```

### 환경 구분

| 환경 | 용도 | 브랜치 |
|------|------|--------|
| Production | 프로덕션 | main |
| Preview | PR 미리보기 | PR 브랜치 |
| Development | 로컬 개발 | - |

### 권장 환경 변수

```env
# 필수
NEXT_PUBLIC_SITE_URL=https://example.com

# 분석 (선택)
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX

# API (필요시)
API_SECRET_KEY=xxx
```

---

## 빌드 설정

### next.config.js

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  // 이미지 도메인 허용
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'source.unsplash.com',
      },
      {
        protocol: 'https',
        hostname: 'images.unsplash.com',
      },
    ],
  },
  
  // 실험적 기능 (필요시)
  experimental: {
    // optimizeCss: true,
  },
};

module.exports = nextConfig;
```

---

## 배포 명령어

```bash
# 미리보기 배포 (Preview)
vercel

# 프로덕션 배포
vercel --prod

# 특정 환경으로 배포
vercel --env production
```

---

## 도메인 설정

### 도메인 추가

```bash
# 도메인 추가
vercel domains add example.com

# www 리다이렉트 설정
vercel domains add www.example.com
```

### Vercel Dashboard에서 설정
1. Project Settings → Domains
2. Add Domain
3. DNS 설정 안내 따라 진행

---

## 무료 티어 제한

| 항목 | 제한 | 비고 |
|------|------|------|
| Bandwidth | 100GB/월 | 초과 시 과금 |
| Build 시간 | 6,000분/월 | |
| Serverless 실행 | 100GB-Hours | |
| Edge 함수 | 500K invocations | |
| 팀 멤버 | 1명 | Hobby |
| 프로젝트 | 무제한 | |

---

## 성능 최적화

### 권장 설정

1. **Region 최적화**
   - `icn1` (서울) 선택
   
2. **캐싱 헤더**
   - 정적 에셋 장기 캐싱
   
3. **이미지 최적화**
   - next/image 사용
   - WebP 자동 변환

### 빌드 최적화

```bash
# 빌드 캐시 활용
# Vercel이 자동으로 처리

# 번들 분석 (로컬)
ANALYZE=true pnpm build
```

---

## 트러블슈팅

### 빌드 실패

```bash
# 로컬에서 빌드 테스트
pnpm build

# 타입 체크
pnpm tsc --noEmit

# 린트 체크
pnpm lint
```

### 환경 변수 문제

```bash
# 환경 변수 확인
vercel env ls

# 환경 변수 삭제 후 재설정
vercel env rm VARIABLE_NAME production
vercel env add VARIABLE_NAME production
```

### 캐시 문제

```bash
# Vercel 캐시 무효화
# Dashboard → Deployments → Redeploy → Clear Cache
```
