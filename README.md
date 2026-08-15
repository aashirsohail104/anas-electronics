# Anas Electronics — E-commerce Platform

Production-ready e-commerce storefront and order management system for **Anas Electronics**, specializing in battery chargers, solar charge controllers, power supplies, and inverters in Pakistan.

**Live:** https://anas-electronics.vercel.app

---

## Features

- **Product Catalog** — 33 products across 12 categories with filtering, sorting, search, and URL-synced sidebar state
- **Product Detail** — Image gallery, specifications, variants, sticky buy box, related products, quick view modal
- **Cart & Checkout** — Persistent cart drawer, quantity steppers, wishlist, product comparison, server-side order totals
- **Order Processing** — Vercel serverless API (`/api/orders`), Supabase Postgres, Resend email confirmation, idempotency keys
- **Pages** — Home, Catalog, Product Detail, Cart, Checkout, Wishlist, Compare, Search, Contact
- **Design System** — Tailwind CSS, design tokens (neon accents, premium shadows, Outfit font), reduced-motion support, WCAG AA contrast
- **Testing** — 160 tests (Vitest + React Testing Library) covering logic, hooks, components, API contracts, SEO

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React 18, TypeScript 5, Vite, React Router 6 |
| Styling | Tailwind CSS 3.4, PostCSS, CSS custom properties |
| Animation | Framer Motion 11 |
| Backend | Vercel Serverless Functions (Node.js) |
| Database | Supabase (PostgreSQL) |
| Email | Resend |
| Testing | Vitest, jsdom, React Testing Library |
| Linting | ESLint 9, TypeScript ESLint, Prettier |

---

## Project Structure

```
anas-electronics/
├── api/                    # Vercel serverless functions
│   ├── lib/
│   │   └── order-core.ts   # Pure order logic (validate, totals, email HTML)
│   ├── orders.ts           # POST /api/orders handler
│   ├── diag.ts             # Diagnostics endpoint
│   └── ping.ts             # Health check
├── config/                 # Build configuration
│   ├── tailwind.config.js
│   ├── tsconfig.json
│   └── vite.config.ts
├── data/                   # Static JSON data
│   ├── categories.json
│   ├── products.json
│   └── site.json
├── public/                 # Static assets
│   ├── favicon.svg
│   ├── og-image.png
│   ├── robots.txt
│   └── sitemap.xml
├── src/
│   ├── components/         # UI components (organized by domain)
│   │   ├── cart/
│   │   ├── catalog/
│   │   ├── commerce/
│   │   ├── compare/
│   │   ├── contact/
│   │   ├── home/
│   │   ├── layout/
│   │   ├── product/
│   │   ├── search/
│   │   └── ui/
│   ├── context/            # React context (CommerceContext)
│   ├── hooks/              # Custom hooks (useCart, useCompare, etc.)
│   ├── lib/                # Utilities & API clients
│   │   ├── api/
│   │   ├── logic/
│   │   ├── orders.ts
│   │   ├── seo/
│   │   └── image.ts
│   ├── pages/              # Route-level pages
│   ├── styles/             # Global CSS & tokens
│   ├── app.tsx             # App root with providers
│   └── main.tsx            # Entry point
├── supabase/
│   └── migrations/
│       └── 0001_orders.sql # Orders table + RLS + RPC
├── tests/
│   ├── fixtures/
│   ├── unit/               # 22 test files, 160 tests
│   ├── qa-report.md
│   └── setup.ts
├── specs/                  # Feature specifications
├── .github/                # (CI workflows can be added)
├── vercel.json             # SPA rewrite rules
├── package.json
└── package-lock.json
```

---

## Getting Started

### Prerequisites

- Node.js 20+
- npm 10+
- Supabase project (for orders backend)
- Resend account (for order emails)
- Vercel account (for deployment)

### Installation

```bash
# Clone the repository
git clone https://github.com/aashirsohail104/anas-electronics.git
cd anas-electronics

# Install dependencies
npm install

# Copy environment template
cp .env.example .env.local   # (create .env.example with required variables)
```

### Environment Variables

Create `.env.local` with:

```env
# Supabase
VITE_SUPABASE_URL=your_supabase_project_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key  # Server-only

# Resend (email)
RESEND_API_KEY=your_resend_api_key
EMAIL_FROM=orders@yourdomain.com
EMAIL_TO=anasrajputups@gmail.com

# Optional: Analytics, etc.
```

### Development

```bash
# Start dev server (Vite)
npm run dev

# Run tests (watch mode)
npm run test

# Run tests (CI mode)
npm run test:run

# Type-check
npm run typecheck

# Lint
npm run lint

# Format
npm run format
```

### Build

```bash
npm run build
# Output: dist/
```

### Preview Production Build

```bash
npm run preview
```

---

## Deployment

### Vercel (Recommended)

1. Connect this repository to Vercel
2. Configure environment variables in Vercel dashboard
3. Deploy — `vercel.json` handles SPA routing automatically

### Supabase Migration

```bash
# Apply orders table migration
supabase db push
# Or run manually:
# supabase/migrations/0001_orders.sql
```

---

## API Reference

### `POST /api/orders`

Create an order with idempotency.

**Request:**
```json
{
  "idempotencyKey": "uuid-v4",
  "email": "customer@example.com",
  "phone": "+92 300 1234567",
  "shippingAddress": "123 Street, City, Pakistan",
  "items": [
    { "productId": 1, "quantity": 2 },
    { "productId": 5, "variantId": "5-12v", "quantity": 1 }
  ]
}
```

**Response (201):**
```json
{
  "orderId": "AE-20260815-1001",
  "status": "created",
  "notification": "sent"
}
```

---

## Testing

```bash
# All tests
npm run test:run

# Specific test file
npx vitest run tests/unit/orders/order-core.test.ts

# With coverage
npx vitest run --coverage
```

**Current coverage:** 160 tests passing (logic, hooks, components, API, SEO)

---

## Design System

Design tokens defined in `src/styles/tokens.css`:

- **Colors** — `--neon-cyan`, `--neon-amber`, `--surface`, `--text-primary`, etc.
- **Shadows** — `--shadow-premium`, `--shadow-glow`
- **Typography** — `--font-display` (Outfit), fluid type scale
- **Motion** — `--duration-fast`, `--duration-normal`, `--ease-out-expo`
- **Reduced motion** — `@media (prefers-reduced-motion: reduce)`

---

## Accessibility

- Semantic HTML5 landmarks
- ARIA labels on interactive elements
- Focus-visible outlines
- Color contrast ≥ 4.5:1 (WCAG AA)
- Reduced motion support
- Keyboard-navigable modals, drawers, tabs

---

## Scripts Summary

| Command | Description |
|---------|-------------|
| `npm run dev` | Start Vite dev server |
| `npm run build` | Production build to `dist/` |
| `npm run preview` | Preview production build |
| `npm run test` | Watch-mode tests |
| `npm run test:run` | CI test run |
| `npm run lint` | ESLint check |
| `npm run typecheck` | TypeScript compile check |
| `npm run format` | Prettier format |

---

## License

Private — All rights reserved. Anas Electronics © 2026.

---

## Contact

**Anas Electronics**  
anasrajputups@gmail.com  
https://anas.electronics