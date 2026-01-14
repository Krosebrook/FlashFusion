# ⚡ FlashFusion

> Build Full-Stack Applications 10× Faster with AI

FlashFusion is a production-ready, cinematic AI-powered development platform that transforms ideas into complete full-stack applications in minutes. Built with modern web technologies and powered by advanced AI, FlashFusion streamlines the entire development lifecycle from ideation to deployment.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Node Version](https://img.shields.io/badge/node-%3E%3D%2022.x-brightgreen)](package.json)
[![Lighthouse Score](https://img.shields.io/badge/Lighthouse-90%2B-success)](lighthouserc.json)

---

## 📚 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Quick Start](#-quick-start)
- [Installation](#-installation)
- [Environment Variables](#-environment-variables)
- [Project Structure](#-project-structure)
- [Database Schema](#-database-schema)
- [API Documentation](#-api-documentation)
- [Development Workflow](#-development-workflow)
- [Design System](#-design-system)
- [Feature Flags](#-feature-flags)
- [Security](#-security)
- [Accessibility](#-accessibility)
- [Performance](#-performance)
- [Testing](#-testing)
- [Deployment](#-deployment)
- [Workflows](#-workflows)
- [AI Integration](#-ai-integration)
- [Payment Integration](#-payment-integration)
- [Analytics](#-analytics)
- [Internationalization](#-internationalization)
- [Troubleshooting](#-troubleshooting)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [License](#-license)

---

## ✨ Features

### Core Platform
- **🎨 Cinematic UI**: Gradient mesh background with grain overlay and parallax animation
- **🤖 AI-Powered Code Generation**: Generate complete applications with AI assistance
- **🖼️ AI Image Generation**: Create stunning visuals with DALL-E 3 integration
- **🔄 Guided Workflows**: 6 specialized workflows for different development needs
- **♿ Accessible**: WCAG 2.1 AA compliant with full keyboard navigation
- **🔒 Secure**: CSP-safe patterns, DOMPurify sanitization, safe iframes
- **🚄 Performant**: LCP ≤ 1.8s, CLS ≤ 0.1, initial JS ≤ 120KB gz
- **📱 Responsive**: Optimized for 375px, 768px, and 1440px viewports

### User Management & Authentication
- **🔐 Replit OAuth**: Seamless authentication with Replit integration
- **👤 User Profiles**: Complete profile management with avatars
- **📊 Usage Tracking**: Real-time monitoring of AI generation limits
- **💰 Subscription Plans**: Free, Pro, and Enterprise tiers

### AI & Generation
- **💬 AI Code Assistant**: Context-aware code generation and debugging
- **🎨 Image Generation**: Multiple styles (photorealistic, digital-art, sketch, cinematic, anime, fantasy)
- **📝 Project Templates**: Pre-built templates for common use cases
- **🔍 Smart Prompting**: Optimized prompt engineering for better results

### Development Tools
- **🛠️ 6 Specialized Workflows**:
  - AI Creation (code generation)
  - Publishing (deployment automation)
  - Commerce (e-commerce setup)
  - Analytics (tracking integration)
  - Security (vulnerability scanning)
  - Quality (code review & testing)
- **📊 Real-time Progress**: Live status updates during generation
- **💾 Project History**: Save and manage generated projects
- **🔄 Version Control**: Track changes and iterations

### Business Features
- **💳 Stripe Integration**: Complete payment processing
- **📈 Analytics Dashboard**: Privacy-first, consent-gated tracking
- **🎯 Feature Flags**: Controlled rollout system
- **🌍 i18n Ready**: Internationalization framework (stub)
- **🔔 Notifications**: In-app and email notifications
- **📧 Email Subscriptions**: Newsletter and updates management

### SEO & Marketing
- **🔍 SEO Optimized**: JSON-LD schemas, meta tags, OpenGraph support
- **🌙 Motion Aware**: Respects prefers-reduced-motion
- **📊 Performance Monitoring**: Lighthouse CI integration
- **🎁 Promotional Features**: Promo badges and launch campaigns
- **📱 PWA Support**: Progressive Web App capabilities (optional)

---

## 🛠️ Tech Stack

### Frontend
- **Framework**: React 18.3 with TypeScript 5.6
- **Build Tool**: Vite 5.4
- **Styling**: Tailwind CSS 3.4 with Tailwind CSS v4 Vite Plugin
- **Routing**: Wouter 3.3 (lightweight React Router alternative)
- **Animation**: Framer Motion 11.18
- **UI Components**: Radix UI primitives
- **State Management**: Zustand 5.0
- **Forms**: React Hook Form 7.55 with Zod validation
- **SEO**: react-helmet-async 2.0

### Backend
- **Runtime**: Node.js 22.x
- **Framework**: Express.js 4.21
- **Database**: PostgreSQL (Neon/Supabase)
- **ORM**: Drizzle ORM 0.39
- **Session**: express-session with connect-pg-simple
- **Authentication**: Passport.js with Replit OAuth

### AI Integration
- **OpenAI**: GPT-4 for code generation
- **DALL-E 3**: Image generation
- **OpenAI SDK**: openai 6.3

### Payment Processing
- **Stripe**: Backend SDK (stripe 19.1)
- **Stripe.js**: Frontend SDK (@stripe/stripe-js 8.1)

### Development Tools
- **TypeScript**: 5.6.3
- **Build**: esbuild 0.25, tsx 4.20
- **Testing**: Playwright 1.56
- **Performance**: Lighthouse CI
- **Database Migrations**: drizzle-kit 0.31

### Deployment
- **Platforms**: Vercel, Replit
- **Database**: Neon PostgreSQL, Supabase
- **CDN**: Vercel Edge Network
- **Monitoring**: Built-in analytics

---

## 🚀 Quick Start

Get FlashFusion running locally in under 2 minutes:

```bash
# Clone the repository
git clone https://github.com/ChaosClubCo/FlashFusion.git
cd FlashFusion

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Edit .env with your credentials

# Run database migrations
npm run db:push

# Start development server
npm run dev
```

Visit `http://localhost:5000` to see the app in action!

---

## 📦 Installation

### Prerequisites
- **Node.js**: ≥ 22.x ([Download](https://nodejs.org/))
- **PostgreSQL**: 14.x or higher (or use Neon/Supabase)
- **npm**: ≥ 10.x (comes with Node.js)

### Step-by-Step Installation

#### 1. Clone Repository
```bash
git clone https://github.com/ChaosClubCo/FlashFusion.git
cd FlashFusion
```

#### 2. Install Dependencies
```bash
npm install
```

#### 3. Database Setup

**Option A: Neon Database (Recommended)**
1. Create account at [neon.tech](https://neon.tech)
2. Create new project
3. Copy connection string
4. Add to `.env` as `DATABASE_URL`

**Option B: Supabase**
1. Create account at [supabase.com](https://supabase.com)
2. Create new project
3. Get connection string from Settings → Database
4. Use **Connection Pooling** URL with **Transaction mode**
5. Add to `.env`

**Option C: Local PostgreSQL**
```bash
# macOS (Homebrew)
brew install postgresql@14
brew services start postgresql@14

# Ubuntu/Debian
sudo apt-get install postgresql-14
sudo systemctl start postgresql

# Create database
createdb flashfusion

# Add to .env
DATABASE_URL=postgresql://localhost:5432/flashfusion
```

#### 4. Environment Variables

Create `.env` file:
```bash
cp .env.example .env
```

Edit `.env` with your credentials (see [Environment Variables](#-environment-variables) section).

#### 5. Run Migrations
```bash
npm run db:push
```

#### 6. Start Development Server
```bash
npm run dev
```

The app will be available at `http://localhost:5000`.

---

## 🔐 Environment Variables

FlashFusion requires several environment variables for full functionality. Create a `.env` file in the root directory:

### Required Variables

#### Database
```bash
# PostgreSQL connection string
DATABASE_URL=postgresql://user:password@host:port/database

# Example for Neon
DATABASE_URL=postgresql://user:password@ep-xxx.us-east-2.aws.neon.tech/neondb

# Example for Supabase (use Connection Pooling - Transaction mode)
DATABASE_URL=postgres://postgres.xxx:[password]@aws-0-us-east-1.pooler.supabase.com:6543/postgres
```

#### Server Configuration
```bash
NODE_ENV=development              # development | production
PORT=5000                          # Server port
```

### Optional Variables

#### OpenAI API (for AI features)
```bash
OPENAI_API_KEY=sk-...             # OpenAI API key
```

#### Stripe Payment (for subscriptions)
```bash
# Get from: https://dashboard.stripe.com/apikeys
STRIPE_SECRET_KEY=sk_test_...     # Server-side key
VITE_STRIPE_PUBLISHABLE_KEY=pk_test_...  # Client-side key

# Get from: https://dashboard.stripe.com/webhooks
STRIPE_WEBHOOK_SECRET=whsec_...   # Webhook signing secret

# Get from: https://dashboard.stripe.com/products
STRIPE_PRO_PRICE_ID=price_...     # Pro plan monthly price ID
STRIPE_ENTERPRISE_PRICE_ID=price_...  # Enterprise plan monthly price ID
```

#### Replit Auth (if deploying on Replit)
```bash
REPLIT_DOMAINS=your-repl.repl.co  # Your Replit domain
```

#### Supabase (if using Supabase)
```bash
SUPABASE_URL=https://xxx.supabase.co
SUPABASE_ANON_KEY=eyJhbGci...     # Public anon key
SUPABASE_SERVICE_ROLE_KEY=eyJhbGci...  # Service role key (keep secret!)
```

### Variable Reference

| Variable | Required | Purpose | Where to Get |
|----------|----------|---------|--------------|
| `DATABASE_URL` | ✅ Yes | Database connection | Neon/Supabase dashboard |
| `NODE_ENV` | ✅ Yes | Environment mode | Set manually |
| `PORT` | ❌ No | Server port (default: 5000) | Set manually |
| `OPENAI_API_KEY` | 🟡 For AI | AI code/image generation | [OpenAI Platform](https://platform.openai.com/api-keys) |
| `STRIPE_SECRET_KEY` | 🟡 For payments | Payment processing | [Stripe Dashboard](https://dashboard.stripe.com/apikeys) |
| `VITE_STRIPE_PUBLISHABLE_KEY` | 🟡 For payments | Client-side Stripe | [Stripe Dashboard](https://dashboard.stripe.com/apikeys) |
| `STRIPE_WEBHOOK_SECRET` | 🟡 For payments | Webhook verification | [Stripe Webhooks](https://dashboard.stripe.com/webhooks) |
| `STRIPE_PRO_PRICE_ID` | 🟡 For payments | Pro plan pricing | [Stripe Products](https://dashboard.stripe.com/products) |
| `STRIPE_ENTERPRISE_PRICE_ID` | 🟡 For payments | Enterprise plan pricing | [Stripe Products](https://dashboard.stripe.com/products) |

**Security Note**: Never commit `.env` to version control. The `.env.example` file shows the required format without sensitive values.

---

## 📂 Project Structure

```
FlashFusion/
├── client/                      # Frontend React application
│   ├── public/                  # Static assets
│   │   ├── icons/              # App icons and favicons
│   │   ├── og/                 # OpenGraph images
│   │   └── grain.png           # Texture overlay
│   └── src/
│       ├── components/          # React components
│       │   ├── ui/             # Radix UI components
│       │   ├── auth/           # Authentication components
│       │   ├── workflows/      # Workflow components
│       │   ├── Background.tsx  # Animated background
│       │   ├── Hero.tsx        # Landing hero section
│       │   ├── Features.tsx    # Features showcase
│       │   ├── Metrics.tsx     # Stats display
│       │   ├── BuildProcess.tsx # Build timeline
│       │   ├── Navigation.tsx  # Site navigation
│       │   ├── ConsentBanner.tsx # GDPR consent
│       │   ├── LimitReachedModal.tsx # Usage limit modal
│       │   ├── CheckoutButton.tsx # Stripe checkout
│       │   └── ...
│       ├── pages/               # Route pages
│       │   ├── Landing.tsx     # Homepage
│       │   ├── Dashboard.tsx   # User dashboard
│       │   ├── Pricing.tsx     # Pricing page
│       │   ├── Workflows.tsx   # Workflow selection
│       │   ├── ImageGeneration.tsx # AI image generation
│       │   ├── Privacy.tsx     # Privacy policy
│       │   ├── Terms.tsx       # Terms of service
│       │   ├── Status.tsx      # System status
│       │   ├── QA.tsx          # Q&A page
│       │   ├── Billing.tsx     # Billing management
│       │   ├── Referrals.tsx   # Referral program
│       │   ├── NotFound.tsx    # 404 page
│       │   └── workflows/      # Individual workflow pages
│       │       ├── AICreation.tsx
│       │       ├── Publishing.tsx
│       │       ├── Commerce.tsx
│       │       ├── Analytics.tsx
│       │       ├── Security.tsx
│       │       └── Quality.tsx
│       ├── hooks/               # Custom React hooks
│       │   └── use-mobile.tsx  # Mobile detection
│       ├── lib/                 # Libraries & configs
│       │   ├── utils.ts        # Utility functions
│       │   └── stripe.ts       # Stripe client utilities
│       ├── utils/               # Utility modules
│       │   ├── sanitize.ts     # DOMPurify wrapper
│       │   ├── events.ts       # Analytics tracking
│       │   ├── featureFlags.ts # Feature flag config
│       │   └── i18n.ts         # Internationalization
│       ├── i18n/                # Translation files
│       │   └── index.tsx       # i18n configuration
│       ├── App.tsx              # Root component
│       └── main.tsx             # Application entry
│
├── server/                      # Backend Express application
│   ├── index.ts                 # Server entry point
│   ├── routes.ts                # API route definitions
│   ├── db.ts                    # Database connection
│   ├── storage.ts               # Data access layer
│   ├── stripe.ts                # Stripe integration
│   ├── generation.ts            # AI generation logic
│   ├── openai.ts                # OpenAI client
│   ├── rateLimit.ts             # Rate limiting middleware
│   ├── replitAuth.ts            # Replit OAuth setup
│   └── vite.ts                  # Vite dev server integration
│
├── shared/                      # Shared code (client + server)
│   └── schema.ts                # Database schema & types
│
├── tests/                       # E2E tests
│   └── e2e/                     # Playwright tests
│
├── docs/                        # Documentation
│
├── scripts/                     # Build & utility scripts
│
├── .github/                     # GitHub configuration
│   └── workflows/               # GitHub Actions
│       └── lighthouse-ci.yml   # Lighthouse CI workflow
│
├── .env.example                 # Environment template
├── .gitignore                   # Git ignore rules
├── package.json                 # Dependencies & scripts
├── tsconfig.json                # TypeScript config
├── vite.config.ts               # Vite configuration
├── tailwind.config.ts           # Tailwind CSS config
├── drizzle.config.ts            # Drizzle ORM config
├── playwright.config.ts         # Playwright config
├── lighthouserc.json            # Lighthouse CI config
├── components.json              # shadcn/ui config
└── README.md                    # This file
```

### Key Directories

- **`client/src/components/`**: Reusable UI components organized by feature
- **`client/src/pages/`**: Route-level page components
- **`server/`**: Backend API, authentication, and business logic
- **`shared/`**: TypeScript types and schemas shared between client and server
- **`tests/`**: End-to-end tests with Playwright

---

## 💾 Database Schema

FlashFusion uses PostgreSQL with Drizzle ORM. The schema includes:

### Core Tables

#### `users`
User accounts and subscription information.
```typescript
{
  id: varchar (primary key, UUID)
  email: varchar (unique)
  firstName: varchar
  lastName: varchar
  profileImageUrl: varchar
  plan: text (free | pro | enterprise)
  role: text (user | admin)
  currentUsage: integer
  usageLimit: integer
  createdAt: timestamp
  updatedAt: timestamp
}
```

#### `sessions`
Session storage for Replit Auth.
```typescript
{
  sid: varchar (primary key)
  sess: jsonb
  expire: timestamp
}
```

### AI & Generation Tables

#### `generation_jobs`
Tracks AI code generation requests.
```typescript
{
  id: varchar (primary key)
  userId: varchar (foreign key → users)
  status: text (pending | processing | completed | failed)
  prompt: text
  result: text (JSON)
  errorMessage: text
  createdAt: timestamp
  completedAt: timestamp
}
```

#### `generated_images`
Stores AI-generated images.
```typescript
{
  id: varchar (primary key)
  userId: varchar (foreign key → users)
  prompt: text
  style: text (photorealistic | digital-art | sketch | cinematic | anime | fantasy)
  model: text (dall-e-3 | stable-diffusion | etc.)
  imageUrl: text
  settings: text (JSON)
  status: text (pending | generating | completed | failed)
  errorMessage: text
  createdAt: timestamp
}
```

#### `generated_projects`
Stores complete AI-generated projects.
```typescript
{
  id: varchar (primary key)
  userId: varchar (foreign key → users)
  workflowRunId: varchar (foreign key → workflow_runs)
  title: text
  description: text
  projectType: text
  files: text (JSON array of {path, content})
  metadata: text (JSON)
  createdAt: timestamp
}
```

### Workflow Tables

#### `workflow_runs`
Tracks workflow executions.
```typescript
{
  id: varchar (primary key)
  userId: varchar (foreign key → users)
  workflowType: text (ai-creation | publishing | commerce | analytics | security | quality)
  status: text (in_progress | completed | failed)
  currentStep: integer
  totalSteps: integer
  configuration: text (JSON)
  createdAt: timestamp
  completedAt: timestamp
}
```

#### `workflow_steps`
Individual steps within workflows.
```typescript
{
  id: varchar (primary key)
  workflowRunId: varchar (foreign key → workflow_runs)
  stepNumber: integer
  stepName: text
  status: text (pending | active | completed | skipped)
  data: text (JSON)
  completedAt: timestamp
}
```

#### `workflow_results`
Final workflow outputs.
```typescript
{
  id: varchar (primary key)
  workflowRunId: varchar (foreign key → workflow_runs)
  resultType: text (code | deployment | analytics | security_report | quality_report)
  resultData: text (JSON)
  files: text (JSON array)
  metrics: text (JSON)
  createdAt: timestamp
}
```

### Analytics & Tracking

#### `analytics_events`
Privacy-first event tracking.
```typescript
{
  id: varchar (primary key)
  name: text
  route: text
  props: text (JSON)
  timestamp: timestamp
  consentGiven: boolean
}
```

#### `email_subscriptions`
Newsletter subscriptions.
```typescript
{
  id: varchar (primary key)
  email: text (unique)
  subscribedAt: timestamp
}
```

#### `rate_limits`
API rate limiting.
```typescript
{
  id: varchar (primary key)
  userId: varchar (foreign key → users)
  requestCount: integer
  windowStart: timestamp
}
```

### Database Migrations

```bash
# Push schema changes to database
npm run db:push

# Generate migration files
npx drizzle-kit generate

# Apply migrations
npx drizzle-kit migrate
```

---

## 🌐 API Documentation

### Authentication

All authenticated routes require a valid session cookie.

#### Check Authentication Status
```http
GET /api/user
```

**Response**:
```json
{
  "id": "user-123",
  "email": "user@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "plan": "pro",
  "currentUsage": 25,
  "usageLimit": 100
}
```

### AI Generation

#### Generate Code
```http
POST /api/generate
Content-Type: application/json

{
  "prompt": "Create a React todo app with TypeScript",
  "userId": "user-123"
}
```

**Response**:
```json
{
  "jobId": "job-456",
  "status": "processing"
}
```

#### Check Generation Status
```http
GET /api/generate/:jobId
```

**Response**:
```json
{
  "id": "job-456",
  "status": "completed",
  "result": {
    "files": [...],
    "structure": {...}
  }
}
```

#### Generate Image
```http
POST /api/generate-image
Content-Type: application/json

{
  "prompt": "A futuristic cityscape at sunset",
  "style": "photorealistic",
  "userId": "user-123"
}
```

**Response**:
```json
{
  "id": "img-789",
  "imageUrl": "https://...",
  "status": "completed"
}
```

### Workflows

#### Start Workflow
```http
POST /api/workflows/start
Content-Type: application/json

{
  "workflowType": "ai-creation",
  "userId": "user-123",
  "configuration": {
    "projectType": "webapp",
    "features": ["auth", "database"]
  }
}
```

**Response**:
```json
{
  "runId": "run-101",
  "status": "in_progress",
  "currentStep": 1,
  "totalSteps": 5
}
```

#### Get Workflow Status
```http
GET /api/workflows/:runId
```

**Response**:
```json
{
  "id": "run-101",
  "status": "completed",
  "currentStep": 5,
  "totalSteps": 5,
  "results": {...}
}
```

### Stripe Integration

#### Create Checkout Session
```http
POST /api/stripe/create-checkout-session
Content-Type: application/json

{
  "plan": "pro",
  "userId": "user-123",
  "successUrl": "https://app.com/success",
  "cancelUrl": "https://app.com/pricing"
}
```

**Response**:
```json
{
  "sessionId": "cs_test_..."
}
```

#### Create Customer Portal Session
```http
POST /api/stripe/create-portal-session
Content-Type: application/json

{
  "userId": "user-123",
  "returnUrl": "https://app.com/dashboard"
}
```

**Response**:
```json
{
  "url": "https://billing.stripe.com/session/..."
}
```

#### Webhook Endpoint
```http
POST /api/stripe/webhook
Headers: stripe-signature
```

Handles events:
- `checkout.session.completed`
- `customer.subscription.created`
- `customer.subscription.updated`
- `customer.subscription.deleted`
- `invoice.payment_succeeded`
- `invoice.payment_failed`

### Analytics

#### Track Event
```http
POST /api/analytics/track
Content-Type: application/json

{
  "name": "cta_click",
  "route": "/pricing",
  "props": { "plan": "pro" },
  "consentGiven": true
}
```

**Response**:
```json
{
  "success": true,
  "eventId": "evt-202"
}
```

### Rate Limiting

All API endpoints are rate-limited:
- **Free Plan**: 10 requests/hour
- **Pro Plan**: 100 requests/hour
- **Enterprise Plan**: Unlimited

Rate limit headers included in responses:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1234567890
```

---

## 💻 Development Workflow

### Available Scripts

```bash
# Development
npm run dev              # Start dev server (client + server)
npm run check            # TypeScript type checking

# Building
npm run build            # Build for production

# Testing
npm run test:e2e         # Run Playwright tests
npm run test:e2e:ui      # Run tests in UI mode
npm run test:e2e:headed  # Run tests in headed mode
npm run test:e2e:report  # Show test report

# Performance
npm run lighthouse       # Run Lighthouse CI
npm run lighthouse:desktop  # Desktop performance test
npm run lighthouse:mobile   # Mobile performance test
npm run perf:analyze     # Analyze bundle size
npm run perf:report      # Generate performance report

# Database
npm run db:push          # Push schema changes to database
```

### Development Server

The development server runs on `http://localhost:5000` and includes:
- Hot Module Replacement (HMR) for React
- API server with live reload
- WebSocket support
- Source maps for debugging

### Code Style

- **TypeScript**: Strict mode enabled
- **ESLint**: React and TypeScript rules
- **Prettier**: Automatic code formatting
- **Naming Conventions**:
  - Components: PascalCase (`UserProfile.tsx`)
  - Utilities: camelCase (`sanitizeHtml.ts`)
  - Constants: UPPER_SNAKE_CASE (`API_BASE_URL`)
  - CSS: kebab-case (`bg-primary`)

### Git Workflow

1. Create feature branch: `git checkout -b feature/new-feature`
2. Make changes and commit: `git commit -m "feat: add new feature"`
3. Push to remote: `git push origin feature/new-feature`
4. Create pull request
5. Wait for CI checks (Lighthouse, tests)
6. Merge to main after approval

### Commit Message Format

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Code style changes
- `refactor`: Code refactoring
- `perf`: Performance improvement
- `test`: Add/update tests
- `chore`: Build process or tooling

**Examples**:
```
feat(auth): add Google OAuth integration
fix(pricing): correct pro plan pricing display
docs(readme): update installation instructions
perf(images): lazy load below-fold images
```

---

## 🎨 Design System

### Colors

#### Primary Palette
```css
--primary: hsl(24 95% 53%)        /* Orange - CTAs, focus states */
--primary-foreground: hsl(0 0% 100%)

--background: rgba(14, 14, 16, 0.85)  /* Dark with transparency */
--foreground: hsl(0 0% 98%)
```

#### Gradient Mesh
```css
background: radial-gradient(
  at 27% 37%,
  hsla(24, 95%, 53%, 0.4) 0px,    /* Orange */
  transparent 50%
),
radial-gradient(
  at 97% 21%,
  hsla(180, 88%, 65%, 0.3) 0px,   /* Cyan */
  transparent 50%
),
radial-gradient(
  at 52% 99%,
  hsla(328, 90%, 60%, 0.3) 0px,   /* Magenta */
  transparent 50%
);
```

#### Semantic Colors
```css
--success: hsl(142 76% 36%)
--warning: hsl(38 92% 50%)
--error: hsl(0 84% 60%)
--info: hsl(199 89% 48%)
```

### Typography

#### Font Family
```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
```

#### Font Weights
- **Regular**: 400 (body text)
- **Medium**: 500 (subheadings)
- **Semibold**: 600 (buttons)
- **Bold**: 700 (headings)
- **Extrabold**: 800 (hero titles)
- **Black**: 900 (emphasis)

#### Type Scale
```css
.text-xs:   0.75rem (12px)
.text-sm:   0.875rem (14px)
.text-base: 1rem (16px)
.text-lg:   1.125rem (18px)
.text-xl:   1.25rem (20px)
.text-2xl:  1.5rem (24px)
.text-3xl:  1.875rem (30px)
.text-4xl:  2.25rem (36px)
.text-5xl:  3rem (48px)
.text-6xl:  3.75rem (60px)
.text-7xl:  4.5rem (72px)
```

### Spacing

Tailwind's default spacing scale (1 unit = 0.25rem = 4px):
```
0, 1, 2, 3, 4, 5, 6, 8, 10, 12, 16, 20, 24, 32, 40, 48, 56, 64
```

### Breakpoints

```css
sm:  640px   /* Mobile landscape */
md:  768px   /* Tablet */
lg:  1024px  /* Desktop */
xl:  1280px  /* Large desktop */
2xl: 1536px  /* Extra large desktop */
```

### Focus States

All interactive elements must have visible focus states:
```css
.focus-visible:ring-2
.focus-visible:ring-primary
.focus-visible:ring-offset-2
```

### Animations

#### Durations
```css
--duration-fast:   150ms
--duration-normal: 300ms
--duration-slow:   500ms
```

#### Easing
```css
--ease-in-out: cubic-bezier(0.4, 0, 0.2, 1)
--ease-bounce: cubic-bezier(0.68, -0.55, 0.265, 1.55)
```

### Component Guidelines

- Minimum touch target: 44×44px (WCAG AAA)
- Border radius: 0.5rem (8px) for cards, 0.25rem (4px) for buttons
- Shadow: Consistent depth with Tailwind shadow utilities
- Transitions: Always include for hover/focus states

---

## 🎯 Feature Flags

Control feature rollout with feature flags in `client/src/utils/featureFlags.ts`:

```typescript
export const featureFlags = {
  PROMO_LAUNCH50: true,         // 50% off launch promo
  PWA_ENABLED: false,            // Progressive Web App
  I18N_ENABLED: false,           // Internationalization
  UPGRADE_MODAL_V2: false,       // New upgrade modal design
  REFERRAL_ENABLED: false,       // Referral program
  AGENT_TEASERS_ENABLED: false,  // AI agent teasers
};
```

### Usage

```typescript
import { featureFlags } from '@/utils/featureFlags';

function PricingPage() {
  return (
    <>
      {featureFlags.PROMO_LAUNCH50 && (
        <PromoBadge>50% OFF - Limited Time!</PromoBadge>
      )}
      {/* ... */}
    </>
  );
}
```

### Enabling Features

To enable a feature:
1. Set flag to `true` in `featureFlags.ts`
2. Commit and deploy
3. Monitor for issues
4. Keep flag for gradual rollout or remove after stable

---

## 🔐 Security

### Content Security Policy

Recommended CSP headers for production:

```http
Content-Security-Policy:
  default-src 'self';
  script-src 'self';
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
  font-src 'self' https://fonts.gstatic.com;
  img-src 'self' data: https:;
  connect-src 'self' wss: https:;
  frame-src https://www.youtube-nocookie.com https://player.vimeo.com https://checkout.stripe.com;

X-Frame-Options: DENY
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

### Input Sanitization

All user input is sanitized using DOMPurify:

```typescript
import { sanitizeHtml } from '@/utils/sanitize';

const cleanHtml = sanitizeHtml(userInput);
```

### Safe Iframes

Only allow trusted iframe sources:

```typescript
const ALLOWED_IFRAME_HOSTS = [
  'https://www.youtube-nocookie.com',
  'https://player.vimeo.com',
  'https://checkout.stripe.com',
];
```

### Secret Management

- ✅ Never commit secrets to Git
- ✅ Use environment variables only
- ✅ Add `.env` to `.gitignore`
- ✅ Rotate API keys regularly
- ✅ Use different keys for dev/staging/production
- ✅ Server-only secrets (never expose to client)

### Authentication

- Session-based authentication with express-session
- Secure cookies with httpOnly and sameSite flags
- CSRF protection enabled
- Rate limiting on authentication endpoints

### Database Security

- Parameterized queries (Drizzle ORM prevents SQL injection)
- Row Level Security (RLS) enabled on Supabase tables
- Connection pooling for better resource management
- Encrypted connections (SSL/TLS)

---

## ♿ Accessibility

FlashFusion is WCAG 2.1 AA compliant.

### Features

- ✅ **Contrast Ratio**: ≥ 4.5:1 for all text
- ✅ **Keyboard Navigation**: Full Tab, Enter, Esc support
- ✅ **Focus Management**: Visible focus indicators on all interactive elements
- ✅ **Focus Trap**: Modals trap focus and return on close
- ✅ **Skip Link**: Jump to main content
- ✅ **ARIA Labels**: All interactive elements labeled
- ✅ **ARIA Live Regions**: Dynamic content announcements
- ✅ **Screen Reader**: Tested with VoiceOver and NVDA
- ✅ **Reduced Motion**: Respects `prefers-reduced-motion`
- ✅ **Semantic HTML**: Proper heading hierarchy, landmarks
- ✅ **Form Labels**: All inputs have associated labels
- ✅ **Error Messages**: Clear, descriptive error messages

### Testing Checklist

Before each release:

- [ ] Run Axe DevTools on all pages
- [ ] Tab through entire page (verify focus visible)
- [ ] Test with screen reader (VoiceOver/NVDA)
- [ ] Verify skip link works
- [ ] Test modal focus trap (Tab, Shift+Tab, Esc)
- [ ] Enable `prefers-reduced-motion` and verify animations stop
- [ ] Check color contrast with WebAIM tool
- [ ] Test keyboard-only navigation
- [ ] Verify form validation messages
- [ ] Test with 200% zoom level

### Keyboard Shortcuts

- **Tab**: Navigate forward
- **Shift+Tab**: Navigate backward
- **Enter/Space**: Activate buttons/links
- **Esc**: Close modals and dropdowns
- **Arrow Keys**: Navigate within components (tabs, menus)

---

## 🚄 Performance

### Performance Targets

- **LCP** (Largest Contentful Paint): ≤ 1.8s
- **FID** (First Input Delay): ≤ 100ms
- **CLS** (Cumulative Layout Shift): ≤ 0.1
- **TTI** (Time to Interactive): ≤ 1.8s
- **Bundle Size**: ≤ 120KB gzipped (initial)
- **Lighthouse Score**: ≥ 90 (Performance, SEO, Accessibility)

### Optimization Techniques

#### 1. Code Splitting
```typescript
// Lazy-load routes
const Dashboard = lazy(() => import('@/pages/Dashboard'));
const Pricing = lazy(() => import('@/pages/Pricing'));
```

#### 2. Image Optimization
- WebP format with fallbacks
- Responsive images with `srcset`
- Lazy loading for below-fold images
- Proper sizing to prevent CLS

```html
<img
  src="image.webp"
  srcset="image-400w.webp 400w, image-800w.webp 800w"
  sizes="(max-width: 768px) 100vw, 50vw"
  loading="lazy"
  decoding="async"
  alt="Description"
/>
```

#### 3. Font Loading
```html
<link
  rel="preconnect"
  href="https://fonts.googleapis.com"
/>
<link
  rel="preconnect"
  href="https://fonts.gstatic.com"
  crossorigin
/>
<link
  href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap"
  rel="stylesheet"
/>
```

#### 4. Caching Strategy
```
/assets/*    → Cache-Control: public, max-age=31536000, immutable
/*.html, /   → Cache-Control: public, max-age=0, stale-while-revalidate=300
```

#### 5. Bundle Analysis
```bash
npm run build
npm run perf:analyze
```

### Monitoring

- Lighthouse CI runs on every pull request
- Real User Monitoring (RUM) with Vercel Analytics
- Core Web Vitals tracking
- Error monitoring and alerting

---

## 🧪 Testing

### End-to-End Testing (Playwright)

#### Installation
```bash
npm install -D @playwright/test
npx playwright install
```

#### Running Tests
```bash
# Run all tests
npm run test:e2e

# Run in UI mode (interactive)
npm run test:e2e:ui

# Run in headed mode (see browser)
npm run test:e2e:headed

# Show test report
npm run test:e2e:report
```

#### Writing Tests

Example test (`tests/e2e/landing.spec.ts`):
```typescript
import { test, expect } from '@playwright/test';

test('landing page loads correctly', async ({ page }) => {
  await page.goto('/');

  // Check hero heading
  await expect(page.getByRole('heading', { name: /build apps/i })).toBeVisible();

  // Check CTA button
  await expect(page.getByRole('button', { name: /start building/i })).toBeVisible();

  // Click CTA
  await page.getByRole('button', { name: /start building/i }).click();
});
```

### Smoke Test Checklist

1. **Landing Page**
   - [ ] Hero section renders
   - [ ] Metrics display correct values
   - [ ] Features grid loads
   - [ ] CTA buttons work
   - [ ] Email subscription form works

2. **Pricing Page**
   - [ ] All 3 tiers display
   - [ ] Promo badge shows (if enabled)
   - [ ] Checkout buttons redirect correctly
   - [ ] Feature lists render

3. **Dashboard**
   - [ ] User info displays
   - [ ] Usage stats show correctly
   - [ ] Quick actions work
   - [ ] Recent projects load

4. **Workflows**
   - [ ] All 6 workflows listed
   - [ ] Workflow wizard opens
   - [ ] Steps progress correctly
   - [ ] Results display properly

5. **Accessibility**
   - [ ] Skip link works
   - [ ] Keyboard navigation functions
   - [ ] Focus trap in modals
   - [ ] Screen reader announcements

---

## 🚀 Deployment

### Vercel Deployment (Recommended)

#### Quick Deploy

1. **Connect Repository**
   - Go to [vercel.com/new](https://vercel.com/new)
   - Import GitHub repository
   - Vercel auto-detects Vite + Express

2. **Add Environment Variables**
   - Go to Settings → Environment Variables
   - Add all required variables (see [Environment Variables](#-environment-variables))
   - Select all environments: Production, Preview, Development

3. **Deploy**
   - Push to main branch
   - Vercel automatically deploys

#### Build Configuration

Vercel should auto-detect, but verify:
- **Framework Preset**: Vite
- **Build Command**: `npm run build`
- **Output Directory**: `dist`
- **Install Command**: `npm install`
- **Node.js Version**: 22.x

#### Custom Domain

1. Go to Settings → Domains
2. Add your domain
3. Configure DNS (A or CNAME record)
4. Wait for SSL certificate provisioning

#### Environment-Specific Variables

```bash
# Production
NODE_ENV=production
DATABASE_URL=postgres://production-db-url

# Preview (for pull requests)
NODE_ENV=development
DATABASE_URL=postgres://staging-db-url
```

### Replit Deployment

1. Fork or import repository
2. Add environment variables to Secrets
3. Click "Run" button
4. App runs on `https://your-repl.repl.co`

### Manual Deployment

#### Build for Production
```bash
npm run build
```

#### Start Production Server
```bash
npm start
```

#### Using PM2 (Process Manager)
```bash
# Install PM2
npm install -g pm2

# Start app
pm2 start dist/index.js --name flashfusion

# View logs
pm2 logs flashfusion

# Restart
pm2 restart flashfusion

# Save configuration
pm2 save
pm2 startup
```

### Docker Deployment

Create `Dockerfile`:
```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 5000

CMD ["npm", "start"]
```

Build and run:
```bash
docker build -t flashfusion .
docker run -p 5000:5000 --env-file .env flashfusion
```

### Deployment Checklist

Before going live:

- [ ] All environment variables configured
- [ ] Database migrations run
- [ ] Stripe webhook endpoint configured
- [ ] OpenAI API key has sufficient credits
- [ ] SSL certificate active (HTTPS)
- [ ] Custom domain configured (if applicable)
- [ ] Error monitoring set up
- [ ] Analytics tracking verified
- [ ] Performance targets met (Lighthouse ≥ 90)
- [ ] Accessibility audit passed
- [ ] Security headers configured
- [ ] Rate limiting enabled
- [ ] Backup strategy in place
- [ ] Monitoring and alerting configured

---

## 🔄 Workflows

FlashFusion includes 6 specialized AI-powered workflows:

### 1. AI Creation Workflow
**Purpose**: Generate complete applications from natural language descriptions.

**Steps**:
1. Describe your project
2. Select tech stack
3. Configure features
4. Generate code
5. Review and download

**Output**: Complete project with files, structure, and documentation.

### 2. Publishing Workflow
**Purpose**: Deploy applications to hosting platforms.

**Steps**:
1. Connect GitHub account
2. Select deployment platform (Vercel, Netlify, Railway, etc.)
3. Configure environment variables
4. Set up custom domain
5. Deploy and monitor

**Output**: Live application with deployment URL.

### 3. Commerce Workflow
**Purpose**: Add e-commerce functionality to applications.

**Steps**:
1. Select commerce features (products, cart, checkout)
2. Configure payment provider (Stripe)
3. Set up inventory management
4. Generate commerce code
5. Test checkout flow

**Output**: Full e-commerce integration with payment processing.

### 4. Analytics Workflow
**Purpose**: Integrate analytics and tracking.

**Steps**:
1. Select analytics provider (Google Analytics, Plausible, etc.)
2. Configure event tracking
3. Set up conversion goals
4. Generate tracking code
5. Verify implementation

**Output**: Complete analytics setup with dashboard.

### 5. Security Workflow
**Purpose**: Scan and fix security vulnerabilities.

**Steps**:
1. Upload project files
2. Run security scan
3. Review vulnerability report
4. Apply automated fixes
5. Verify improvements

**Output**: Security report with fixes and recommendations.

### 6. Quality Workflow
**Purpose**: Improve code quality and add tests.

**Steps**:
1. Analyze code quality
2. Identify improvements
3. Generate test suites
4. Apply refactoring suggestions
5. Run quality checks

**Output**: Improved code with tests and quality report.

---

## 🤖 AI Integration

### OpenAI Setup

1. **Get API Key**
   - Visit [platform.openai.com](https://platform.openai.com/)
   - Create account or sign in
   - Go to API Keys section
   - Create new secret key
   - Copy and save (shown only once)

2. **Add to Environment**
   ```bash
   OPENAI_API_KEY=sk-...
   ```

3. **Usage Monitoring**
   - Monitor usage at [platform.openai.com/usage](https://platform.openai.com/usage)
   - Set spending limits to prevent overages
   - Track costs per feature

### Code Generation

FlashFusion uses GPT-4 for code generation:

```typescript
import { generateCode } from '@/server/generation';

const result = await generateCode({
  prompt: "Create a React todo app with TypeScript",
  userId: "user-123",
});
```

**Prompt Engineering Tips**:
- Be specific about requirements
- Mention tech stack and frameworks
- Include architectural preferences
- Specify styling approach
- Mention accessibility/security needs

### Image Generation

Uses DALL-E 3 for high-quality images:

```typescript
import { generateImage } from '@/server/generation';

const image = await generateImage({
  prompt: "A futuristic cityscape at sunset",
  style: "photorealistic",
  userId: "user-123",
});
```

**Supported Styles**:
- `photorealistic`: Photo-realistic images
- `digital-art`: Digital artwork style
- `sketch`: Hand-drawn sketch style
- `cinematic`: Movie poster style
- `anime`: Anime/manga style
- `fantasy`: Fantasy art style

### Rate Limits

OpenAI rate limits by plan:
- **Free Tier**: 3 requests/minute, 200 requests/day
- **Pay-as-you-go**: 60 requests/minute, 10,000 requests/day
- **Tier 1+**: Higher limits based on usage

FlashFusion implements client-side rate limiting to prevent API overages.

---

## 💳 Payment Integration

FlashFusion uses Stripe for payment processing.

### Setup

1. **Create Stripe Account**
   - Go to [stripe.com](https://stripe.com)
   - Sign up for account
   - Complete verification

2. **Get API Keys**
   - Dashboard → Developers → API Keys
   - Copy Secret key (`sk_test_...` or `sk_live_...`)
   - Copy Publishable key (`pk_test_...` or `pk_live_...`)

3. **Create Products**
   - Dashboard → Products
   - Create "Pro Plan" - $29/month
   - Create "Enterprise Plan" - $99/month
   - Copy Price IDs (`price_...`)

4. **Configure Webhooks**
   - Dashboard → Developers → Webhooks
   - Add endpoint: `https://your-domain.com/api/stripe/webhook`
   - Select events:
     - `checkout.session.completed`
     - `customer.subscription.created`
     - `customer.subscription.updated`
     - `customer.subscription.deleted`
     - `invoice.payment_succeeded`
     - `invoice.payment_failed`
   - Copy Signing Secret (`whsec_...`)

5. **Set Environment Variables**
   ```bash
   STRIPE_SECRET_KEY=sk_test_...
   VITE_STRIPE_PUBLISHABLE_KEY=pk_test_...
   STRIPE_WEBHOOK_SECRET=whsec_...
   STRIPE_PRO_PRICE_ID=price_...
   STRIPE_ENTERPRISE_PRICE_ID=price_...
   ```

### Subscription Plans

#### Free Plan
- **Price**: $0/month
- **Usage**: 10 AI generations/month
- **Features**: Basic templates, community support

#### Pro Plan
- **Price**: $29/month
- **Usage**: 100 AI generations/month
- **Features**: Premium templates, priority support, advanced features

#### Enterprise Plan
- **Price**: $99/month
- **Usage**: Unlimited AI generations
- **Features**: All templates, dedicated support, custom features, API access

### Testing Payments

Use Stripe test cards:
- **Success**: `4242 4242 4242 4242`
- **Requires Auth**: `4000 0027 6000 3184`
- **Declined**: `4000 0000 0000 0002`

Use any future expiry date and any 3-digit CVC.

### Webhook Testing

Test locally with Stripe CLI:
```bash
# Install Stripe CLI
brew install stripe/stripe-cli/stripe

# Login
stripe login

# Forward webhooks to local server
stripe listen --forward-to localhost:5000/api/stripe/webhook

# Trigger test events
stripe trigger checkout.session.completed
```

---

## 📊 Analytics

### Privacy-First Approach

FlashFusion implements privacy-conscious analytics:
- **Consent Required**: No tracking without explicit user consent
- **First-Party Only**: No third-party analytics or trackers
- **Minimal Data**: Only essential event data collected
- **Transparent**: Clear disclosure in consent banner
- **Opt-Out**: Users can revoke consent anytime

### Tracked Events

```typescript
// Landing page view
trackEvent('landing_view', '/');

// CTA click
trackEvent('cta_click', '/pricing', { plan: 'pro' });

// Generation started
trackEvent('generation_started', '/dashboard', { type: 'code' });

// Generation completed
trackEvent('generation_completed', '/dashboard', { type: 'code', duration: 5000 });

// Upgrade click
trackEvent('upgrade_click', '/pricing', { plan: 'pro' });
```

### Event Structure

```typescript
{
  name: string;          // Event name
  route: string;         // Current route
  props?: object;        // Additional properties
  timestamp: Date;       // Event timestamp
  consentGiven: boolean; // User consent status
}
```

### Analytics Dashboard

View analytics in the database:
```sql
SELECT
  name,
  COUNT(*) as count,
  DATE(timestamp) as date
FROM analytics_events
WHERE consent_given = true
GROUP BY name, DATE(timestamp)
ORDER BY date DESC;
```

---

## 🌍 Internationalization

i18n is stubbed and ready for implementation.

### Current Setup

```typescript
// client/src/utils/i18n.ts
export function t(key: string, locale: string = 'en'): string {
  // Returns translation for key
}

export function setLocale(locale: string): void {
  // Sets active locale
}
```

### Usage

```typescript
import { t, setLocale } from '@/utils/i18n';

// Set locale
setLocale('es');

// Get translation
const title = t('hero.title'); // Returns Spanish translation
```

### Adding Translations

1. Enable feature flag:
   ```typescript
   I18N_ENABLED: true
   ```

2. Add translation files in `client/src/i18n/`:
   ```
   i18n/
   ├── en.json
   ├── es.json
   ├── fr.json
   └── de.json
   ```

3. Use translation function:
   ```typescript
   <h1>{t('hero.title')}</h1>
   ```

### RTL Support

For RTL languages (Arabic, Hebrew):
1. Add `dir="auto"` to `<html>` tag
2. Use logical properties in CSS (start/end instead of left/right)
3. Test layout in RTL mode

---

## 🔧 Troubleshooting

### Common Issues

#### "DATABASE_URL must be set"
**Cause**: Missing or invalid database connection string.

**Solution**:
1. Verify `.env` file exists
2. Check `DATABASE_URL` is set
3. Verify connection string format
4. Test database connectivity

#### "Stripe integration not working"
**Cause**: Missing Stripe credentials or incorrect configuration.

**Solution**:
1. Verify all Stripe env vars are set:
   - `STRIPE_SECRET_KEY`
   - `VITE_STRIPE_PUBLISHABLE_KEY`
   - `STRIPE_WEBHOOK_SECRET`
   - `STRIPE_PRO_PRICE_ID`
   - `STRIPE_ENTERPRISE_PRICE_ID`
2. Check Stripe webhook endpoint is configured
3. Verify webhook signature

#### "OpenAI API error"
**Cause**: Invalid API key or rate limit exceeded.

**Solution**:
1. Verify `OPENAI_API_KEY` is correct
2. Check OpenAI usage limits
3. Verify account has credits
4. Review error message for details

#### "Build fails with MODULE_NOT_FOUND"
**Cause**: Missing dependencies or incorrect imports.

**Solution**:
```bash
# Clean install
rm -rf node_modules package-lock.json
npm install

# Check TypeScript
npm run check
```

#### "Port 5000 already in use"
**Cause**: Another process using port 5000.

**Solution**:
```bash
# Find process
lsof -i :5000

# Kill process
kill -9 <PID>

# Or use different port
PORT=3000 npm run dev
```

#### "Database connection timeout"
**Cause**: Network issue or incorrect connection string.

**Solution**:
1. Check internet connectivity
2. Verify database is accessible
3. For Supabase/Neon: Use Connection Pooling URL
4. Check firewall settings

#### "Lighthouse CI fails"
**Cause**: Performance issues or missing assets.

**Solution**:
1. Check all assets exist (grain.png, OG images)
2. Review Lighthouse report for specific issues
3. Optimize images and code splitting
4. Check lighthouse thresholds in `lighthouserc.json`

### Debug Mode

Enable debug logging:
```bash
DEBUG=* npm run dev
```

View server logs:
```bash
# In production
pm2 logs flashfusion

# View specific lines
pm2 logs flashfusion --lines 100
```

### Getting Help

- **Documentation**: Check all `.md` files in repository
- **Issues**: [GitHub Issues](https://github.com/ChaosClubCo/FlashFusion/issues)
- **Community**: Join discussions
- **Support**: Contact support team

---

## 🗺️ Roadmap

FlashFusion is ~55% complete. See [ROADMAP.md](ROADMAP.md) for the complete development plan.

### Current Status (October 2025)
- ✅ Core platform (90% complete)
- ✅ AI features (55% complete)
- 🟡 Workflows (30% complete - UI done, backend in progress)
- ✅ User management (85% complete)
- 🟡 Payment integration (90% complete - needs final testing)
- ✅ Analytics (70% complete)

### Upcoming Features

#### Q4 2025
- [ ] Real-Time Collaboration Workspace (4 weeks)
  - Live collaborative code editing
  - Multi-user cursor tracking
  - Comment threads
  - Version history

- [ ] AI Code Assistant Chat (4 weeks)
  - Context-aware AI chat interface
  - Code explanation and debugging
  - Refactoring suggestions
  - Streaming responses

#### Q1 2026
- [ ] Template Marketplace (8 weeks)
  - User-generated template library
  - Creator revenue sharing
  - Rating and review system
  - Template forking and remixing

- [ ] Deployment Pipeline Integration (5 weeks)
  - One-click deploy to Vercel, Netlify, Railway, etc.
  - Automated CI/CD setup
  - Environment variable management
  - Deployment monitoring

#### Q2 2026
- [ ] AI Learning Mode (6 weeks)
  - Prompt engineering course
  - Code understanding path
  - Interactive exercises
  - Gamification and achievements

See [ROADMAP.md](ROADMAP.md) for detailed timelines and implementation plans.

---

## 🤝 Contributing

Contributions are welcome! FlashFusion is a production application, so we maintain high standards.

### Guidelines

1. **Code Quality**
   - Follow existing patterns
   - Write TypeScript (no `any` types)
   - Include JSDoc comments
   - Format with Prettier

2. **Accessibility**
   - Maintain WCAG 2.1 AA compliance
   - Test with keyboard navigation
   - Add ARIA labels where needed
   - Test with screen readers

3. **Performance**
   - Keep bundle size minimal
   - Lazy-load when possible
   - Optimize images
   - Run Lighthouse tests

4. **Testing**
   - Write Playwright tests for new features
   - Test across browsers
   - Check mobile responsiveness
   - Verify accessibility

5. **Documentation**
   - Update README for major changes
   - Add JSDoc for functions
   - Update API docs
   - Include usage examples

### Pull Request Process

1. Fork the repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Make changes and commit: `git commit -m 'feat: add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open Pull Request
6. Wait for CI checks (Lighthouse, type checking)
7. Address review feedback
8. Merge after approval

### Development Setup

See [Quick Start](#-quick-start) and [Installation](#-installation) sections.

---

## 📝 License

Copyright © 2025 FlashFusion. All rights reserved.

This is proprietary software. Unauthorized copying, distribution, or modification is prohibited.

For licensing inquiries, contact: [email protected]

---

## 📞 Support & Resources

### Core Documentation
| Document | Purpose |
|----------|---------|
| [README.md](README.md) | Main project documentation |
| [CHANGELOG.md](CHANGELOG.md) | Version history and release notes |
| [ROADMAP.md](ROADMAP.md) | Development roadmap (MVP to Scale) |
| [DEVELOPMENT_STATUS.md](DEVELOPMENT_STATUS.md) | Current implementation status |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution guidelines |
| [SECURITY.md](SECURITY.md) | Security policy |

### AI & Agent Guides
| Document | Purpose |
|----------|---------|
| [CLAUDE.md](CLAUDE.md) | AI agent development guide |
| [AGENTS.md](AGENTS.md) | Multi-agent orchestration |
| [GEMINI.md](GEMINI.md) | Google AI integration guide |

### Setup & Integration
| Document | Purpose |
|----------|---------|
| [SETUP_GITHUB_SECRETS.md](SETUP_GITHUB_SECRETS.md) | GitHub Actions setup |
| [STRIPE_INTEGRATION.md](STRIPE_INTEGRATION.md) | Stripe payment setup |
| [STRIPE_QUICKSTART.md](STRIPE_QUICKSTART.md) | Quick Stripe onboarding |
| [VERCEL_DEPLOYMENT.md](VERCEL_DEPLOYMENT.md) | Vercel deployment guide |
| [SUPABASE_BEST_PRACTICES.md](SUPABASE_BEST_PRACTICES.md) | Database optimization |
| [LIGHTHOUSE_CI_FIXES.md](LIGHTHOUSE_CI_FIXES.md) | Performance optimization |

### Feature Documentation
| Document | Purpose |
|----------|---------|
| [AGENT_TEASERS_VISUAL_GUIDE.md](AGENT_TEASERS_VISUAL_GUIDE.md) | Agent preview component |
| [QUICK_START_AGENT_TEASERS.md](QUICK_START_AGENT_TEASERS.md) | Agent teasers quick start |
| [AGENT_TEASERS_INTEGRATION.md](AGENT_TEASERS_INTEGRATION.md) | Integration guide |

### Links
- **Repository**: [github.com/ChaosClubCo/FlashFusion](https://github.com/ChaosClubCo/FlashFusion)
- **Issues**: [github.com/ChaosClubCo/FlashFusion/issues](https://github.com/ChaosClubCo/FlashFusion/issues)
- **Website**: [flashfusion.dev](https://flashfusion.dev)
- **Status**: [status.flashfusion.dev](https://status.flashfusion.dev)
- **Documentation**: [docs.flashfusion.dev](https://docs.flashfusion.dev)

### Contact
- **Email**: [email protected]
- **Support**: [email protected]
- **Sales**: [email protected]

---

## 🙏 Acknowledgments

Built with amazing open-source technologies:
- [React](https://react.dev/)
- [TypeScript](https://www.typescriptlang.org/)
- [Vite](https://vitejs.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Radix UI](https://www.radix-ui.com/)
- [Framer Motion](https://www.framer.com/motion/)
- [Drizzle ORM](https://orm.drizzle.team/)
- [OpenAI](https://openai.com/)
- [Stripe](https://stripe.com/)
- [Playwright](https://playwright.dev/)
- [And many more...](package.json)

---

**Last Updated**: December 30, 2025
**Version**: 2.0.0
**Project Status**: Active Development (55% Complete)

**Ready to build something amazing? Get started with FlashFusion today!** ⚡
