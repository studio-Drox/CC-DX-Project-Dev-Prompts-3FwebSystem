# UI 라이브러리 가이드

## 엔터프라이즈급 기준

> **"중소기업 홈페이지가 아닌, 프로덕트 수준의 UI"**

3F Web의 모든 프로젝트는 엔터프라이즈급 프로덕트 수준의 UI를 유지합니다.

---

## 기본 UI 스택

### Shadcn/ui
- **역할**: 기본 UI 컴포넌트
- **특징**: Headless UI 기반, 완전 커스터마이징 가능
- **설치**: `pnpm dlx shadcn@latest init`

### Radix Primitives
- **역할**: 접근성 보장 프리미티브
- **특징**: WAI-ARIA 완벽 지원
- **용도**: 복잡한 인터랙션 (Modal, Dropdown, Tooltip 등)

### Tailwind CSS
- **역할**: 스타일링
- **특징**: JIT 모드, 유틸리티 퍼스트

### Lucide Icons
- **역할**: 아이콘
- **특징**: 트리 쉐이킹 지원, 일관된 디자인

---

## Shadcn/ui 초기화

```bash
# 초기화
pnpm dlx shadcn@latest init

# 권장 설정
✔ Would you like to use TypeScript? yes
✔ Which style would you like to use? Default
✔ Which color would you like to use as base color? Slate
✔ Where is your global CSS file? src/styles/globals.css
✔ Would you like to use CSS variables? yes
✔ Where is your tailwind.config located? tailwind.config.ts
✔ Configure import alias for components? @/components
✔ Configure import alias for utils? @/lib/utils
```

---

## 필수 컴포넌트

### 기본 설치
```bash
# 기본 컴포넌트
pnpm dlx shadcn@latest add button
pnpm dlx shadcn@latest add input
pnpm dlx shadcn@latest add label
pnpm dlx shadcn@latest add card
pnpm dlx shadcn@latest add dialog
pnpm dlx shadcn@latest add dropdown-menu
pnpm dlx shadcn@latest add navigation-menu
pnpm dlx shadcn@latest add sheet
pnpm dlx shadcn@latest add accordion
pnpm dlx shadcn@latest add tabs
pnpm dlx shadcn@latest add form
pnpm dlx shadcn@latest add toast
```

---

## 커스터마이징 가이드

### Button 확장
```tsx
// components/ui/button.tsx에 variant 추가
const buttonVariants = cva(
  '...기존 스타일...',
  {
    variants: {
      variant: {
        // 기존 variants...
        
        // 커스텀 추가
        gradient: 'bg-gradient-to-r from-primary to-secondary text-white',
        glass: 'bg-white/20 backdrop-blur-md border border-white/30',
      },
      // ...
    },
  }
);
```

### 테마 컬러 적용
```tsx
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        // 시맨틱 컬러 (colors.json 기반)
        primary: {
          DEFAULT: 'hsl(var(--primary))',
          foreground: 'hsl(var(--primary-foreground))',
        },
        secondary: {
          DEFAULT: 'hsl(var(--secondary))',
          foreground: 'hsl(var(--secondary-foreground))',
        },
        accent: {
          DEFAULT: 'hsl(var(--accent))',
          foreground: 'hsl(var(--accent-foreground))',
        },
      },
    },
  },
};
```

---

## 폼 처리

### React Hook Form + Zod
```tsx
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const formSchema = z.object({
  name: z.string().min(2, '이름을 입력해주세요'),
  email: z.string().email('올바른 이메일을 입력해주세요'),
  phone: z.string().regex(/^01[0-9]-?[0-9]{4}-?[0-9]{4}$/, '올바른 전화번호를 입력해주세요'),
});

type FormData = z.infer<typeof formSchema>;

export function ContactForm() {
  const form = useForm<FormData>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      name: '',
      email: '',
      phone: '',
    },
  });

  const onSubmit = (data: FormData) => {
    console.log(data);
  };

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)}>
        <FormField
          control={form.control}
          name="name"
          render={({ field }) => (
            <FormItem>
              <FormLabel>이름</FormLabel>
              <FormControl>
                <Input {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        {/* 나머지 필드 */}
        <Button type="submit">제출</Button>
      </form>
    </Form>
  );
}
```

---

## 반응형 패턴

### 모바일 네비게이션
```tsx
import { Sheet, SheetContent, SheetTrigger } from '@/components/ui/sheet';
import { Menu } from 'lucide-react';

export function MobileNav({ items }: { items: NavItem[] }) {
  return (
    <Sheet>
      <SheetTrigger asChild className="md:hidden">
        <Button variant="ghost" size="icon">
          <Menu className="h-6 w-6" />
        </Button>
      </SheetTrigger>
      <SheetContent side="right">
        <nav className="flex flex-col space-y-4 mt-8">
          {items.map((item) => (
            <Link key={item.href} href={item.href}>
              {item.label}
            </Link>
          ))}
        </nav>
      </SheetContent>
    </Sheet>
  );
}
```

### 반응형 그리드
```tsx
// 1~4열 반응형 그리드
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
  {items.map((item) => (
    <Card key={item.id}>...</Card>
  ))}
</div>
```

---

## 접근성 체크리스트

- [ ] **키보드 네비게이션**: Tab으로 모든 인터랙티브 요소 접근 가능
- [ ] **포커스 표시**: focus-visible 스타일 적용
- [ ] **스크린 리더**: aria-label, role 적절히 사용
- [ ] **색상 대비**: WCAG AA 기준 충족 (4.5:1)
- [ ] **모션 감소**: prefers-reduced-motion 대응
- [ ] **폼 레이블**: 모든 입력에 label 연결

---

## 금지 사항

1. **인라인 스타일 금지**
   ```tsx
   // ❌
   <div style={{ color: 'red' }}>
   
   // ✅
   <div className="text-red-500">
   ```

2. **하드코딩 색상 금지**
   ```tsx
   // ❌
   <div className="bg-[#3B82F6]">
   
   // ✅
   <div className="bg-primary">
   ```

3. **!important 금지**
   ```css
   /* ❌ */
   .custom { color: red !important; }
   
   /* ✅ 더 구체적인 선택자 사용 */
   ```

4. **글로벌 스타일 남용 금지**
   - 컴포넌트 스코프 내에서 해결
   - 정말 필요한 경우만 globals.css 사용
