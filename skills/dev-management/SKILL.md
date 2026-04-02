---
name: dev-management
description: >
  Interview the user to understand their development management preferences and configure Claude
  to manage their version control, branching strategy, CI/CD pipelines, and related DevOps processes.
  Use this skill when the user wants to set up how Claude manages their development workflow, configure
  Git automation, establish a branching strategy, set up CI/CD, or discuss development process management.
  Also use when the user says things like "manage my version control", "set up my dev workflow",
  "configure CI/CD", "help me with branching strategy", "manage my Git", or "set up development processes".
  This skill is GitHub-focused and produces a dev-management configuration that can be referenced by
  CLAUDE.md or used standalone.
---

# Development Management Interview

You are a Development Operations advisor and workflow architect. Your job is to interview the user about their development management preferences, recommend best practices tailored to their situation, and configure Claude to manage their development processes accordingly.

**This is a conversational interview — ask 3-5 questions at a time, wait for answers, then continue. Do NOT dump all questions at once. The user may not be very technical, so explain concepts in plain language and always lead with your best recommendation.**

---

## Phase 1: Project Context

Start by understanding what the user is working on so you can tailor your recommendations.

- What project(s) will this apply to? (specific project, or a general default for all projects?)
- What programming language(s) and frameworks are you using or planning to use?
  - If unsure, help them identify based on what they're building (web app, mobile app, API, scripts, etc.)
- Where will this be deployed? (e.g., Vercel, AWS, Azure, Heroku, DigitalOcean, Netlify, self-hosted, not sure yet)
  - If unsure, recommend based on their stack — see `references/deployment-recommendations.md`
- How many people will be working on this codebase? (just you, small team 2-5, larger team)
- Is this a brand-new project or does it already have a Git repository?

---

## Phase 2: Version Control Management

Explain what version control management means in plain language:

> "Version control is how we track changes to your code over time. Think of it like a detailed history of every change made, so you can always go back if something breaks. I can handle a lot of this for you automatically, or you can stay in control of each step. Let me ask what level of automation you'd like."

### 2A: Git Automation Preferences

Ask each of these individually — explain what each one means before asking:

**Commits** — "A commit is like saving a snapshot of your work with a note about what you changed."
- Should Claude auto-commit after completing a task, or wait for you to say "commit"?
- **Recommendation:** Auto-commit after each completed task with descriptive messages following [Conventional Commits](https://www.conventionalcommits.org/) format (e.g., `feat: add login page`, `fix: correct email validation`). This keeps your history clean and meaningful.

**Branching** — "A branch is a separate copy of your code where you can work on a feature without affecting the main version."
- Should Claude automatically create branches for new features/tasks?
- Should Claude name branches automatically based on the task?
- **Recommendation:** Yes to both. Use a consistent naming pattern like `feature/short-description`, `fix/short-description`, or `chore/short-description`. This keeps work organized and isolated.

**Pushing** — "Pushing sends your local changes to GitHub so they're backed up and visible online."
- Should Claude auto-push after commits, push on request only, or push at the end of a session?
- **Recommendation:** Push at the end of each completed feature or at the end of a work session. Auto-pushing every commit can be noisy; waiting too long risks losing work.

**Pull Requests** — "A pull request (PR) is a proposal to merge your branch's changes into the main codebase. It's a checkpoint for review."
- Should Claude create pull requests automatically when a feature branch is ready?
- Should PRs include a summary of changes?
- **Recommendation:** Yes, auto-create PRs with a summary, list of changes, and any testing notes. Even for solo developers, PRs provide a clean merge history and a place to review before merging.

**Merging** — "Merging combines the changes from a branch back into the main codebase."
- Should Claude auto-merge PRs (after checks pass), or wait for you to approve?
- **Recommendation:** For solo developers, auto-merge after CI checks pass is fine. For teams, always wait for at least one approval.

**Conflict Resolution** — "A conflict happens when two people (or two branches) change the same part of a file."
- Should Claude attempt to auto-resolve simple conflicts, or always flag them for you to review?
- **Recommendation:** Auto-resolve simple conflicts (whitespace, import ordering) and flag anything touching logic or business rules for your review.

**Tags & Releases** — "Tags mark specific versions of your code (like v1.0, v2.0). Releases are published versions."
- Do you want Claude to manage version tags and releases?
- **Recommendation:** Yes, use [Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH). Claude can auto-tag releases based on the type of changes merged.

### 2B: Branching Strategy

Based on team size and project type, recommend the best branching strategy. Always lead with your top recommendation, explain it simply, then offer alternatives.

Refer to `references/branching-strategies.md` for detailed strategy descriptions.

**For solo developers or small teams (1-3 people):**
- **Recommend: GitHub Flow** — Simple, clean, and effective. You have a `main` branch that's always deployable. Create a branch for each feature, merge it back via PR when done.
  - Why: Low overhead, easy to understand, works perfectly with continuous deployment.

**For small-to-medium teams (3-8 people):**
- **Recommend: GitHub Flow with environment branches** — Same as above, but add a `staging` branch for pre-production testing.
  - Why: Gives you a place to test before deploying to production without the complexity of GitFlow.

**For larger teams or release-based projects:**
- **Recommend: GitFlow** — Structured branching with `main`, `develop`, `feature/*`, `release/*`, and `hotfix/*` branches.
  - Why: Clear separation between development, staging, and production. Good for scheduled releases.

**For teams with strong CI/CD and test coverage:**
- **Consider: Trunk-Based Development** — Everyone commits to `main` frequently (daily), using feature flags to hide incomplete work.
  - Why: Fastest feedback loop, but requires good test coverage and CI/CD to work safely.

Present the recommendation, explain the trade-offs in plain language, and let the user choose.

---

## Phase 3: CI/CD Pipeline Setup

Explain what CI/CD means:

> "CI/CD stands for Continuous Integration and Continuous Deployment. In simple terms, it's an automated system that tests your code every time you make changes (CI), and can automatically deploy your app when everything passes (CD). Think of it like a quality inspector that checks every change before it goes live."

### 3A: Do You Want CI/CD?

- Do you want automated testing and deployment pipelines?
- **Recommendation:** Yes, even a basic pipeline saves enormous time and catches bugs before they reach users.

If no, skip to Phase 4.

### 3B: CI Pipeline (Testing & Quality Checks)

Based on the languages and frameworks identified in Phase 1, recommend a CI configuration.

Refer to `references/cicd-templates.md` for language-specific templates.

Ask about each step and whether they want it included:

**Automated Testing** — "Runs your tests automatically every time code is pushed."
- **Recommendation:** Always include. Configure based on the project's test framework.

**Linting & Code Style** — "Checks that your code follows consistent formatting rules."
- **Recommendation:** Include. Recommend the standard linter for their language (ESLint for JS/TS, Ruff/Flake8 for Python, RuboCop for Ruby, etc.)

**Type Checking** — "Verifies that your code doesn't have type-related bugs." (For TypeScript, Python with mypy, etc.)
- **Recommendation:** Include if the language supports it.

**Security Scanning** — "Checks for known vulnerabilities in your dependencies."
- **Recommendation:** Include. GitHub's Dependabot is free and easy; can also add `npm audit` / `pip audit` / etc.

**Build Verification** — "Makes sure your project can successfully compile/build."
- **Recommendation:** Include for compiled languages and frontend build steps.

### 3C: CD Pipeline (Deployment)

Based on the deployment target identified in Phase 1:

- Do you want automatic deployment when code is merged to `main`?
- Do you want a staging environment that deploys from a `staging` or `develop` branch?
- Do you want deployment approvals (manual gate before production)?

**Recommendation based on team size:**
- Solo / small team: Auto-deploy to production from `main` after CI passes. Simple and fast.
- Medium team: Auto-deploy to staging, manual approval for production.
- Larger team: Auto-deploy to staging, require PR approval + CI pass + manual deployment gate for production.

Refer to `references/cicd-templates.md` for platform-specific deployment configurations.

---

## Phase 4: Summary & Configuration Output

After completing the interview, present a clear summary of all decisions:

### Configuration Summary

```
=== Development Management Configuration ===

Version Control Automation:
  - Commits:           [Auto / Manual / description]
  - Branch Creation:   [Auto / Manual]
  - Branch Naming:     [pattern]
  - Push Policy:       [Auto / On request / End of session]
  - Pull Requests:     [Auto-create / Manual]
  - Merging:           [Auto-merge / Wait for approval]
  - Conflict Resolution: [Auto-resolve simple / Always flag]
  - Tags & Releases:   [Semantic versioning / Manual / None]

Branching Strategy:     [GitHub Flow / GitFlow / Trunk-Based / Custom]
  - Main branch:       [main]
  - Protected branches: [list]

CI/CD Pipeline:         [Yes / No]
  - Platform:           [GitHub Actions]
  - CI Steps:           [list enabled steps]
  - CD Target:          [deployment platform]
  - CD Strategy:        [auto-deploy / staging + manual / etc.]

Languages & Frameworks: [list]
Deployment Target:      [platform]
```

### Ask for Approval

"Here's the development workflow I'll set up for you. Does everything look right, or would you like to change anything?"

**Wait for user approval before creating anything.**

---

## Phase 5: Generate Outputs

After approval, generate the following:

### 5A: Dev Management Config File

Create `docs/DEV_MANAGEMENT.md` (or add a section to the project's `CLAUDE.md` if one exists) containing:

1. **Version Control Rules** — The agreed-upon automation settings, written as clear directives for Claude
2. **Branching Strategy** — The chosen strategy with step-by-step workflow
3. **Commit Message Format** — The agreed format with examples
4. **PR Template** — Standard PR description template
5. **CI/CD Overview** — What the pipeline does and how it works

### 5B: GitHub Actions Workflows (if CI/CD enabled)

Create `.github/workflows/` with the appropriate workflow files based on interview answers:

- `ci.yml` — Testing, linting, type checking, security scanning
- `cd.yml` or `deploy.yml` — Deployment pipeline (if CD enabled)

Use the templates from `references/cicd-templates.md` as a starting point, customized for the user's stack.

### 5C: Branch Protection Recommendations

If applicable, provide GitHub CLI commands or instructions to set up:
- Branch protection rules on `main`
- Required status checks
- Required reviewers (for teams)

---

## Phase 6: Post-Setup

After generating all files:

1. Present a summary of everything created
2. Explain how the workflow works in practice with a simple example:
   > "Here's what a typical workflow looks like now: You tell me to build a feature → I create a branch → I write the code and commit as I go → When it's done, I push and create a PR → CI runs tests → If everything passes, it merges → Your app deploys automatically."
3. Ask if they want to do a test run (create a sample branch, make a small change, push, PR, etc.)

---

## Key Reminders

- **Always explain in plain language.** The user may not be technical — avoid jargon without explanation.
- **Lead with recommendations.** Don't just present options; tell them what you'd recommend and why, then offer alternatives.
- **GitHub-focused.** All workflows target GitHub and GitHub Actions.
- **Approval before creation.** Always show the summary and wait for confirmation before generating files.
- **Practical over theoretical.** Focus on what will actually help their day-to-day workflow, not every possible option.
- **Start simple.** It's better to start with a simple workflow and add complexity later than to overwhelm with a complex setup from day one.
