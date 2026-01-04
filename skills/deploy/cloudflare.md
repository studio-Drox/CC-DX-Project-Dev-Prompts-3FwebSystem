# Cloudflare 설정 가이드

## ⚠️ 주의

> **설정 전 반드시 `/check-docs cloudflare` 실행하여 최신 문서 확인!**

---

## 역할

Cloudflare는 다음 역할을 담당합니다:
- **DNS 관리**: 도메인 네임 서버
- **CDN**: 전 세계 엣지 캐싱
- **SSL/TLS**: HTTPS 인증서
- **보안**: DDoS 방어, WAF

---

## 초기 설정

### 1. 도메인 추가

1. Cloudflare 대시보드 접속
2. "Add a Site" 클릭
3. 도메인 입력 (example.com)
4. Free 플랜 선택
5. 네임서버 변경 안내 확인

### 2. 네임서버 변경

도메인 등록기관에서 네임서버를 Cloudflare로 변경:
```
# Cloudflare 제공 네임서버 (예시)
ns1.cloudflare.com
ns2.cloudflare.com
```

---

## DNS 설정

### Vercel 연결

| Type | Name | Content | Proxy |
|------|------|---------|-------|
| CNAME | @ | cname.vercel-dns.com | ✅ Proxied |
| CNAME | www | cname.vercel-dns.com | ✅ Proxied |

### 기타 레코드 (필요시)

```
# 이메일 (MX)
MX    @    mail.example.com    10    DNS only

# 이메일 인증 (SPF)
TXT   @    "v=spf1 include:_spf.google.com ~all"    DNS only

# 서브도메인 (API 등)
CNAME api  cname.vercel-dns.com    Proxied
```

---

## SSL/TLS 설정

### 권장 설정

| 설정 | 값 | 설명 |
|------|-----|------|
| Mode | Full (strict) | 완전 암호화 |
| Always Use HTTPS | On | HTTP → HTTPS 리다이렉트 |
| Automatic HTTPS Rewrites | On | 혼합 콘텐츠 자동 수정 |
| Minimum TLS Version | 1.2 | 보안 강화 |
| TLS 1.3 | On | 최신 프로토콜 |

### Edge Certificates

- Universal SSL (무료) 자동 발급
- 발급까지 최대 24시간 소요

---

## 캐싱 설정

### Browser Cache TTL

| 콘텐츠 | TTL |
|--------|-----|
| 정적 에셋 (JS, CSS, 이미지) | 1 year |
| HTML | 4 hours |
| API | No cache |

### Cache Rules (Page Rules 대체)

```
# 정적 에셋 캐싱
IF: (http.request.uri.path matches "/_next/static/*")
THEN: Cache Level = Cache Everything, Edge TTL = 1 year

# 폰트 캐싱
IF: (http.request.uri.path matches "/fonts/*")
THEN: Cache Level = Cache Everything, Edge TTL = 1 year

# API 캐시 바이패스
IF: (http.request.uri.path matches "/api/*")
THEN: Cache Level = Bypass
```

---

## 성능 설정

### Speed → Optimization

| 설정 | 권장 값 | 비고 |
|------|---------|------|
| Auto Minify (HTML) | ✅ On | |
| Auto Minify (CSS) | ✅ On | |
| Auto Minify (JS) | ✅ On | |
| Brotli | ✅ On | 압축 |
| Early Hints | ✅ On | 103 응답 |
| Rocket Loader | ❌ Off | Next.js 충돌 |
| Mirage | ❌ Off | next/image 충돌 |

### Speed → Image Optimization

| 설정 | 권장 값 |
|------|---------|
| Polish | Off (next/image 사용) |
| WebP | Off (next/image 사용) |

---

## 보안 설정

### Security → Settings

| 설정 | 권장 값 |
|------|---------|
| Security Level | Medium |
| Challenge Passage | 30 minutes |
| Browser Integrity Check | On |

### Security → Bots

| 설정 | 권장 값 |
|------|---------|
| Bot Fight Mode | On |

---

## 무료 티어 제한

| 항목 | 제한 |
|------|------|
| 요청 수 | 무제한 |
| Bandwidth | 무제한 |
| SSL 인증서 | 포함 |
| DDoS 방어 | 포함 |
| Page Rules | 3개 |
| Cache Rules | 10개 |

---

## 트러블슈팅

### SSL 인증서 오류

1. SSL/TLS → Full (strict) 모드 확인
2. Edge Certificates 발급 상태 확인
3. 24시간 대기 후 재확인

### 캐시 문제

```bash
# 캐시 퍼지 (Cloudflare Dashboard)
Caching → Configuration → Purge Cache → Purge Everything
```

### DNS 전파 지연

- 최대 48시간 소요 가능
- 확인: https://dnschecker.org

### Vercel 연결 실패

1. DNS 레코드 타입 확인 (CNAME)
2. Proxy 상태 확인 (Proxied)
3. Vercel에서 도메인 검증 상태 확인

---

## 모니터링

### Analytics

- Requests: 요청 수
- Bandwidth: 대역폭 사용량
- Cache Rate: 캐시 히트율
- Threats: 차단된 위협

### Security Events

- 차단된 요청 확인
- Bot 트래픽 분석
