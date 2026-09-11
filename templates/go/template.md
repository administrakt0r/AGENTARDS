# Go Stack Template

## Detected Technologies
- **Framework:** [Gin/Fiber/Echo/Chi/standard lib/etc.]
- **Package Manager:** [Go modules (go.mod)]
- **Test Runner:** [go test]
- **Linter:** [golangci-lint/staticcheck/etc.]
- **Build Tool:** [go build]

## Common Stack Patterns

### Project Structure
```
├── cmd/                  # Entry points
│   └── server/
│       └── main.go
├── internal/             # Private packages
│   ├── handler/
│   ├── service/
│   ├── repository/
│   ├── model/
│   └── middleware/
├── pkg/                  # Public packages
├── api/                  # API definitions
├── configs/              # Configuration
├── migrations/           # Database migrations
└── scripts/              # Utility scripts
```

### HTTP Handlers
- Handler functions: `func(c *gin.Context)`
- Middleware: `func(next http.Handler) http.Handler`
- Routes: `router.GET()`, `router.POST()`

### Database
- GORM: struct-based models
- sqlx: named queries
- database/sql: standard library
- Migrations: migrate, golang-migrate

### Testing
- Table-driven tests
- Testify for assertions
- httptest for HTTP testing
- Mock generation: mockery, counterfeiter

## Project Type Detection Signals
- `go.mod` file
- `go.sum` file
- `main.go` with `package main`
- `cmd/` directory
- `internal/` directory
- `.go` files with `package` declaration

## Agent Customizations

### Go-Specific
- Error handling patterns
- Context propagation
- Goroutine safety
- Interface design
- Build tags for platform-specific code

### Concurrency
- Goroutine leak detection
- Channel patterns
- sync.Pool usage
- Race condition detection (`-race` flag)

### Performance
- pprof profiling
- Memory allocation optimization
- HTTP server tuning
- Database connection pooling
