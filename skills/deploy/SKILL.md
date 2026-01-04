# 배포 스킬

## ⚠️ 필수 주의사항

> **Vercel, Cloudflare 관련 작업 전 반드시 최신 공식 문서를 확인할 것!**

```bash
# 문서 확인 커맨드
/check-docs vercel
/check-docs cloudflare
```

---

## 아키텍처 개요

```
GitHub (소스) → Vercel (호스팅) → Cloudflare (CDN/DNS)
   Free            Hobby (Free)         Free
```

모든 인프라는 **무료 티어**로 구성합니다.
유료 서비스 사용은 금지입니다.

---

## 상세 가이드

### Vercel 배포
@skills/deploy/vercel.md

### Cloudflare 설정
@skills/deploy/cloudflare.md

### GitHub Actions
@skills/deploy/github-actions.md

---

## 빠른 배포 체크리스트

### 1. GitHub
- [ ] 저장소 생성 (Public 또는 Private)
- [ ] .gitignore 설정
- [ ] 첫 커밋 및 푸시

### 2. Vercel
- [ ] GitHub 연동
- [ ] 프로젝트 Import
- [ ] 환경 변수 설정
- [ ] 빌드 설정 확인
- [ ] 배포 테스트

### 3. Cloudflare
- [ ] 도메인 추가
- [ ] DNS 레코드 설정
- [ ] SSL/TLS Full (strict)
- [ ] 캐싱 규칙 설정

### 4. 도메인 연결
- [ ] Vercel에 도메인 추가
- [ ] DNS 전파 확인
- [ ] HTTPS 작동 확인

---

## 무료 티어 제한 요약

### Vercel Hobby
- Bandwidth: 100GB/월
- Build: 6,000분/월
- Serverless: 100GB-Hours
- Edge: 500K invocations

### Cloudflare Free
- 요청: 무제한
- Bandwidth: 무제한
- Page Rules: 3개

### GitHub Free
- Actions: 2,000분/월
- Packages: 500MB

---

## 모니터링

### Vercel
- Analytics (기본 제공)
- Speed Insights
- Logs

### Cloudflare
- Analytics
- Security Events
- Cache Stats
