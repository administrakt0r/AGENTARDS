# Backend API Stack Template

## Detected Technologies
- **Framework:** [Express/NestJS/Fastify/Django/Flask/FastAPI/Laravel/Rails/Gin/Fiber/etc.]
- **Language:** [JavaScript/TypeScript/Python/Go/PHP/Ruby/etc.]
- **Package Manager:** [npm/yarn/pnpm/composer/bundler/go mod/cargo/etc.]
- **Database:** [PostgreSQL/MySQL/MongoDB/SQLite/Redis/etc.]
- **ORM:** [Prisma/Sequelize/TypeORM/Django ORM/Eloquent/GORM/etc.]
- **Test Runner:** [Jest/Mocha/Pytest/Go test/RSpec/etc.]
- **API Style:** [REST/GraphQL/gRPC/etc.]

## Common Stack Patterns

### Routing
- Express: `app.get()`, `router.get()` → `routes/`, `src/routes/`
- NestJS: `@Controller()`, `@Get()` → `src/modules/`, `src/controllers/`
- Django: `urlpatterns` → `urls.py`
- FastAPI: `@app.get()` → `routers/`
- Laravel: `Route::get()` → `routes/api.php`
- Gin: `router.GET()` → `handlers/`

### Database
- Connection: env vars (`DATABASE_URL`), config files
- Migrations: Prisma, Knex, Alembic, Flyway, GORM AutoMigrate
- Seeders: `seed/`, `seeds/`, `database/seeders/`
- Models: `models/`, `src/models/`, `app/Models/`

### Authentication
- JWT: token-based auth, middleware validation
- Session: cookie-based, express-session
- OAuth: passport.js, social-auth
- API keys: middleware validation, rate limiting

### Middleware
- Express: `app.use()` chain
- NestJS: `@UseGuards()`, interceptors
- Django: MIDDLEWARE setting
- FastAPI: dependency injection
- Laravel: middleware kernel

### Testing
- Unit: function/service tests
- Integration: API endpoint tests
- Load: k6, Artillery, locust
- Contract: Pact, Spring Cloud Contract

### Common File Locations
```
src/
├── controllers/         # Request handlers
├── routes/              # Route definitions
├── models/              # Data models
├── services/            # Business logic
├── middleware/           # Request middleware
├── validators/          # Input validation
├── migrations/          # Database migrations
├── seeders/             # Seed data
├── config/              # Configuration
├── utils/               # Utility functions
└── types/               # Type definitions
```

## Project Type Detection Signals
- `package.json` with express/nest/fastify dependency
- `manage.py` (Django), `artisan` (Laravel), `Gemfile` (Rails)
- `go.mod` with gin/fiber dependency
- `requirements.txt` or `pyproject.toml` with flask/fastapi
- `src/` with controllers/routes structure
- `.env` with DATABASE_URL

## Agent Customizations by Sub-Type

### Express / Fastify
- Middleware chain pattern
- Route organization: `routes/` directory
- Error handling: error middleware
- Validation: Joi, Zod, express-validator

### NestJS
- Module pattern: `@Module()` decorators
- Dependency injection: `@Injectable()`
- Guards, interceptors, pipes
- DTO validation: class-validator

### Django
- MVT pattern: Models, Views, Templates
- URL routing: `urlpatterns` lists
- ORM queries: QuerySet API
- Admin: `admin.py` auto-generated

### FastAPI
- Dependency injection: `Depends()`
- Pydantic models for validation
- Async/await patterns
- Auto-generated docs: `/docs`, `/redoc`

### Laravel
- MVC pattern: Controllers, Models, Views
- Eloquent ORM: relationships, scopes
- Artisan commands
- Service providers

### Gin / Fiber (Go)
- Handler functions
- Middleware chaining
- GORM for database
- Struct validation

## Common Issues
- Request input not validated at the boundary (schema/type checks missing)
- N+1 queries and missing pagination on list endpoints
- Secrets or connection strings committed in config
- No timeouts/retries on downstream calls
- Unhandled async errors crashing the process
- Irreversible migrations or missing indexes

## Testing Patterns
- Unit: `npm test` / `pytest` / `go test ./...` / `mvn test`
- HTTP integration: supertest, httpx/TestClient, `httptest`, MockMvc
- Contract/schema: OpenAPI validation, Pact
- Smoke: `curl -f http://localhost:PORT/health`
