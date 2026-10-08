# AIcoding Enterprise Delivery Reference

This file contains reusable templates and checklists for consistent enterprise-grade project delivery.

## Enterprise Delivery Checklist (Copy this for every project)

\`\`\`markdown
**AIcoding Delivery Verification**

**Phase 1: Code Quality**
- [ ] All linter errors fixed (ReadLints passed)
- [ ] Code follows language best practices
- [ ] No TODO/FIXME comments left
- [ ] Comprehensive error handling and logging

**Phase 2: Testing (Minimum 80% coverage)**
- [ ] Unit tests for all core logic
- [ ] Integration tests for APIs/database
- [ ] E2E tests for critical user flows (if frontend)
- [ ] CI pipeline runs all tests successfully

**Phase 3: Observability & Production Readiness**
- [ ] Structured logging (Winston/Pino or structlog)
- [ ] Health check endpoints (/health, /ready)
- [ ] Metrics endpoint (Prometheus compatible)
- [ ] Environment-based configuration (.env.example)
- [ ] Rate limiting, CORS, security headers

**Phase 4: Containerization**
- [ ] Multi-stage Dockerfile
- [ ] docker-compose.yml for local + prod
- [ ] .dockerignore optimized
- [ ] Image size < 300MB where possible

**Phase 5: CI/CD**
- [ ] .github/workflows/ci.yml (lint, test, build)
- [ ] .github/workflows/cd.yml (deploy on main)
- [ ] Automated semantic versioning (if applicable)
- [ ] Artifact publishing (Docker Hub or GHCR)

**Phase 6: Documentation & Deliverables**
- [ ] Comprehensive README.md with:
  - Architecture diagram (mermaid)
  - Quickstart (1-command run)
  - Environment setup
  - Deployment instructions (Docker, cloud)
  - Monitoring setup
  - Scaling notes
- [ ] Architecture Canvas (.canvas.tsx)
- [ ] API documentation (OpenAPI/Swagger if backend)
- [ ] Contribution guidelines

**Final Validation**
- [ ] Project can be cloned and run with `git clone && docker compose up`
- [ ] All tests pass in CI
- [ ] Canvas artifact created and reviewed
- [ ] git history shows logical commits
\`\`\`

## Sample GitHub CI Workflow (ci.yml)

\`\`\`yaml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Setup Node/Python
      uses: actions/setup-node@v4 # or setup-python
      with:
        node-version: 20
    - name: Install dependencies
      run: npm ci # or uv pip install -r requirements.txt
    - name: Lint
      run: npm run lint
    - name: Test
      run: npm test
    - name: Build
      run: npm run build
\`\`\`

## Sample Dockerfile (Multi-stage)

\`\`\`dockerfile
# Build stage
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package*.json ./
RUN npm ci --production
EXPOSE 3000
CMD ["npm", "start"]
\`\`\`

## Sample docker-compose.yml

\`\`\`yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
\`\`\`

## Monitoring Template (Prometheus + Grafana basics)

Include basic prometheus.yml, Grafana dashboards for:
- Request rate, latency, error rate (RED method)
- Resource utilization (CPU, Memory)
- Database connection pool

## Canvas Architecture Review Template

When creating .canvas.tsx, use:
- H1 for project name
- Grid of Stats (lines of code, test coverage, tech stack count)
- Mermaid diagram rendered as text or description
- Table for key endpoints/components
- Stack of sections for trade-off analysis

See canvas/SKILL.md for exact component usage (no slop, use HostTheme tokens).

## Project Type Templates

**Next.js Fullstack**:
- App Router + Server Components
- Auth: NextAuth.js or Clerk
- DB: Prisma + PostgreSQL
- Testing: Jest + RTL + Playwright
- Styling: Tailwind + shadcn/ui

**FastAPI Backend**:
- FastAPI + Pydantic v2
- SQLAlchemy 2.0 + Alembic
- Testing: pytest + httpx + pytest-asyncio
- OpenAPI auto docs

Update this reference.md as new patterns are discovered through deliveries.
