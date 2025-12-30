# Contributing to FlashFusion

Thank you for your interest in contributing to FlashFusion! This document provides guidelines and instructions for contributing.

---

## Code of Conduct

By participating in this project, you agree to maintain a respectful and inclusive environment. Be considerate, constructive, and professional in all interactions.

---

## Getting Started

### Prerequisites

- Node.js 22.x or higher
- npm 10.x or higher
- PostgreSQL 14.x or higher (or Neon/Supabase account)
- Git

### Development Setup

```bash
# Clone the repository
git clone https://github.com/ChaosClubCo/FlashFusion.git
cd FlashFusion

# Install dependencies
npm install

# Copy environment template
cp .env.example .env

# Configure your .env file with:
# - DATABASE_URL (required)
# - OPENAI_API_KEY (for AI features)
# - STRIPE_* variables (for payment features)

# Run database migrations
npm run db:push

# Start development server
npm run dev
```

The app will be available at `http://localhost:5000`.

---

## How to Contribute

### Reporting Bugs

1. Search existing issues to avoid duplicates
2. Use the bug report template
3. Include:
   - Clear description of the issue
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details (OS, Node version, browser)
   - Screenshots or error logs if applicable

### Suggesting Features

1. Search existing issues for similar suggestions
2. Use the feature request template
3. Include:
   - Clear problem statement
   - Proposed solution
   - Alternative approaches considered
   - Potential impact on existing features

### Submitting Pull Requests

1. **Fork the repository** and create your branch from `main`
2. **Install dependencies** with `npm install`
3. **Make your changes** following our coding standards
4. **Write tests** for new functionality
5. **Run the test suite** with `npm run test:e2e`
6. **Run type checking** with `npm run check`
7. **Update documentation** if needed
8. **Create a pull request** with a clear description

---

## Development Workflow

### Branch Naming

```
feature/short-description   # New features
fix/issue-description       # Bug fixes
docs/what-changed           # Documentation updates
refactor/what-changed       # Code refactoring
test/what-covered           # Test additions
```

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Code style changes (formatting, semicolons, etc.)
- `refactor`: Code refactoring
- `perf`: Performance improvement
- `test`: Adding or updating tests
- `chore`: Build process or auxiliary tool changes

**Examples:**
```
feat(auth): add Google OAuth integration
fix(pricing): correct pro plan display price
docs(readme): update installation instructions
perf(images): implement lazy loading for gallery
test(workflows): add e2e tests for ai-creation
```

---

## Coding Standards

### TypeScript

- Use strict mode (no `any` types)
- Define explicit return types for functions
- Use Zod for runtime validation
- Prefer interfaces over type aliases for object shapes

```typescript
// Good
interface UserProfile {
  id: string;
  email: string;
  plan: 'free' | 'pro' | 'enterprise';
}

function getUser(id: string): Promise<UserProfile | undefined> {
  return storage.getUser(id);
}

// Avoid
function getUser(id: any): any {
  return storage.getUser(id);
}
```

### React Components

- Use functional components with hooks
- Follow accessibility patterns (WCAG 2.1 AA)
- Use Radix UI primitives from `components/ui/`
- Implement proper error boundaries

```typescript
// Good
export function UserCard({ user }: { user: UserProfile }) {
  return (
    <Card>
      <CardHeader>
        <CardTitle>{user.email}</CardTitle>
      </CardHeader>
      <CardContent>
        <Badge>{user.plan}</Badge>
      </CardContent>
    </Card>
  );
}
```

### CSS/Styling

- Use Tailwind CSS utilities
- Follow mobile-first responsive design
- Maintain 4.5:1 color contrast ratio
- Support reduced motion preferences

```tsx
// Good
<button className="px-4 py-2 bg-primary text-primary-foreground rounded-md
  hover:bg-primary/90 focus:ring-2 focus:ring-offset-2 focus:ring-primary
  motion-safe:transition-colors">
  Click me
</button>
```

### Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Components | PascalCase | `UserProfile.tsx` |
| Hooks | camelCase with `use` | `useAuth.ts` |
| Utilities | camelCase | `sanitizeHtml.ts` |
| Constants | UPPER_SNAKE_CASE | `API_BASE_URL` |
| CSS classes | kebab-case | `bg-primary` |
| Database tables | snake_case | `workflow_runs` |

---

## Testing Guidelines

### E2E Tests (Playwright)

Write tests in `tests/e2e/`:

```typescript
// tests/e2e/my-feature.spec.ts
import { test, expect } from '@playwright/test';

test.describe('My Feature', () => {
  test('should do something', async ({ page }) => {
    await page.goto('/my-page');

    await page.fill('[data-testid="input"]', 'value');
    await page.click('[data-testid="submit"]');

    await expect(page.locator('[data-testid="result"]'))
      .toBeVisible();
  });
});
```

### Running Tests

```bash
# Run all tests
npm run test:e2e

# Run in UI mode
npm run test:e2e:ui

# Run specific test file
npx playwright test tests/e2e/my-feature.spec.ts
```

### Test Coverage Goals

- Critical paths: 100%
- UI components: 80%
- API endpoints: 90%
- Edge cases: All known edge cases

---

## Documentation

### When to Update Docs

- New features require README updates
- API changes require API docs updates
- Breaking changes require upgrade guide
- New components require usage examples

### Documentation Files

| File | Purpose |
|------|---------|
| `README.md` | Main project documentation |
| `CLAUDE.md` | AI agent development guide |
| `AGENTS.md` | Multi-agent orchestration |
| `GEMINI.md` | Google AI integration |
| `CHANGELOG.md` | Version history |
| `ROADMAP.md` | Development roadmap |

---

## Pull Request Process

### Before Submitting

- [ ] Code follows style guidelines
- [ ] Self-reviewed the changes
- [ ] Added tests for new functionality
- [ ] All tests pass locally
- [ ] Updated documentation if needed
- [ ] No console errors or warnings
- [ ] Accessibility checked (keyboard nav, screen reader)

### PR Template

```markdown
## Description
Brief description of changes.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
Describe testing performed.

## Screenshots (if applicable)

## Checklist
- [ ] Tests pass
- [ ] Docs updated
- [ ] Follows style guide
```

### Review Process

1. Automated checks must pass (TypeScript, tests, Lighthouse)
2. At least one maintainer review required
3. All comments addressed
4. Squash merge to main

---

## Development Tips

### Hot Reload

The dev server supports hot reload for both client and server changes.

### Database Changes

1. Update schema in `shared/schema.ts`
2. Run `npm run db:push` to apply changes
3. Update storage methods if needed

### Adding UI Components

Use shadcn/ui CLI when possible:

```bash
npx shadcn-ui@latest add button
```

### Debugging

```bash
# Enable debug logging
DEBUG=* npm run dev

# View server logs
npm run dev 2>&1 | grep "API"
```

---

## Getting Help

- Check existing documentation
- Search closed issues
- Ask in discussions
- Contact maintainers

---

## License

By contributing, you agree that your contributions will be licensed under the project's license.

---

**Thank you for contributing to FlashFusion!**
