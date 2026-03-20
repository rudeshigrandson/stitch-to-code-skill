# Web (React / Next.js) Reference

## Project Structure

```
src/
├── app/                    # Next.js App Router
│   ├── layout.tsx          # Root layout with providers
│   ├── page.tsx            # Home page
│   └── [route]/
│       └── page.tsx
├── components/
│   ├── ui/                 # Base components
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.types.ts
│   │   │   └── index.ts
│   │   ├── Card/
│   │   ├── Input/
│   │   └── ...
│   └── layout/             # Layout components
│       ├── Header/
│       ├── Footer/
│       └── Navigation/
├── theme/
│   ├── colors.ts
│   ├── typography.ts
│   ├── spacing.ts
│   └── index.ts
├── hooks/                  # Custom hooks
├── lib/                    # Utilities
└── types/                  # TypeScript types
```

## Component Template

```tsx
// components/ui/Button/Button.tsx
import { cn } from '@/lib/utils';

interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  children: React.ReactNode;
}

export function Button({
  variant = 'primary',
  size = 'md',
  className,
  children,
  ...props
}: ButtonProps) {
  return (
    <button
      className={cn(
        'inline-flex items-center justify-center rounded-md font-medium transition-colors',
        'focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2',
        {
          'bg-primary text-white hover:bg-primary/90': variant === 'primary',
          'bg-secondary text-secondary-foreground hover:bg-secondary/80': variant === 'secondary',
          'hover:bg-accent hover:text-accent-foreground': variant === 'ghost',
        },
        {
          'h-9 px-3 text-sm': size === 'sm',
          'h-10 px-4 text-base': size === 'md',
          'h-12 px-6 text-lg': size === 'lg',
        },
        className
      )}
      {...props}
    >
      {children}
    </button>
  );
}
```

## Theme Setup

```tsx
// theme/colors.ts
export const colors = {
  primary: '#4A90E2',
  secondary: '#7B61FF',
  accent: '#FF6B6B',
  background: '#FFFFFF',
  surface: '#F5F5F5',
  text: {
    primary: '#111111',
    secondary: '#666666',
    muted: '#999999',
  },
} as const;

// tailwind.config.ts - extend with these colors
```

## Tailwind Configuration

```ts
// tailwind.config.ts
import type { Config } from 'tailwindcss';
import { colors } from './src/theme/colors';
import { spacing } from './src/theme/spacing';

const config: Config = {
  content: ['./src/**/*.{js,ts,jsx,tsx,mdx}'],
  theme: {
    extend: {
      colors: {
        primary: colors.primary,
        secondary: colors.secondary,
        // ... map all colors
      },
      spacing: {
        // Map spacing scale
      },
      borderRadius: {
        // Map radius scale
      },
    },
  },
  plugins: [],
};

export default config;
```

## Navigation Patterns

### Tab Navigation (Bottom)
```tsx
'use client';
import Link from 'next/link';
import { usePathname } from 'next/navigation';

const tabs = [
  { href: '/', label: 'Home', icon: HomeIcon },
  { href: '/search', label: 'Search', icon: SearchIcon },
  { href: '/profile', label: 'Profile', icon: UserIcon },
];

export function BottomNav() {
  const pathname = usePathname();
  
  return (
    <nav className="fixed bottom-0 left-0 right-0 bg-white border-t">
      <div className="flex justify-around py-2">
        {tabs.map((tab) => (
          <Link
            key={tab.href}
            href={tab.href}
            className={cn(
              'flex flex-col items-center p-2',
              pathname === tab.href ? 'text-primary' : 'text-muted'
            )}
          >
            <tab.icon className="w-6 h-6" />
            <span className="text-xs">{tab.label}</span>
          </Link>
        ))}
      </div>
    </nav>
  );
}
```

### Stack Navigation
```tsx
// Use Next.js App Router - each page is a route
// Link components for navigation
// useRouter() for programmatic navigation
```

## Responsive Patterns

```tsx
// Mobile-first responsive design
<div className="
  flex flex-col          // Mobile: stack
  md:flex-row            // Tablet+: row
  gap-4
  p-4 md:p-6 lg:p-8      // Responsive padding
">
  <aside className="
    w-full md:w-64       // Full width mobile, fixed sidebar desktop
  ">
    {/* Sidebar */}
  </aside>
  <main className="flex-1">
    {/* Content */}
  </main>
</div>
```

## State Management (Zustand)

```tsx
// stores/useAppStore.ts
import { create } from 'zustand';

interface AppState {
  theme: 'light' | 'dark';
  setTheme: (theme: 'light' | 'dark') => void;
}

export const useAppStore = create<AppState>((set) => ({
  theme: 'light',
  setTheme: (theme) => set({ theme }),
}));
```

## Animation (Framer Motion)

```tsx
import { motion } from 'framer-motion';

<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  exit={{ opacity: 0, y: -20 }}
  transition={{ duration: 0.2 }}
>
  {/* Content */}
</motion.div>
```
