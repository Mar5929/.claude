# Branching Strategies Reference

This reference provides detailed descriptions of common branching strategies for use during the dev-management interview. Use this to explain trade-offs and help the user choose the right strategy.

---

## GitHub Flow

**Best for:** Solo developers, small teams (1-3), projects with continuous deployment.

**How it works:**
1. `main` is always deployable
2. Create a feature branch from `main` (e.g., `feature/add-login`)
3. Work on the branch, commit often
4. Open a Pull Request when ready
5. Review, discuss, and run CI checks
6. Merge to `main` — deploy automatically or on demand
7. Delete the feature branch

**Branch naming convention:**
- `feature/short-description` — new features
- `fix/short-description` — bug fixes
- `chore/short-description` — maintenance, refactoring, deps
- `docs/short-description` — documentation changes

**Pros:**
- Simple to understand and use
- Low overhead — no complex branch management
- `main` is always production-ready
- Works naturally with GitHub PRs
- Great for continuous deployment

**Cons:**
- No built-in staging or pre-production environment
- All features go straight to production after merge
- Can be risky without good CI/CD and test coverage

**When to recommend:** Default recommendation for most projects. Start here unless there's a specific reason not to.

---

## GitHub Flow + Environment Branches

**Best for:** Small-to-medium teams (3-8), projects that need a staging environment.

**How it works:**
1. `main` is production
2. `staging` (or `develop`) is the pre-production testing environment
3. Create feature branches from `main`
4. Open PRs to `staging` first for testing
5. After testing passes, promote to `main` (via PR or merge)
6. `main` deploys to production

**Branch naming convention:** Same as GitHub Flow.

**Pros:**
- Simple like GitHub Flow, but with a safety net
- Staging environment catches issues before production
- Clear promotion path: feature → staging → production

**Cons:**
- Slightly more overhead than pure GitHub Flow
- Staging can get out of sync with production if not managed
- Need to maintain two deployed environments

**When to recommend:** When the user has a staging or testing environment, or when deploying directly to production feels too risky.

---

## GitFlow

**Best for:** Larger teams, projects with scheduled releases, enterprise environments.

**How it works:**
1. `main` — production code, tagged with release versions
2. `develop` — integration branch for next release
3. `feature/*` — branched from `develop`, merged back to `develop`
4. `release/*` — branched from `develop` when ready to release; bug fixes go here
5. `hotfix/*` — branched from `main` for emergency production fixes; merged to both `main` and `develop`

**Branch naming convention:**
- `feature/ticket-id-short-description`
- `release/v1.2.0`
- `hotfix/v1.2.1-critical-fix`

**Release workflow:**
1. Features accumulate on `develop`
2. When ready to release, create `release/vX.Y.Z` from `develop`
3. Only bug fixes on the release branch — no new features
4. Merge release branch to `main` (tag it) and back to `develop`
5. Deploy `main`

**Pros:**
- Clear separation between development, release prep, and production
- Supports parallel feature development and release stabilization
- Well-suited for versioned software with scheduled releases
- Hotfix path for emergency production fixes

**Cons:**
- More complex — more branches to manage
- Merge conflicts more likely with long-lived branches
- Overhead may not be worth it for small teams
- Can slow down delivery compared to simpler strategies

**When to recommend:** When the project has formal release cycles, multiple environments (dev/staging/prod), or when the team is large enough that feature isolation is critical.

---

## Trunk-Based Development

**Best for:** Experienced teams with strong CI/CD, comprehensive test suites, and fast feedback loops.

**How it works:**
1. Everyone works on `main` (the "trunk")
2. Commits are small and frequent (at least daily)
3. Feature flags hide incomplete work from users
4. Short-lived branches (max 1-2 days) are OK for larger changes
5. CI runs on every push; deployment is continuous

**Branch naming convention:**
- Short-lived branches: `short/description` or just commit directly to `main`

**Pros:**
- Fastest feedback loop — no long-lived branches
- Minimal merge conflicts — small, frequent integrations
- Encourages small, incremental changes
- Works perfectly with continuous deployment

**Cons:**
- Requires strong test coverage — broken `main` breaks everyone
- Requires feature flags for in-progress work
- Not suitable for teams without mature CI/CD
- Higher risk if testing is weak

**When to recommend:** Only when the user has good test coverage, a solid CI/CD pipeline, and experience with continuous deployment. Not recommended as a starting point for less technical users.

---

## Comparison Table

Use this table to help the user compare strategies at a glance:

| Aspect | GitHub Flow | GH Flow + Staging | GitFlow | Trunk-Based |
|---|---|---|---|---|
| Complexity | Low | Low-Medium | High | Low (but requires discipline) |
| Best team size | 1-3 | 3-8 | 5+ | 3+ (experienced) |
| Release style | Continuous | Continuous with staging | Scheduled releases | Continuous |
| Environments | Production only | Staging + Production | Dev + Staging + Production | Production (with feature flags) |
| Merge conflicts | Rare | Rare | More common | Very rare |
| Safety net | CI checks | Staging environment | Release branches | Test suite + feature flags |
| Learning curve | Easy | Easy | Moderate | Easy (concepts), harder (practices) |
