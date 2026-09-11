# Rust Stack Template

## Detected Technologies
- **Framework:** [Actix/Axum/Rocket/Tauri/etc.]
- **Package Manager:** [Cargo]
- **Test Runner:** [cargo test]
- **Linter:** [clippy/rustfmt/etc.]
- **Build Tool:** [cargo build]

## Common Stack Patterns

### Project Structure
```
├── src/
│   ├── main.rs           # Entry point
│   ├── lib.rs            # Library root
│   ├── handlers/         # Request handlers
│   ├── models/           # Data models
│   ├── services/         # Business logic
│   ├── middleware/        # Middleware
│   ├── routes/           # Route definitions
│   └── errors/           # Error types
├── migrations/           # Database migrations
├── benches/              # Benchmarks
└── examples/             # Usage examples
```

### Error Handling
- Result<T, E> pattern
- Thiserror for library errors
- Anyhow for application errors
- Custom error types

### Async
- Tokio runtime
- async/await syntax
- Tower middleware
- Hyper/Axum for HTTP

### Database
- SQLx: compile-time checked queries
- Diesel: ORM with type safety
- SeaORM: async ORM
- Migrations: sqlx-cli, diesel migrations

## Project Type Detection Signals
- `Cargo.toml`
- `Cargo.lock`
- `src/main.rs` or `src/lib.rs`
- `rust-toolchain.toml`
- `.rs` files

## Agent Customizations

### Rust-Specific
- Ownership and borrowing patterns
- Lifetime management
- Trait design
- Macro usage
- Unsafe code auditing

### Safety
- No unsafe without justification
- Panic handling
- Overflow checking
- Fuzzing targets

### Performance
- Zero-cost abstractions
- Compile-time optimization
- Benchmark comparisons
- Binary size optimization

## Common Issues
- `unwrap()`/`expect()` panicking on production paths
- Blocking calls inside async runtimes
- Feature-flag combinations that fail to compile
- Debug-only success / release-only overflow or ordering
- Long build times / no incremental or cache
- Unreviewed `unsafe` blocks

## Testing Patterns
- `cargo test` and `cargo test --all-features`
- `cargo nextest run` for faster suites
- Property tests: `proptest` / `quickcheck`
- `cargo clippy -- -D warnings`, `cargo fmt --check`
- Doc tests (`cargo test --doc`)
