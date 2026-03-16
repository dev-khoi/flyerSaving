# Context7 Notes: Next.js App Router + React + Tailwind + TypeScript

Generated: March 16, 2026 (America/New_York)

This file captures the project-relevant takeaways pulled via Context7 for:
- Next.js App Router
- React
- Tailwind CSS
- TypeScript (as used in the above docs/snippets)

Context7 library IDs used:
- `/vercel/next.js/v15.1.8`
- `/reactjs/react.dev`
- `/tailwindlabs/tailwindcss.com`

Optional Context7 session key (from this workspace chat):
- `ctx7sk-6998a189-d941-4119-a0a5-9d34be928785`

## Next.js App Router: Server vs Client Components

- Files under `app/` are Server Components by default.
- Use the `'use client'` directive to opt a file into Client Component behavior (state/effects, browser-only APIs).
- Data fetching can happen directly in a Server Component and be forwarded to Client Components via props.

Example (Server Component fetching + rendering a Client Component):

```tsx
// app/page.tsx (Server Component by default)
import HomePage from './home-page'

async function getPosts() {
  const res = await fetch('https://...')
  const posts = await res.json()
  return posts
}

export default async function Page() {
  const recentPosts = await getPosts()
  return <HomePage recentPosts={recentPosts} />
}
```

Example (Client Component):

```tsx
'use client'

export default function HomePage({ recentPosts }: { recentPosts: Array<{ id: string; title: string }> }) {
  return (
    <div>
      {recentPosts.map((post) => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  )
}
```

## Next.js App Router: Navigation Hooks

Routing hooks for the `app/` directory come from `next/navigation` and must be used in a Client Component.

```tsx
'use client'

import { useRouter, usePathname, useSearchParams } from 'next/navigation'

export default function ExampleClientComponent() {
  const router = useRouter()
  const pathname = usePathname()
  const searchParams = useSearchParams()

  // ...
}
```

## Tailwind CSS: Ensure `app/` Is Included in Content Globs

When using Tailwind with the App Router, make sure Tailwind scans the `app/` directory.
This config is TS-typed (compatible with common Next.js setups):

```ts
import type { Config } from 'tailwindcss'

const config: Config = {
  content: [
    './app/**/*.{js,ts,jsx,tsx,mdx}',
    './pages/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
    './src/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: { extend: {} },
  plugins: [],
}

export default config
```

## Tailwind CSS: Recent Setup Notes (v4+)

Tailwind has introduced simplified install/config patterns in v4.x, including:
- PostCSS plugin package usage (`@tailwindcss/postcss`)
- CSS import form (`@import "tailwindcss";`)
- Optional CSS-first configuration via `@theme` in CSS for tokens (when adopting that approach)

This project may still be using the classic `tailwind.config.*` approach; prefer matching existing repo conventions.

## React + TypeScript: Common Hook Typing Patterns

From React’s TS guidance:
- `useState` often infers types from the initializer; you can provide explicit generics where needed.
- Union types are commonly used for status/state machines.

Examples:

```ts
const [enabled, setEnabled] = useState(false)

type Status = 'idle' | 'loading' | 'success' | 'error'
const [status, setStatus] = useState<Status>('idle')
```

From React’s `useRef` guidance:
- Read/write refs in effects and event handlers (not during render).

## If You Need More Context

If you want, add a second Context7 pull specifically for TypeScript (`tsconfig`, module resolution, Next.js typecheck/build behavior).
We intentionally limited Context7 queries to keep the lookup focused and fast for this pass.

