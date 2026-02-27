---
name: frontend
description: Comprehensive frontend skill. Use when building UI (components, pages, apps), reviewing/auditing design, writing Tailwind CSS layouts, or implementing Next.js App Router patterns.
---

# Frontend Skill

Handles four modes automatically based on the request:

1. **Build UI** — Create distinctive, production-grade frontend interfaces
2. **Review UI** — Audit code against Web Interface Guidelines
3. **Tailwind Layouts** — Apply advanced CSS Grid / Flexbox patterns
4. **Next.js Patterns** — Apply App Router architecture and data fetching patterns

---

## Mode 1: Build UI

Triggered when the user asks to **create, design, or build** a component, page, app, poster, dashboard, or any visual interface.

This guides creation of distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. Implement real working code with exceptional attention to aesthetic details and creative choices.

### Design Thinking

Before coding, understand the context and commit to a BOLD aesthetic direction:
- **Purpose**: What problem does this interface solve? Who uses it?
- **Tone**: Pick an extreme: brutally minimal, maximalist chaos, retro-futuristic, organic/natural, luxury/refined, playful/toy-like, editorial/magazine, brutalist/raw, art deco/geometric, soft/pastel, industrial/utilitarian, etc.
- **Constraints**: Technical requirements (framework, performance, accessibility).
- **Differentiation**: What makes this UNFORGETTABLE? What's the one thing someone will remember?

**CRITICAL**: Choose a clear conceptual direction and execute it with precision. Bold maximalism and refined minimalism both work — the key is intentionality, not intensity.

Then implement working code (HTML/CSS/JS, React, Vue, etc.) that is:
- Production-grade and functional
- Visually striking and memorable
- Cohesive with a clear aesthetic point-of-view
- Meticulously refined in every detail

### Frontend Aesthetics Guidelines

- **Typography**: Choose fonts that are beautiful, unique, and interesting. Avoid generic fonts like Arial and Inter; opt instead for distinctive choices that elevate the frontend's aesthetics. Pair a distinctive display font with a refined body font.
- **Color & Theme**: Commit to a cohesive aesthetic. Use CSS variables for consistency. Dominant colors with sharp accents outperform timid, evenly-distributed palettes.
- **Motion**: Use animations for effects and micro-interactions. Prioritize CSS-only solutions for HTML. Use Motion library for React when available. Focus on high-impact moments: one well-orchestrated page load with staggered reveals creates more delight than scattered micro-interactions.
- **Spatial Composition**: Unexpected layouts. Asymmetry. Overlap. Diagonal flow. Grid-breaking elements. Generous negative space OR controlled density.
- **Backgrounds & Visual Details**: Create atmosphere and depth. Add contextual effects and textures — gradient meshes, noise textures, geometric patterns, layered transparencies, dramatic shadows, decorative borders, custom cursors, grain overlays.

NEVER use generic AI-generated aesthetics like overused font families (Inter, Roboto, Arial, system fonts), cliched color schemes (particularly purple gradients on white backgrounds), or predictable layouts that lack context-specific character.

**IMPORTANT**: Match implementation complexity to the aesthetic vision. Maximalist designs need elaborate code with extensive animations. Minimalist designs need restraint, precision, and careful attention to spacing and typography.

---

## Mode 2: Review UI

Triggered when the user asks to **review, audit, check accessibility, or check UX** of existing UI code.

1. Fetch the latest guidelines from the source URL below
2. Read the specified files (or prompt user for files/pattern)
3. Check against all rules in the fetched guidelines
4. Output findings in the terse `file:line` format

### Guidelines Source

Fetch fresh guidelines before each review:

```
https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md
```

Use WebFetch to retrieve the latest rules. The fetched content contains all the rules and output format instructions.

If no files are specified by the user, ask which files to review.

---

## Mode 3: Tailwind CSS Advanced Layouts

Triggered when the user is **writing Tailwind CSS** and needs Grid, Flexbox, Container Queries, or other layout patterns. Apply these patterns directly in the implementation.

### CSS Grid

```html
<!-- Holy Grail Layout -->
<div class="grid min-h-screen grid-rows-[auto_1fr_auto]">
  <header class="bg-white shadow">Header</header>
  <div class="grid grid-cols-[250px_1fr_300px]">
    <aside class="bg-gray-50 p-4">Sidebar</aside>
    <main class="p-6">Main Content</main>
    <aside class="bg-gray-50 p-4">Right Sidebar</aside>
  </div>
  <footer class="bg-gray-800 text-white">Footer</footer>
</div>

<!-- Responsive Holy Grail -->
<div class="grid min-h-screen grid-rows-[auto_1fr_auto]">
  <header>Header</header>
  <div class="grid grid-cols-1 md:grid-cols-[250px_1fr] lg:grid-cols-[250px_1fr_300px]">
    <aside class="order-2 md:order-1">Sidebar</aside>
    <main class="order-1 md:order-2">Main</main>
    <aside class="order-3 hidden lg:block">Right</aside>
  </div>
  <footer>Footer</footer>
</div>

<!-- Auto-fill responsive grid -->
<div class="grid grid-cols-[repeat(auto-fill,minmax(250px,1fr))] gap-6">
  <div class="bg-white rounded-lg shadow p-4">Card</div>
</div>
```

### Grid Template Areas

```css
@utility grid-areas-dashboard {
  grid-template-areas:
    "header header header"
    "nav main aside"
    "nav footer footer";
}
@utility area-header { grid-area: header; }
@utility area-nav    { grid-area: nav; }
@utility area-main   { grid-area: main; }
@utility area-aside  { grid-area: aside; }
@utility area-footer { grid-area: footer; }
```

### Flexbox Patterns

```html
<!-- Fixed + flexible -->
<div class="flex">
  <div class="w-64 shrink-0">Fixed 256px</div>
  <div class="flex-1 min-w-0">Flexible (can shrink)</div>
</div>

<!-- Prevent overflow with text -->
<div class="flex min-w-0">
  <div class="shrink-0">Icon</div>
  <div class="min-w-0 truncate">Very long text that should truncate</div>
</div>
```

### Container Queries

```html
<div class="@container">
  <div class="flex flex-col @md:flex-row @lg:grid @lg:grid-cols-3 gap-4">
    <div>Item 1</div>
    <div>Item 2</div>
    <div>Item 3</div>
  </div>
</div>
```

### Z-Index Management

```css
@theme {
  --z-dropdown: 100;
  --z-sticky: 200;
  --z-fixed: 300;
  --z-modal-backdrop: 400;
  --z-modal: 500;
  --z-popover: 600;
  --z-tooltip: 700;
  --z-toast: 800;
}
```

### Scroll Snap

```html
<!-- Horizontal carousel -->
<div class="flex snap-x snap-mandatory overflow-x-auto gap-4">
  <div class="snap-start shrink-0 w-80">Card 1</div>
  <div class="snap-start shrink-0 w-80">Card 2</div>
</div>

<!-- Full-page sections -->
<div class="h-screen snap-y snap-mandatory overflow-y-auto">
  <section class="h-screen snap-start">Section 1</section>
  <section class="h-screen snap-start">Section 2</section>
</div>
```

### Fluid Sizing

```html
<section class="py-[clamp(2rem,5vw,6rem)] px-[clamp(1rem,3vw,4rem)]">
  Content with responsive padding
</section>
```

### Layout Best Practices

- Prefer **Grid** for 2D layouts, **Flexbox** for 1D layouts
- Always add `min-w-0` to flex children that contain truncated text
- Use `scroll-mt-{n}` on anchor targets to offset for fixed headers
- Use `aspect-video`, `aspect-square`, or `aspect-[w/h]` for ratio containers

---

## Mode 4: Next.js App Router Patterns

Triggered when the user is **building or working on a Next.js application** using App Router (Next.js 14+).

### Rendering Decision

| Mode | Where | When to Use |
|---|---|---|
| **Server Components** | Server only | Data fetching, secrets, heavy computation |
| **Client Components** | Browser | Interactivity, hooks, browser APIs |
| **Static** | Build time | Content that rarely changes |
| **Dynamic** | Request time | Personalized or real-time data |
| **Streaming** | Progressive | Large pages, slow data sources |

**Rule**: Start with Server Components. Add `'use client'` only when needed.

### File Conventions

```
app/
├── layout.tsx       # Shared UI wrapper
├── page.tsx         # Route UI
├── loading.tsx      # Suspense fallback
├── error.tsx        # Error boundary
├── not-found.tsx    # 404 UI
├── route.ts         # API endpoint
└── template.tsx     # Re-mounted layout
```

### Server Component with Data Fetching

```typescript
// app/products/page.tsx
import { Suspense } from 'react'

export default async function ProductsPage({
  searchParams,
}: {
  searchParams: Promise<{ category?: string; page?: string }>
}) {
  const params = await searchParams
  return (
    <div className="flex gap-8">
      <FilterSidebar />
      <Suspense key={JSON.stringify(params)} fallback={<ProductListSkeleton />}>
        <ProductList category={params.category} page={Number(params.page) || 1} />
      </Suspense>
    </div>
  )
}
```

### Server Actions

```typescript
// app/actions/cart.ts
'use server'
import { revalidateTag } from 'next/cache'
import { cookies } from 'next/headers'
import { redirect } from 'next/navigation'

export async function addToCart(productId: string) {
  const cookieStore = await cookies()
  const sessionId = cookieStore.get('session')?.value
  if (!sessionId) redirect('/login')

  try {
    await db.cart.upsert({
      where: { sessionId_productId: { sessionId, productId } },
      update: { quantity: { increment: 1 } },
      create: { sessionId, productId, quantity: 1 },
    })
    revalidateTag('cart')
    return { success: true }
  } catch {
    return { error: 'Failed to add item to cart' }
  }
}
```

### Parallel Routes

```typescript
// app/dashboard/layout.tsx
export default function DashboardLayout({
  children, analytics, team,
}: {
  children: React.ReactNode
  analytics: React.ReactNode
  team: React.ReactNode
}) {
  return (
    <div className="dashboard-grid">
      <main>{children}</main>
      <aside>{analytics}</aside>
      <aside>{team}</aside>
    </div>
  )
}
```

### Streaming with Suspense

```typescript
export default async function ProductPage({ params }: { params: Promise<{ id: string }> }) {
  const { id } = await params
  const product = await getProduct(id) // Fast, blocking

  return (
    <div>
      <ProductHeader product={product} />
      <Suspense fallback={<ReviewsSkeleton />}>
        <Reviews productId={id} />        {/* Slow, streamed */}
      </Suspense>
      <Suspense fallback={<RecommendationsSkeleton />}>
        <Recommendations productId={id} /> {/* Slow, streamed */}
      </Suspense>
    </div>
  )
}
```

### Caching

```typescript
fetch(url, { cache: 'no-store' })                    // Always fresh
fetch(url, { cache: 'force-cache' })                 // Cache forever
fetch(url, { next: { revalidate: 60 } })             // ISR: 60s
fetch(url, { next: { tags: ['products'] } })         // Tag-based

// Invalidate
revalidateTag('products')
revalidatePath('/products')
```

### Route Handler

```typescript
// app/api/products/route.ts
import { NextRequest, NextResponse } from 'next/server'

export async function GET(request: NextRequest) {
  const category = request.nextUrl.searchParams.get('category')
  const products = await db.product.findMany({
    where: category ? { category } : undefined,
    take: 20,
  })
  return NextResponse.json(products)
}
```

### Dynamic Metadata + Static Params

```typescript
export async function generateMetadata({ params }: Props): Promise<Metadata> {
  const { slug } = await params
  const product = await getProduct(slug)
  return {
    title: product.name,
    description: product.description,
    openGraph: { images: [{ url: product.image, width: 1200, height: 630 }] },
  }
}

export async function generateStaticParams() {
  const products = await db.product.findMany({ select: { slug: true } })
  return products.map((p) => ({ slug: p.slug }))
}
```
