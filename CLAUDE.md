# Jason Warren - Global Configuration

> Last updated: 2026-01-06

Claude reads this first. The more specific you are here, the less you repeat yourself in every chat.

---

## Developer Profile

**Role**: Level 4 Software Developer Apprentice @ Founders and Coders  
**Current Focus**: AM2 Portfolio assessment, preparing Iris (ILR File Creator replacement)

### Technical Expertise

**Languages**
- **TypeScript**: Primary language, expert-level
- **JavaScript**: ES2022+, deep familiarity
- **Python**: Minimal experience, learning as needed - not a go-to
- **Rust**: None (Tauri uses it, but I'm not writing Rust myself)

**Frontend**
- **Preferred**: Svelte, SvelteKit (elegant, straightforward)
- **When Required**: React, Next.js (used when necessary, not by choice)

**Backend**
- Node.js, Express
- API design and integration patterns

**Databases**
- **Relational**: PostgreSQL, Supabase (with Row-Level Security expertise)
- **Graph**: Neo4j (polyglot persistence architectures)
- **Document**: MongoDB
- **Client-side**: RxDB (experimented with for offline-first patterns - interesting but not a go-to)

**Testing**
- Familiar with Vitest and Jest
- Testing approaches still evolving - this is a weakness
- Write tests when valuable, but no systematic methodology yet

**Tooling**
- Git (comprehensive commit history culture)
- Zed IDE (Catppuccin theme, Fira Code)
- CLI workflows, Desktop Commander MCP
- Vite for build tooling

**Architectural Patterns**
- Security-first implementation (RLS, authentication layers, data validation)
- Polyglot persistence (choosing databases for their strengths)
- Cross-language documentation and code bridging
- Semantic validation over brittle positional mapping

---

## Communication Preferences

### What Works
- **Direct and structured**: Clear problem decomposition, explicit assumptions
- **Neurodivergent-friendly**: Avoid ambiguity, use concrete examples, explicit over implicit
- **British English**: Spelling, idioms, cultural context
- **Authentic voice**: Technical precision wrapped in personality, not corporate drivel
- **Clever humour**: Contextual, intellectually playful, never forced

### What Doesn't Work
- Sycophantic responses (they reduce trust - I'll use a competitor's model)
- Excessive apologies or hedging
- American colloquialisms or cultural assumptions
- Over-formatted responses (excessive bullets/bold when prose would do)
- Long explanations when a direct answer suffices
- Pretending weaknesses are strengths

---

## Code Style & Conventions

### Paradigm Preferences
- Pragmatic over ideological - use OOP when it fits, functional when it fits
- Don't impose functional patterns just because they're trendy
- Clear structure matters more than paradigm purity

### Naming Conventions

**Projects**: Evocative single words + functional descriptors
- Examples: Rhea (curriculum generator), Iris (file creator), Theia (assessment tool)

**Variables/Functions**: Clear, semantic names over brevity
```typescript
// ✅ Yes
const authenticatedUserId = await getAuthenticatedUser();
const monthlyRevenue = calculateMonthlyRevenue(transactions);

// ❌ No
const authUsrId = await getAuth();
const revM = calcMR(txs);
```

**Files**: kebab-case for files, PascalCase for React components
- `user-authentication.ts`, `UserProfile.tsx`

### TypeScript Standards
- Strict mode enabled (`"strict": true`)
- Prefer interfaces over types for object shapes
- Avoid `any` - use `unknown` when type is truly uncertain
- Leverage union types and discriminated unions
- Return types explicitly stated on exported functions

### Testing Reality
Look, testing is a known weakness. I write tests when they're valuable, but there's no systematic TDD approach or comprehensive coverage culture here. Claude shouldn't assume tests exist for everything, and shouldn't be precious about test-first workflows.

When tests do exist:
- Test file naming: `module-name.test.ts` alongside source
- Integration tests for critical paths preferred over exhaustive unit coverage
- E2E for user-facing flows when it matters

---

## Documentation Style

### Structure
- **Mermaid diagrams** for system architecture and data flow (use them liberally)
- **Markdown** for all long-form documentation
- **Inline comments** sparingly - code should largely explain itself
- **ADRs (Architecture Decision Records)** for significant technical choices

### Format Preferences
- Headers: `##` for main sections, `###` for subsections
- Code blocks: Always specify language for syntax highlighting
- Lists: Only when genuinely needed (not for every response)
- Accessibility: Meaningful alt text, semantic HTML in React

### README Requirements
Every project should include:
- One-line description at top
- Quick start (installation + first run)
- Architecture overview (with Mermaid if non-trivial)
- Development workflow
- Testing approach (or honest admission that it's lacking)
- License

---

## Project Patterns & Workflows

### Git Commit Style
Conventional Commits format: `type(scope): description`

**Types**: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`

Detailed commit bodies when context is needed - good git history is documentation. Reference issue/ticket numbers where relevant.

### Breaking Change Detection

**Always flag potential breaking changes** - even when the user is handling commits themselves. Proactively warn when changes might need a `BREAKING CHANGE:` footer or `!` indicator.

Flag when you see:
- Removed or renamed exports (functions, types, components)
- Changed function signatures (parameters added/removed/reordered)
- Modified return types or response shapes
- Database schema changes (columns, constraints, relations)
- API endpoint changes (routes, methods, request/response format)
- Environment variable additions or changes
- Configuration format changes
- Removed features or deprecated code deletion

Format: `This looks like a breaking change - consider adding BREAKING CHANGE: to the commit footer or using feat!: prefix.`

### Development Environment
- **IDE**: Zed (Catppuccin Latte light / Macchiato dark)
- **Font**: Fira Code 16px
- **Hardware**: MacBook Air M2 (2022)
- **Package Manager**: npm/pnpm (project-specific)
- **Formatter**: Prettier (external) for JS/TS
- **Linting**: ESLint with TypeScript rules

### Tooling Preferences
- Vite for build tooling (over webpack)
- Vitest for testing in new projects (over Jest)
- SvelteKit preferred, Next.js only when genuinely needed
- Avoid Next.js app router complexity unless there's a compelling reason

---

## Security & Best Practices

### Security Defaults
- Never commit secrets (environment variables only)
- Row-Level Security (RLS) for multi-tenant data - this is an area of expertise
- Input validation at boundaries (Zod, class-validator)
- HTTPS only, secure cookies
- CSP headers on sensitive applications

### Database Practices
- Migrations versioned and tested
- Connection pooling configured
- Indexes on frequently queried columns
- Avoid N+1 queries (seriously)
- Use transactions for multi-step operations

---

## Project-Specific Overrides

This global config is just the baseline. Project-level CLAUDE.md files take precedence:

- Place project conventions in `./.claude/CLAUDE.md`
- Use nested CLAUDEs for subsystem context (e.g., `frontend/CLAUDE.md`)
- Project configs override global settings - hierarchy matters

---

## Meta Notes

- This file is intentionally detailed - comprehensive context reduces repetitive explanations
- Not every section applies to every interaction - use judgment
- When in doubt, ask rather than assume
- Review and update as patterns evolve (this is a living document)
