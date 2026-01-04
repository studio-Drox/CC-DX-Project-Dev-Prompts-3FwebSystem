# 배포 아키텍처

## 무료 고정비용 구성

```
┌─────────────────────────────────────────────────────────────────┐
│                        사용자 요청                               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                     Cloudflare (CDN/DNS)                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │   DNS 관리   │  │  SSL/TLS   │  │  캐싱/CDN   │              │
│  └─────────────┘  └─────────────┘  └─────────────┘              │
│                         Free Plan                                │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      Vercel (호스팅)                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │  정적 배포   │  │ Serverless │  │  Edge 함수  │              │
│  └─────────────┘  └─────────────┘  └─────────────┘              │
│                        Hobby Plan                                │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                       GitHub (소스)                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │   저장소    │  │   Actions   │  │  PR Preview │              │
│  └─────────────┘  └─────────────┘  └─────────────┘              │
│                         Free Plan                                │
└─────────────────────────────────────────────────────────────────┘
```

---

## 무료 티어 제한사항

### GitHub Free
| 항목 | 제한 |
|------|------|
| 저장소 | 무제한 (Public/Private) |
| Actions | 2,000분/월 |
| Packages | 500MB |
| LFS | 1GB |

### Vercel Hobby
| 항목 | 제한 |
|------|------|
| Bandwidth | 100GB/월 |
| Build 시간 | 6,000분/월 |
| Serverless | 100GB-Hours |
| Edge | 500,000 invocations |
| 팀 멤버 | 1명 |

### Cloudflare Free
| 항목 | 제한 |
|------|------|
| 요청 수 | 무제한 |
| Bandwidth | 무제한 |
| SSL | 포함 |
| DDoS 보호 | 포함 |
| Page Rules | 3개 |

---

## 워크플로우

### 개발 → 배포 흐름

```
1. 로컬 개발
   └── pnpm dev

2. 커밋 & 푸시
   └── git push origin main

3. Vercel 자동 빌드
   └── GitHub 연동으로 자동 트리거

4. Preview 배포 (PR 시)
   └── 각 PR마다 고유 URL 생성

5. Production 배포 (main 머지 시)
   └── 메인 도메인에 자동 배포

6. Cloudflare 캐시 업데이트
   └── 자동 캐시 무효화
```

---

## 환경 변수 관리

### 로컬 (.env.local)
```env
# 개발용 환경 변수
NEXT_PUBLIC_SITE_URL=http://localhost:3000
NEXT_PUBLIC_GA_ID=
```

### Vercel 환경 변수
```bash
# CLI로 추가
vercel env add NEXT_PUBLIC_SITE_URL production
vercel env add NEXT_PUBLIC_GA_ID production
```

### 환경별 구분
- `Development`: 로컬 개발
- `Preview`: PR Preview 배포
- `Production`: 프로덕션 배포

---

## 도메인 설정

### Cloudflare DNS → Vercel

```
# Root 도메인
Type: CNAME
Name: @
Content: cname.vercel-dns.com
Proxy: ✅ Proxied

# www 서브도메인
Type: CNAME
Name: www
Content: cname.vercel-dns.com
Proxy: ✅ Proxied
```

### Vercel 도메인 설정

```bash
# 도메인 추가
vercel domains add example.com

# www → root 리다이렉트 설정
# vercel.json에서 설정
```

---

## 보안 설정

### Cloudflare
- SSL Mode: Full (strict)
- Always Use HTTPS: On
- Minimum TLS: 1.2
- HSTS: Enable

### Vercel (vercel.json)
```json
{
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "X-Frame-Options", "value": "DENY" },
        { "key": "X-XSS-Protection", "value": "1; mode=block" },
        { "key": "Referrer-Policy", "value": "strict-origin-when-cross-origin" }
      ]
    }
  ]
}
```

---

## 모니터링

### Vercel Analytics (무료)
- Web Vitals 모니터링
- 실시간 트래픽

### Cloudflare Analytics (무료)
- 요청/대역폭 통계
- 보안 이벤트
- 캐시 히트율

---

## 비용 최적화 팁

1. **이미지 최적화**
   - next/image 필수
   - WebP 자동 변환
   - CDN 캐싱 활용

2. **정적 생성 우선**
   - 가능하면 SSG 사용
   - ISR로 재검증

3. **Edge 함수 최소화**
   - 필요한 경우에만 사용
   - Serverless로 대체 가능하면 대체

4. **캐싱 전략**
   - 정적 에셋 장기 캐싱
   - API 응답 적절한 캐싱
