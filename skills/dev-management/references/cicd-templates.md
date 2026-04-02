# CI/CD Templates Reference

This reference provides GitHub Actions workflow templates for common language/framework combinations. Use these as starting points during the dev-management interview, customized based on the user's stack and preferences.

---

## Template Selection Guide

| Language / Framework | CI Template | Recommended Linter | Recommended Test Framework | Package Manager |
|---|---|---|---|---|
| JavaScript / Node.js | [Node.js CI](#nodejs-ci) | ESLint | Jest / Vitest | npm / pnpm / yarn |
| TypeScript / Node.js | [Node.js CI](#nodejs-ci) + type checking | ESLint + tsc | Jest / Vitest | npm / pnpm / yarn |
| Next.js | [Next.js CI](#nextjs-ci) | ESLint (built-in) | Jest / Vitest + Playwright | npm / pnpm |
| React (Vite) | [Vite CI](#vite-ci) | ESLint | Vitest + Playwright | npm / pnpm |
| Python | [Python CI](#python-ci) | Ruff | pytest | pip / uv / poetry |
| Python (Django) | [Django CI](#django-ci) | Ruff | pytest-django | pip / uv / poetry |
| Python (FastAPI) | [FastAPI CI](#fastapi-ci) | Ruff | pytest + httpx | pip / uv / poetry |
| Ruby on Rails | [Rails CI](#rails-ci) | RuboCop | RSpec / Minitest | bundler |
| Go | [Go CI](#go-ci) | golangci-lint | go test | go modules |
| Rust | [Rust CI](#rust-ci) | clippy | cargo test | cargo |
| Java / Spring Boot | [Java CI](#java-ci) | Checkstyle / SpotBugs | JUnit 5 | Maven / Gradle |
| .NET / C# | [.NET CI](#dotnet-ci) | dotnet format | xUnit / NUnit | NuGet |

---

## Node.js CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [20.x]

    steps:
      - uses: actions/checkout@v4

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'  # Change to 'pnpm' or 'yarn' as needed

      - name: Install dependencies
        run: npm ci

      # Linting (if enabled)
      - name: Lint
        run: npm run lint

      # Type checking (if TypeScript)
      - name: Type check
        run: npx tsc --noEmit

      # Tests
      - name: Run tests
        run: npm test

      # Build verification
      - name: Build
        run: npm run build
```

---

## Next.js CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Type check
        run: npx tsc --noEmit

      - name: Run tests
        run: npm test

      - name: Build
        run: npm run build

  # E2E tests (if Playwright enabled)
  e2e:
    runs-on: ubuntu-latest
    needs: ci
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps

      - name: Run Playwright tests
        run: npx playwright test

      - name: Upload test results
        if: failure()
        uses: actions/upload-artifact@v4
        with:
          name: playwright-report
          path: playwright-report/
```

---

## Vite CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Type check
        run: npx tsc --noEmit

      - name: Run tests
        run: npx vitest run

      - name: Build
        run: npm run build
```

---

## Python CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ['3.12']

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install ruff pytest

      # Linting
      - name: Lint with Ruff
        run: ruff check .

      # Format checking
      - name: Check formatting
        run: ruff format --check .

      # Type checking (if mypy/pyright used)
      - name: Type check
        run: mypy .

      # Tests
      - name: Run tests
        run: pytest
```

---

## Django CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
          POSTGRES_DB: test_db
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install ruff pytest-django

      - name: Lint
        run: ruff check .

      - name: Run tests
        env:
          DATABASE_URL: postgres://test:test@localhost:5432/test_db
        run: pytest

      - name: Run migrations check
        env:
          DATABASE_URL: postgres://test:test@localhost:5432/test_db
        run: python manage.py migrate --check
```

---

## FastAPI CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install ruff pytest httpx

      - name: Lint
        run: ruff check .

      - name: Type check
        run: mypy .

      - name: Run tests
        run: pytest
```

---

## Rails CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_PASSWORD: password
        ports:
          - 5432:5432

    steps:
      - uses: actions/checkout@v4

      - name: Set up Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.3'
          bundler-cache: true

      - name: Setup database
        env:
          DATABASE_URL: postgres://postgres:password@localhost:5432/test
          RAILS_ENV: test
        run: |
          bundle exec rails db:create
          bundle exec rails db:schema:load

      - name: Lint
        run: bundle exec rubocop

      - name: Run tests
        env:
          DATABASE_URL: postgres://postgres:password@localhost:5432/test
          RAILS_ENV: test
        run: bundle exec rspec
```

---

## Go CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up Go
        uses: actions/setup-go@v5
        with:
          go-version: '1.22'

      - name: Lint
        uses: golangci/golangci-lint-action@v4

      - name: Run tests
        run: go test ./...

      - name: Build
        run: go build ./...
```

---

## Rust CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Rust
        uses: dtolnay/rust-toolchain@stable
        with:
          components: clippy, rustfmt

      - name: Check formatting
        run: cargo fmt --check

      - name: Lint
        run: cargo clippy -- -D warnings

      - name: Run tests
        run: cargo test

      - name: Build
        run: cargo build --release
```

---

## Java CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up JDK
        uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'
          cache: 'maven'  # Change to 'gradle' if using Gradle

      # Maven
      - name: Build and test
        run: mvn clean verify

      # Or Gradle
      # - name: Build and test
      #   run: ./gradlew build
```

---

## .NET CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup .NET
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '8.0.x'

      - name: Restore dependencies
        run: dotnet restore

      - name: Check formatting
        run: dotnet format --verify-no-changes

      - name: Build
        run: dotnet build --no-restore

      - name: Test
        run: dotnet test --no-build
```

---

## Deployment Templates

### Deploy to Vercel

```yaml
# Vercel handles deployment automatically via GitHub integration.
# No workflow file needed — just connect your repo at vercel.com.
# Vercel auto-deploys:
#   - Preview deployments for every PR
#   - Production deployment when merging to main
```

### Deploy to Netlify

```yaml
# Netlify handles deployment automatically via GitHub integration.
# Connect your repo at netlify.com.
# Netlify auto-deploys:
#   - Deploy previews for every PR
#   - Production deployment when merging to main
```

### Deploy to AWS (via SST / CDK)

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Deploy
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        run: npx sst deploy --stage production
```

### Deploy to Heroku

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Deploy to Heroku
        uses: akhileshns/heroku-deploy@v3.13.15
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_app_name: ${{ secrets.HEROKU_APP_NAME }}
          heroku_email: ${{ secrets.HEROKU_EMAIL }}
```

### Deploy to DigitalOcean App Platform

```yaml
# DigitalOcean App Platform handles deployment automatically via GitHub integration.
# Connect your repo in the DigitalOcean dashboard.
# Auto-deploys when pushing to the configured branch.
```

### Deploy Docker to Any Server

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production

    steps:
      - uses: actions/checkout@v4

      - name: Login to Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          push: true
          tags: ghcr.io/${{ github.repository }}:latest

      # Add deployment step for your specific server
      # (SSH, kubectl, docker compose, etc.)
```

---

## Security Scanning Add-on

Add this to any CI workflow for dependency vulnerability scanning:

```yaml
      # Add after test steps
      - name: Security audit
        run: npm audit --audit-level=high  # For Node.js
        # run: pip audit                   # For Python (requires pip-audit)
        # run: bundle audit                # For Ruby
        # run: go vuln check ./...         # For Go (requires govulncheck)
```

Or enable GitHub's built-in Dependabot by creating `.github/dependabot.yml`:

```yaml
version: 2
updates:
  - package-ecosystem: "npm"  # Change per language
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 10
```
