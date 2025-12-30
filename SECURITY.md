# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 2.x.x   | :white_check_mark: |
| 1.x.x   | :white_check_mark: |
| < 1.0   | :x:                |

---

## Reporting a Vulnerability

We take security vulnerabilities seriously. If you discover a security issue, please report it responsibly.

### How to Report

**DO NOT** create a public GitHub issue for security vulnerabilities.

Instead, please email: **security@flashfusion.dev**

Include:
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Any suggested fixes

### Response Timeline

- **Initial Response**: Within 48 hours
- **Assessment**: Within 7 days
- **Fix Timeline**: Depends on severity
  - Critical: 24-48 hours
  - High: 7 days
  - Medium: 30 days
  - Low: Next release

### After Reporting

1. We will acknowledge your report
2. We will investigate and validate the issue
3. We will work on a fix
4. We will coordinate disclosure with you
5. We will credit you (if desired) in the security advisory

---

## Security Measures

### Authentication

- **Replit OAuth**: OpenID Connect for user authentication
- **Session Security**:
  - httpOnly cookies (prevents XSS access)
  - sameSite: strict (prevents CSRF)
  - Secure flag in production
- **CSRF Protection**: Built into Passport.js

### Input Validation

- **Zod Schemas**: All API inputs validated with Zod
- **DOMPurify**: HTML sanitization for user content
- **Parameterized Queries**: Drizzle ORM prevents SQL injection

### API Security

- **Rate Limiting**: Per-user rate limits on all endpoints
- **CORS**: Configured for authenticated requests
- **Request Logging**: API requests logged for monitoring

### Data Protection

- **Environment Variables**: Secrets never committed to code
- **Encryption**: Database connections use SSL/TLS
- **Stripe Security**: PCI-compliant payment processing

### Content Security

- **Safe Iframes**: Whitelist of allowed iframe sources
- **No Inline Scripts**: CSP-safe patterns
- **Image Sanitization**: External images proxied or validated

---

## Security Checklist for Contributors

### Before Submitting Code

- [ ] No secrets or API keys in code
- [ ] All user input validated with Zod
- [ ] SQL queries use parameterized statements
- [ ] XSS prevention (DOMPurify for HTML)
- [ ] CSRF tokens for state-changing operations
- [ ] Authentication required for protected routes
- [ ] Authorization checks for user-specific data
- [ ] Error messages don't leak sensitive info
- [ ] Dependencies checked for known vulnerabilities

### For New Features

- [ ] Security impact assessment
- [ ] Input validation added
- [ ] Authorization checks implemented
- [ ] Rate limiting considered
- [ ] Logging for security events

---

## Security Best Practices

### Environment Variables

```bash
# Required secrets - NEVER commit these
DATABASE_URL=...
OPENAI_API_KEY=...
STRIPE_SECRET_KEY=...
STRIPE_WEBHOOK_SECRET=...
```

### Database Security

```typescript
// Good - parameterized query (Drizzle ORM)
const user = await db
  .select()
  .from(users)
  .where(eq(users.id, userId));

// Bad - string interpolation
const user = await db.execute(`SELECT * FROM users WHERE id = '${userId}'`);
```

### Input Sanitization

```typescript
// For HTML content
import { sanitizeHtml } from '@/utils/sanitize';
const safeHtml = sanitizeHtml(userInput);

// For API input
import { insertUserSchema } from '@shared/schema';
const validated = insertUserSchema.parse(req.body);
```

### Authentication Checks

```typescript
// Protected route pattern
app.get('/api/user/data', isAuthenticated, async (req, res) => {
  const userId = req.user.claims.sub;
  // Fetch only this user's data
  const data = await storage.getUserData(userId);
  res.json(data);
});
```

### Authorization Patterns

```typescript
// Ensure user can only access their own resources
app.get('/api/projects/:id', isAuthenticated, async (req, res) => {
  const project = await storage.getProject(req.params.id);

  if (!project) {
    return res.status(404).json({ error: 'Not found' });
  }

  // Authorization check
  if (project.userId !== req.user.claims.sub) {
    return res.status(403).json({ error: 'Forbidden' });
  }

  res.json(project);
});
```

---

## Known Security Considerations

### Development Mode

In development (`NODE_ENV=development`):
- Demo user bypass is enabled
- Some security headers may be relaxed
- Debug logging may include sensitive info

**Never use development mode in production.**

### Third-Party Dependencies

We regularly audit dependencies for vulnerabilities:

```bash
# Check for vulnerabilities
npm audit

# Fix automatically
npm audit fix
```

### Rate Limiting

Current rate limits:
- Free plan: 10 requests/hour
- Pro plan: 100 requests/hour
- Enterprise: Configurable

---

## Security Headers (Production)

Recommended headers for production deployment:

```
Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' https://fonts.gstatic.com; img-src 'self' data: https:; connect-src 'self' wss: https:; frame-src https://www.youtube-nocookie.com https://player.vimeo.com https://checkout.stripe.com;
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=()
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

---

## Incident Response

If a security incident occurs:

1. **Contain**: Isolate affected systems
2. **Assess**: Determine scope and impact
3. **Notify**: Inform affected users if data exposed
4. **Remediate**: Fix the vulnerability
5. **Review**: Conduct post-incident analysis
6. **Improve**: Update security measures

---

## Contact

- **Security Issues**: security@flashfusion.dev
- **General Support**: support@flashfusion.dev

---

**Last Updated**: December 30, 2025
