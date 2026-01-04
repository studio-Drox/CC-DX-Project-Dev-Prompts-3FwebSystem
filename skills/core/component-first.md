# 컴포넌트 퍼스트 개발 원칙

## 핵심 철학

> **"페이지를 만들지 말고, 컴포넌트를 조립하라"**

페이지 단위로 개발하면 중복 코드가 늘어나고 유지보수가 어려워집니다.
컴포넌트 단위로 먼저 설계하고, 이를 조립하여 페이지를 완성합니다.

---

## Atomic Design 기반 계층

```
┌─────────────────────────────────────────────────────────────┐
│                         Pages                                │
│   (app/page.tsx, app/about/page.tsx, ...)                   │
├─────────────────────────────────────────────────────────────┤
│                       Templates                              │
│   (PageLayout, SectionLayout, GridLayout, ...)              │
├─────────────────────────────────────────────────────────────┤
│                       Organisms                              │
│   (Header, Footer, Hero, ServiceCard, ...)                  │
├─────────────────────────────────────────────────────────────┤
│                       Molecules                              │
│   (NavItem, FormField, SearchBar, CardHeader, ...)          │
├─────────────────────────────────────────────────────────────┤
│                         Atoms                                │
│   (Button, Input, Badge, Icon, Text, ...)                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 컴포넌트 설계 원칙

### 1. Single Responsibility
```tsx
// ❌ 너무 많은 책임
const Header = () => {
  const [user, setUser] = useState();
  const [cart, setCart] = useState();
  const [menu, setMenu] = useState();
  // ... 인증, 장바구니, 메뉴 로직 모두 포함
};

// ✅ 분리된 책임
const Header = () => (
  <header>
    <Logo />
    <Navigation />
    <UserMenu />
  </header>
);
```

### 2. Props로 제어 가능
```tsx
// ❌ 하드코딩
const Button = () => (
  <button className="bg-blue-500 text-white px-4 py-2">
    클릭
  </button>
);

// ✅ Props로 변형
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'outline';
  size?: 'sm' | 'md' | 'lg';
  children: React.ReactNode;
}

const Button = ({ variant = 'primary', size = 'md', children }: ButtonProps) => (
  <button className={buttonVariants({ variant, size })}>
    {children}
  </button>
);
```

### 3. 합성 가능 (Composable)
```tsx
// ✅ 합성 패턴
<Card>
  <Card.Header>
    <Card.Title>제목</Card.Title>
    <Card.Description>설명</Card.Description>
  </Card.Header>
  <Card.Content>
    본문 내용
  </Card.Content>
  <Card.Footer>
    <Button>액션</Button>
  </Card.Footer>
</Card>
```

---

## 컴포넌트 파일 구조

### 표준 구조
```
src/components/common/Header/
├── index.tsx              # 메인 export
├── Header.tsx             # 메인 컴포넌트
├── Header.types.ts        # 타입 정의
├── Header.utils.ts        # 헬퍼 함수 (선택)
├── MobileNav.tsx          # 서브 컴포넌트
├── NavItem.tsx            # 서브 컴포넌트
└── Header.test.tsx        # 테스트 (선택)
```

### index.tsx 패턴
```tsx
// 깔끔한 export
export { Header } from './Header';
export type { HeaderProps } from './Header.types';

// 또는 default export
export { default } from './Header';
```

---

## 타입 정의 패턴

### 기본 Props 타입
```tsx
// Header.types.ts
import type { ReactNode } from 'react';

export interface NavItem {
  label: string;
  href: string;
  children?: NavItem[];
}

export interface HeaderProps {
  /** 헤더 변형 */
  variant?: 'default' | 'transparent' | 'dark';
  
  /** 스티키 여부 */
  sticky?: boolean;
  
  /** 로고 이미지 경로 */
  logo?: string;
  
  /** 네비게이션 아이템 */
  navItems?: NavItem[];
  
  /** CTA 버튼 텍스트 */
  ctaText?: string;
  
  /** CTA 클릭 핸들러 */
  onCtaClick?: () => void;
  
  /** 추가 클래스명 */
  className?: string;
  
  /** 자식 요소 */
  children?: ReactNode;
}
```

### Variant 타입 with CVA
```tsx
import { cva, type VariantProps } from 'class-variance-authority';

const headerVariants = cva(
  'w-full flex items-center justify-between px-4 py-3',
  {
    variants: {
      variant: {
        default: 'bg-white border-b',
        transparent: 'bg-transparent absolute top-0 left-0 right-0',
        dark: 'bg-gray-900 text-white',
      },
      sticky: {
        true: 'sticky top-0 z-50',
        false: '',
      },
    },
    defaultVariants: {
      variant: 'default',
      sticky: true,
    },
  }
);

export type HeaderVariants = VariantProps<typeof headerVariants>;
export interface HeaderProps extends HeaderVariants {
  // 추가 props
}
```

---

## 재사용성 체크리스트

컴포넌트 작성 시 다음을 확인:

- [ ] **Props로 커스터마이징 가능한가?**
  - 색상, 크기, 변형 등이 Props로 제어됨
  
- [ ] **하드코딩된 값이 없는가?**
  - 텍스트, 색상, 크기 등이 Props나 테마에서 옴
  
- [ ] **다른 페이지에서 재사용 가능한가?**
  - 특정 페이지에 종속되지 않음
  
- [ ] **타입이 명확하게 정의되었는가?**
  - 모든 Props에 타입과 JSDoc 주석
  
- [ ] **기본값이 합리적인가?**
  - Props 없이도 기본 동작함
  
- [ ] **접근성이 고려되었는가?**
  - aria-*, role, tabIndex 등

---

## 예시: Hero 컴포넌트

### Hero.types.ts
```tsx
export interface HeroProps {
  title: string;
  subtitle?: string;
  backgroundImage?: string;
  ctaText?: string;
  ctaHref?: string;
  variant?: 'fullscreen' | 'half' | 'minimal';
  overlay?: boolean;
  className?: string;
}
```

### Hero.tsx
```tsx
import Image from 'next/image';
import Link from 'next/link';
import { cn } from '@/lib/utils';
import { shouldUseAnimation } from '@/lib/animation';
import { Button } from '@/components/ui/button';
import type { HeroProps } from './Hero.types';

const variantStyles = {
  fullscreen: 'min-h-screen',
  half: 'min-h-[50vh]',
  minimal: 'py-20',
};

export function Hero({
  title,
  subtitle,
  backgroundImage,
  ctaText,
  ctaHref = '#contact',
  variant = 'fullscreen',
  overlay = true,
  className,
}: HeroProps) {
  const enableAnimation = shouldUseAnimation();
  
  return (
    <section
      className={cn(
        'relative flex items-center justify-center',
        variantStyles[variant],
        className
      )}
    >
      {backgroundImage && (
        <>
          <Image
            src={backgroundImage}
            alt=""
            fill
            className="object-cover"
            priority
          />
          {overlay && (
            <div className="absolute inset-0 bg-black/50" />
          )}
        </>
      )}
      
      <div
        className={cn(
          'relative z-10 text-center text-white px-4',
          enableAnimation && 'animate-fade-in-up'
        )}
      >
        <h1 className="font-heading text-4xl md:text-6xl font-bold mb-4">
          {title}
        </h1>
        
        {subtitle && (
          <p className="text-lg md:text-xl mb-8 max-w-2xl mx-auto">
            {subtitle}
          </p>
        )}
        
        {ctaText && (
          <Button asChild size="lg">
            <Link href={ctaHref}>{ctaText}</Link>
          </Button>
        )}
      </div>
    </section>
  );
}
```

---

## 페이지 조립 예시

```tsx
// app/page.tsx
import { Hero } from '@/components/sections/Hero';
import { ServicesPreview } from '@/components/sections/ServicesPreview';
import { AboutPreview } from '@/components/sections/AboutPreview';
import { Testimonials } from '@/components/sections/Testimonials';
import { CTASection } from '@/components/sections/CTASection';

export default function HomePage() {
  return (
    <>
      <Hero
        title="아름다움을 디자인합니다"
        subtitle="강남 최고의 피부과 클리닉"
        backgroundImage="https://source.unsplash.com/random/1920x1080?aesthetic-clinic"
        ctaText="상담 예약하기"
      />
      
      <ServicesPreview
        services={services}
        title="대표 시술"
      />
      
      <AboutPreview
        title="클리닉 소개"
        description="20년 경력의 전문의가..."
      />
      
      <Testimonials
        items={testimonials}
        title="고객 후기"
      />
      
      <CTASection
        title="지금 상담 예약하세요"
        buttonText="예약하기"
      />
    </>
  );
}
```
