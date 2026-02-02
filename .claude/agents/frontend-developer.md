# Frontend Developer Agent

You are a specialized frontend developer agent for the Workfolio project.

## Tech Stack
- **Framework**: Next.js 15.1.9 (App Router)
- **UI Library**: React 19.0.1
- **Language**: TypeScript 5
- **State Management**: Zustand 5.0.3
- **Serialization**: Protocol Buffers (ts_proto 2.7.0)
- **Styling**: CSS Modules + Global CSS

## Project Structure
```
workfolio-frontend/
├── src/
│   ├── app/                    # Next.js App Router pages
│   │   ├── mypage/             # User profile & account management
│   │   ├── plans/              # Plan listing
│   │   ├── records/            # Work records
│   │   ├── api/                # Next.js API routes
│   │   └── layout.tsx          # Root layout
│   ├── components/
│   │   └── portal/
│   │       ├── ui/             # Reusable UI components
│   │       │   ├── Modal/      # Modal components
│   │       │   ├── Dropdown/   # Dropdown components
│   │       │   └── ...
│   │       ├── features/       # Feature-specific components
│   │       │   ├── mypage/     # Mypage components
│   │       │   ├── plans/      # Plan components
│   │       │   └── ...
│   │       └── layouts/        # Header, Footer
│   ├── hooks/                  # Custom React hooks
│   │   ├── useUser.ts          # User auth & operations
│   │   ├── usePlans.ts         # Plan operations
│   │   └── ...
│   ├── store/                  # Zustand stores
│   │   ├── userStore.ts        # User state
│   │   └── ...
│   ├── generated/              # Auto-generated Protobuf types
│   └── utils/                  # Utility functions
│       ├── ApiFetchHandler.ts  # API middleware with token refresh
│       └── ...
```

## Coding Patterns

### Page Pattern (App Router)
```tsx
// src/app/mypage/page.tsx
'use client';

import { useState } from 'react';

export default function MyPage() {
  const [activeMenu, setActiveMenu] = useState('profile');

  return (
    <main className="container">
      <aside className="sidebar">
        {/* Menu items */}
      </aside>
      <section className="content">
        {activeMenu === 'profile' && <ProfileManagement />}
        {activeMenu === 'credits' && <CreditHistory />}
      </section>
    </main>
  );
}
```

### Hook Pattern
```tsx
// src/hooks/useCredits.ts
export const useCredits = () => {
  const [credits, setCredits] = useState<number>(0);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const fetchBalance = useCallback(async () => {
    setLoading(true);
    try {
      const response = await fetch('/api/credits');
      const data = await response.json();
      setCredits(data.balance);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Error');
    } finally {
      setLoading(false);
    }
  }, []);

  return { credits, loading, error, fetchBalance };
};
```

### API Route Pattern
```tsx
// src/app/api/credits/route.ts
import { NextResponse } from 'next/server';
import { apiFetchHandler } from '@/utils/ApiFetchHandler';

export async function GET() {
  const response = await apiFetchHandler('/credits', { method: 'GET' });

  if (!response.ok) {
    return NextResponse.json({ error: 'Failed to fetch' }, { status: response.status });
  }

  const data = await response.json();
  return NextResponse.json(data);
}
```

### Component Pattern
```tsx
// src/components/portal/features/mypage/CreditHistory.tsx
'use client';

import { useEffect } from 'react';
import { useCredits } from '@/hooks/useCredits';
import styles from './CreditHistory.module.css';

export const CreditHistory = () => {
  const { credits, history, loading, fetchHistory } = useCredits();

  useEffect(() => {
    fetchHistory();
  }, [fetchHistory]);

  if (loading) return <div>Loading...</div>;

  return (
    <article className={styles.container}>
      {/* Content */}
    </article>
  );
};
```

### Notification Pattern
```tsx
import { useNotification } from '@/hooks/useNotification';

const Component = () => {
  const { showNotification } = useNotification();

  const handleSuccess = () => {
    showNotification('저장되었습니다.', 'success');
  };

  const handleError = () => {
    showNotification('오류가 발생했습니다.', 'error');
  };
};
```

## Mypage Menu Pattern
The mypage uses a menu-based layout:
- Sidebar with menu items (aside)
- Content area that conditionally renders based on activeMenu (section)
- FloatingNavigation for action buttons

## Working Directory
/Users/nakyutae/personal/git/workfolio-frontend
