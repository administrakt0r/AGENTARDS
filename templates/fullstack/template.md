# Fullstack Stack Template

## Detected Technologies
- **Frontend:** [React/Vue/Angular/Svelte + Next.js/Nuxt/etc.]
- **Backend:** [Express/NestJS/Django/Flask/FastAPI/Laravel/etc.]
- **Language:** [TypeScript/Python/Go/PHP/etc.]
- **Database:** [PostgreSQL/MySQL/MongoDB/etc.]
- **ORM:** [Prisma/Sequelize/TypeORM/Django ORM/Eloquent/etc.]
- **Monorepo Tool:** [Turborepo/Nx/Lerna/npm workspaces/etc.]
- **Package Manager:** [npm/yarn/pnpm/etc.]

## Common Stack Patterns

### Monorepo Structure
```
/
├── apps/
│   ├── web/              # Frontend app
│   ├── api/              # Backend app
│   └── admin/            # Admin panel
├── packages/
│   ├── shared/           # Shared types/utils
│   ├── ui/               # Shared components
│   └── config/           # Shared configs
├── turbo.json or nx.json
└── package.json          # Root workspace
```

### Shared Code
- Types: shared between frontend and backend
- Validation: shared schemas (Zod, Joi, Pydantic)
- Utils: shared utility functions
- Constants: shared constants and enums

### API Contract
- REST: shared OpenAPI specs
- GraphQL: shared schema definitions
- tRPC: end-to-end type safety
- WebSocket: shared event types

### Deployment
- Monolith: single deploy unit
- Split: separate frontend/backend deploys
- Serverless: API routes as functions
- Docker: containerized per app

### Testing
- Frontend: Jest, Vitest, Playwright, Cypress
- Backend: Jest, Pytest, Go test
- Integration: API + frontend tests
- E2E: full stack tests

## Project Type Detection Signals
- Root `package.json` with workspaces
- `turbo.json` or `nx.json`
- `apps/` directory with multiple apps
- `packages/` directory with shared code
- Both frontend and backend dependencies in root
- `.env` with both DATABASE_URL and frontend env vars

## Agent Customizations

### Frontend-Backend Sync
- Verify shared types are used on both sides
- Check API contract matches frontend expectations
- Ensure validation schemas are shared
- Verify error handling is consistent

### Monorepo Concerns
- Package dependency management
- Build ordering and caching
- Shared configuration consistency
- Version management across packages

## Common Issues
- Client/server type or schema drift
- CORS and cookie/auth misconfiguration
- Authorization enforced in UI but not the API
- Cache invalidation / stale cross-boundary data
- Secrets leaking to client bundles
- N+1 queries behind API routes

## Testing Patterns
- Unit: `npm test` / Vitest / Jest
- API: supertest / httpx / `httptest`
- E2E: Playwright / Cypress against a running stack
- DB: Testcontainers / ephemeral test database
- `tsc --noEmit` for shared types
