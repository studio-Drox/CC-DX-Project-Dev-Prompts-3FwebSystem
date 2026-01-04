# String Tune 인터랙션 가이드

## 개요

**String Tune**: https://string-tune.fiddle.digital/

웹 기반 인터랙션/애니메이션 라이브러리로, 
스크롤 트리거 애니메이션과 마이크로 인터랙션에 활용합니다.

---

## ⛔ 중요: PWA 사용 금지

> **모바일 PWA에서는 String Tune 사용을 금지합니다.**

### 이유
- PWA 환경에서 성능 저하
- 배터리 소모 증가
- 일부 기기 호환성 문제

### PWA 감지 코드

```typescript
// lib/animation.ts

/**
 * PWA Standalone 모드 감지
 */
export const isStandalonePWA = (): boolean => {
  if (typeof window === 'undefined') return false;
  
  // iOS Safari standalone
  if ((window.navigator as any).standalone === true) {
    return true;
  }
  
  // Android Chrome / Desktop PWA
  if (window.matchMedia('(display-mode: standalone)').matches) {
    return true;
  }
  
  // Window Controls Overlay (Desktop PWA)
  if (window.matchMedia('(display-mode: window-controls-overlay)').matches) {
    return true;
  }
  
  return false;
};

/**
 * 애니메이션 사용 가능 여부
 * PWA가 아니고, prefers-reduced-motion이 아닐 때만 true
 */
export const shouldUseAnimation = (): boolean => {
  if (typeof window === 'undefined') return false;
  
  // PWA면 비활성화
  if (isStandalonePWA()) return false;
  
  // 사용자가 모션 감소를 선호하면 비활성화
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    return false;
  }
  
  return true;
};

/**
 * 애니메이션 클래스 조건부 적용 유틸
 */
export const getAnimationClass = (
  animationClass: string,
  fallbackClass: string = ''
): string => {
  return shouldUseAnimation() ? animationClass : fallbackClass;
};
```

---

## 설치 및 설정

### 설치
```bash
pnpm add string-tune
```

### 초기화
```tsx
// app/layout.tsx 또는 providers.tsx
'use client';

import { useEffect } from 'react';
import { shouldUseAnimation } from '@/lib/animation';

export function AnimationProvider({ children }: { children: React.ReactNode }) {
  useEffect(() => {
    if (shouldUseAnimation()) {
      // String Tune 초기화
      import('string-tune').then((StringTune) => {
        StringTune.init({
          // 설정 옵션
        });
      });
    }
  }, []);

  return <>{children}</>;
}
```

---

## 주요 사용 패턴

### 1. 스크롤 트리거 페이드인

```tsx
'use client';

import { useEffect, useRef } from 'react';
import { shouldUseAnimation } from '@/lib/animation';
import { cn } from '@/lib/utils';

interface FadeInSectionProps {
  children: React.ReactNode;
  className?: string;
  delay?: number;
}

export function FadeInSection({ 
  children, 
  className,
  delay = 0 
}: FadeInSectionProps) {
  const ref = useRef<HTMLDivElement>(null);
  const enableAnimation = shouldUseAnimation();
  
  useEffect(() => {
    if (!enableAnimation || !ref.current) return;
    
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            setTimeout(() => {
              entry.target.classList.add('animate-in');
            }, delay);
          }
        });
      },
      { threshold: 0.1 }
    );
    
    observer.observe(ref.current);
    return () => observer.disconnect();
  }, [enableAnimation, delay]);
  
  return (
    <div
      ref={ref}
      className={cn(
        enableAnimation && 'opacity-0 translate-y-8 transition-all duration-700',
        className
      )}
    >
      {children}
    </div>
  );
}
```

### 2. 순차 등장 애니메이션

```tsx
'use client';

import { shouldUseAnimation } from '@/lib/animation';
import { cn } from '@/lib/utils';

interface StaggerChildrenProps {
  children: React.ReactNode[];
  className?: string;
  staggerDelay?: number;
}

export function StaggerChildren({
  children,
  className,
  staggerDelay = 100,
}: StaggerChildrenProps) {
  const enableAnimation = shouldUseAnimation();
  
  return (
    <div className={className}>
      {children.map((child, index) => (
        <div
          key={index}
          className={cn(
            enableAnimation && 'animate-fade-in-up'
          )}
          style={
            enableAnimation
              ? { animationDelay: `${index * staggerDelay}ms` }
              : undefined
          }
        >
          {child}
        </div>
      ))}
    </div>
  );
}
```

### 3. 패럴랙스 효과

```tsx
'use client';

import { useEffect, useState } from 'react';
import { shouldUseAnimation } from '@/lib/animation';

interface ParallaxProps {
  children: React.ReactNode;
  speed?: number; // 0.1 ~ 1
}

export function Parallax({ children, speed = 0.5 }: ParallaxProps) {
  const [offset, setOffset] = useState(0);
  const enableAnimation = shouldUseAnimation();
  
  useEffect(() => {
    if (!enableAnimation) return;
    
    const handleScroll = () => {
      setOffset(window.scrollY * speed);
    };
    
    window.addEventListener('scroll', handleScroll, { passive: true });
    return () => window.removeEventListener('scroll', handleScroll);
  }, [enableAnimation, speed]);
  
  return (
    <div
      style={
        enableAnimation
          ? { transform: `translateY(${offset}px)` }
          : undefined
      }
    >
      {children}
    </div>
  );
}
```

---

## CSS 애니메이션 클래스

### globals.css에 추가

```css
@layer utilities {
  /* 페이드인 */
  .animate-fade-in {
    animation: fadeIn 0.6s ease-out forwards;
  }
  
  /* 페이드인 + 위로 */
  .animate-fade-in-up {
    animation: fadeInUp 0.6s ease-out forwards;
  }
  
  /* 페이드인 + 스케일 */
  .animate-fade-in-scale {
    animation: fadeInScale 0.5s ease-out forwards;
  }
  
  /* 슬라이드 인 (좌측에서) */
  .animate-slide-in-left {
    animation: slideInLeft 0.5s ease-out forwards;
  }
  
  /* 슬라이드 인 (우측에서) */
  .animate-slide-in-right {
    animation: slideInRight 0.5s ease-out forwards;
  }
  
  /* 애니메이션 활성화 클래스 */
  .animate-in {
    opacity: 1 !important;
    transform: translateY(0) !important;
  }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeInScale {
  from {
    opacity: 0;
    transform: scale(0.95);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes slideInLeft {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes slideInRight {
  from {
    opacity: 0;
    transform: translateX(20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}
```

---

## 권장 / 비권장 사용처

### ✅ 권장 사용처
- 히어로 섹션 배경 패럴랙스
- 섹션 진입 시 콘텐츠 페이드인
- 서비스/갤러리 카드 순차 등장
- 랜딩 페이지 스크롤 스토리텔링
- 숫자 카운트업 애니메이션

### ❌ 비권장 사용처
- 모바일 PWA (절대 금지)
- 폼 입력 필드 (사용성 저하)
- 핵심 네비게이션 (접근성)
- 빠른 인터랙션 필요 영역
- 접근성 중요 콘텐츠

---

## 체크리스트

- [ ] lib/animation.ts에 PWA 감지 로직 구현
- [ ] shouldUseAnimation() 사용하여 조건부 적용
- [ ] prefers-reduced-motion 대응
- [ ] 폼/네비게이션에는 애니메이션 미적용
- [ ] CSS 폴백 클래스 준비
