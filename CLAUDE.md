# CLAUDE.md - AI Agent Development Guide

> **FlashFusion AI-Powered Development Platform**
> This file provides context for Claude and other AI coding assistants working on this codebase.

---

## Project Overview

**FlashFusion** is a production-ready AI-powered development platform that generates full-stack applications from natural language descriptions. Built with React + TypeScript (frontend) and Express.js + PostgreSQL (backend), powered by OpenAI GPT-5 and DALL-E 3.

### Quick Stats
- **Completion**: ~55% (Active Development)
- **Architecture**: Monorepo with client/server/shared structure
- **Primary Language**: TypeScript 5.6+
- **Node Version**: 22.x
- **Package Manager**: npm 10.x+

---

## Repository Structure

```
FlashFusion/
├── client/                    # React 18 + Vite frontend
│   ├── src/
│   │   ├── components/        # Reusable UI components
│   │   │   ├── ui/            # Radix UI primitives (shadcn/ui)
│   │   │   ├── workflows/     # Workflow-specific components
│   │   │   └── auth/          # Authentication components
│   │   ├── pages/             # Route-level page components
│   │   │   └── workflows/     # Individual workflow pages
│   │   ├── hooks/             # Custom React hooks
│   │   ├── stores/            # Zustand state stores
│   │   ├── lib/               # Utilities and configs
│   │   ├── utils/             # Helper functions
│   │   ├── i18n/              # Internationalization
│   │   └── locales/           # Translation files
│   └── public/                # Static assets
├── server/                    # Express.js backend
│   ├── index.ts               # Server entry point
│   ├── routes.ts              # API route definitions
│   ├── storage.ts             # Data access layer (IStorage interface)
│   ├── db.ts                  # Drizzle ORM database connection
│   ├── stripe.ts              # Stripe payment integration
│   ├── generation.ts          # AI generation job queue
│   ├── openai.ts              # OpenAI client configuration
│   ├── rateLimit.ts           # Rate limiting middleware
│   ├── replitAuth.ts          # Replit OAuth setup
│   └── vite.ts                # Vite dev server integration
├── shared/                    # Shared code (client + server)
│   └── schema.ts              # Drizzle ORM schema + Zod types
├── tests/                     # E2E tests (Playwright)
│   └── e2e/                   # Test specifications
└── docs/                      # Additional documentation
```

---

## Key Technologies

### Frontend Stack
| Technology | Version | Purpose |
|------------|---------|---------|
| React | 18.3 | UI framework |
| TypeScript | 5.6 | Type safety |
| Vite | 5.4 | Build tool & dev server |
| Tailwind CSS | 3.4 | Styling |
| Radix UI | Latest | Accessible UI primitives |
| Framer Motion | 11.18 | Animations |
| Zustand | 5.0 | State management |
| React Query | 5.60 | Data fetching |
| Wouter | 3.3 | Lightweight routing |

### Backend Stack
| Technology | Version | Purpose |
|------------|---------|---------|
| Express.js | 4.21 | HTTP server |
| Drizzle ORM | 0.39 | Database ORM |
| PostgreSQL | 14+ | Database |
| Passport.js | 0.7 | Authentication |
| OpenAI SDK | 6.3 | AI generation |
| Stripe SDK | 19.1 | Payments |

---

## Development Commands

```bash
# Development
npm run dev              # Start dev server (localhost:5000)
npm run check            # TypeScript type checking

# Database
npm run db:push          # Push schema changes to database

# Testing
npm run test:e2e         # Run Playwright E2E tests
npm run test:e2e:ui      # Interactive test mode
npm run lighthouse       # Run Lighthouse CI

# Build & Deploy
npm run build            # Production build
npm start                # Start production server
```

---

## Core Patterns

### 1. Storage Interface Pattern
All data access goes through `IStorage` interface in `server/storage.ts`:
```typescript
// Use storage interface for all CRUD operations
import { storage } from "./storage";

const user = await storage.getUser(userId);
const workflow = await storage.createWorkflowRun(data);
```

### 2. Zod Schema Validation
All API inputs are validated with Zod schemas from `shared/schema.ts`:
```typescript
import { insertWorkflowRunSchema } from "@shared/schema";

const validated = insertWorkflowRunSchema.parse(req.body);
```

### 3. React Query for Data Fetching
Use React Query hooks for server state:
```typescript
const { data: user, isLoading } = useQuery({
  queryKey: ['/api/auth/user'],
  queryFn: fetchUser,
});
```

### 4. Feature Flags
Control feature rollout via `client/src/utils/featureFlags.ts`:
```typescript
import { featureFlags } from '@/utils/featureFlags';

if (featureFlags.PROMO_LAUNCH50) {
  // Show promo badge
}
```

---

## API Endpoints Reference

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/auth/user` | Get current user (dev bypass available) |

### Usage & Limits
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/usage/check` | Check usage vs limit |
| POST | `/api/usage/increment` | Increment usage counter |

### AI Generation
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/generate` | Create AI generation job |
| GET | `/api/generate/:jobId` | Get job status |
| POST | `/api/generate-code` | Generate code (SSE streaming) |
| POST | `/api/generate-image` | Generate image (SSE streaming) |

### Workflows
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/workflows` | Create workflow run |
| GET | `/api/workflows/:id` | Get workflow status |
| PATCH | `/api/workflows/:id` | Update workflow |

### Payments (Stripe)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/stripe/create-checkout-session` | Create checkout |
| POST | `/api/stripe/create-portal-session` | Customer portal |
| POST | `/api/stripe/webhook` | Stripe webhook handler |

---

## Database Schema (Key Tables)

### users
```sql
id              VARCHAR PRIMARY KEY
email           VARCHAR UNIQUE
firstName       VARCHAR
lastName        VARCHAR
profileImageUrl VARCHAR
plan            TEXT DEFAULT 'free'    -- free, pro, enterprise
role            TEXT DEFAULT 'user'
currentUsage    INTEGER DEFAULT 0
usageLimit      INTEGER DEFAULT 10
createdAt       TIMESTAMP
updatedAt       TIMESTAMP
```

### workflow_runs
```sql
id              VARCHAR PRIMARY KEY
userId          VARCHAR REFERENCES users(id)
workflowType    TEXT    -- ai-creation, publishing, commerce, etc.
status          TEXT DEFAULT 'in_progress'
currentStep     INTEGER DEFAULT 1
totalSteps      INTEGER
configuration   TEXT    -- JSON string
createdAt       TIMESTAMP
completedAt     TIMESTAMP
```

### generated_images
```sql
id              VARCHAR PRIMARY KEY
userId          VARCHAR REFERENCES users(id)
prompt          TEXT
style           TEXT    -- photorealistic, digital-art, sketch, etc.
model           TEXT DEFAULT 'dall-e-3'
imageUrl        TEXT
settings        TEXT    -- JSON
status          TEXT DEFAULT 'pending'
errorMessage    TEXT
createdAt       TIMESTAMP
```

---

## Common Tasks

### Adding a New API Endpoint
1. Add route in `server/routes.ts`
2. Add Zod schema in `shared/schema.ts` if needed
3. Add storage method in `server/storage.ts`
4. Add React Query hook in `client/src/hooks/`

### Adding a New Component
1. Create component in `client/src/components/`
2. Use Radix UI primitives from `client/src/components/ui/`
3. Follow accessibility patterns (ARIA labels, keyboard nav)
4. Add Playwright test in `tests/e2e/`

### Adding a New Workflow
1. Create page in `client/src/pages/workflows/`
2. Add route in `client/src/App.tsx`
3. Add workflow card in `client/src/pages/Workflows.tsx`
4. Implement backend logic in `server/routes.ts`

---

## Environment Variables

### Required
```bash
DATABASE_URL=postgresql://...     # Neon/Supabase connection
NODE_ENV=development              # development | production
```

### AI Features
```bash
OPENAI_API_KEY=sk-...             # OpenAI API key
```

### Payments
```bash
STRIPE_SECRET_KEY=sk_...          # Stripe secret key
VITE_STRIPE_PUBLISHABLE_KEY=pk_...# Stripe publishable key
STRIPE_WEBHOOK_SECRET=whsec_...   # Webhook signature
STRIPE_PRO_PRICE_ID=price_...     # Pro plan price ID
STRIPE_ENTERPRISE_PRICE_ID=price_...
```

---

## Code Style Guidelines

### TypeScript
- Strict mode enabled (`strict: true`)
- No `any` types - use proper types or `unknown`
- Use Zod for runtime validation

### React
- Functional components only
- Use hooks for state and side effects
- Follow accessibility patterns (WCAG 2.1 AA)

### Naming Conventions
- Components: `PascalCase` (e.g., `UserProfile.tsx`)
- Utilities: `camelCase` (e.g., `sanitizeHtml.ts`)
- Constants: `UPPER_SNAKE_CASE` (e.g., `API_BASE_URL`)
- CSS classes: `kebab-case` (Tailwind)

### Commit Messages
Follow Conventional Commits:
```
feat(auth): add Google OAuth integration
fix(pricing): correct pro plan pricing display
docs(readme): update installation instructions
```

---

## Known Issues & Gotchas

### 1. Demo User Initialization
The demo user in `server/storage.ts:588-617` uses deprecated `username` and `password` fields. These should be removed when transitioning to production OAuth only.

### 2. Feature Flags Hardcoded
Feature flags in `server/storage.ts` are hardcoded. For production, consider:
- Environment variable configuration
- Database-driven flags
- LaunchDarkly or similar service

### 3. Routes File Size
`server/routes.ts` is 677 lines. Consider modularizing:
- `/server/routes/auth.ts`
- `/server/routes/workflows.ts`
- `/server/routes/stripe.ts`
- `/server/routes/generation.ts`

### 4. Usage Credit Rollback
If AI generation fails after incrementing usage, credits are not rolled back. The `generate-image` endpoint handles this partially but needs improvement.

---

## Testing

### E2E Tests (Playwright)
```bash
npm run test:e2e         # Run all tests
npm run test:e2e:ui      # Interactive mode
npm run test:e2e:headed  # See browser
```

### Test Coverage
- `landing.spec.ts` - Landing page tests
- `pricing.spec.ts` - Pricing page tests
- `accessibility.spec.ts` - WCAG compliance
- `modals.spec.ts` - Modal interactions
- `404.spec.ts` - Error pages

---

## Performance Targets

| Metric | Target |
|--------|--------|
| LCP | ≤ 1.8s |
| CLS | ≤ 0.1 |
| FID | ≤ 100ms |
| Initial JS | ≤ 120KB gzipped |
| Lighthouse Score | ≥ 90 |

---

## Security Considerations

### Implemented
- CSRF protection (Passport.js)
- DOMPurify HTML sanitization
- Rate limiting middleware
- Parameterized queries (Drizzle ORM)
- Secure session cookies

### Required for Production
- CSP headers configuration
- HTTPS enforcement
- API key rotation strategy
- Audit logging

---

## Contact & Resources

- **Repository**: `/home/user/FlashFusion`
- **Main Docs**: `README.md`
- **Roadmap**: `ROADMAP.md`
- **Status**: `DEVELOPMENT_STATUS.md`
- **Changelog**: `CHANGELOG.md`

---

**Last Updated**: December 30, 2025
**Version**: 2.0.0
