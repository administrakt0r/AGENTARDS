# CLI Stack Template

## Detected Technologies
- **Language:** [JavaScript/TypeScript/Python/Go/Rust/Ruby/etc.]
- **Framework:** [Commander/Yargs/Click/Cobra/clap/etc.]
- **Package Manager:** [npm/pip/cargo/go mod/etc.]
- **Build Tool:** [pkg/nexe/pyinstaller/cargo build/etc.]

## Common Stack Patterns

### Command Structure
- Entry point: `bin/`, `cli.js`, `cli.ts`, `main.go`, `main.rs`
- Commands: subcommand pattern
- Options: flags, arguments, environment variables
- Help: auto-generated from definitions

### Argument Parsing
- JS/TS: Commander.js, Yargs, oclif
- Python: Click, argparse, typer
- Go: Cobra, urfave/cli
- Rust: clap, structopt

### Output
- stdout: primary output
- stderr: errors, warnings
- Formatting: plain, JSON, table, colors
- Quiet/verbose modes

### Configuration
- Config files: `.rc`, `.yaml`, `.json`, `.toml`
- Environment variables
- XDG directories
- Global vs local config

### Common File Locations
```
├── bin/                  # Entry points
├── src/                  # Source code
├── commands/             # Command implementations
├── lib/ or utils/        # Shared utilities
├── config/               # Configuration handling
├── test/                 # Tests
└── docs/                 # Documentation
```

## Project Type Detection Signals
- `bin` field in `package.json`
- `#!/usr/bin/env` shebang
- `click.command` or `@click.command()` (Python)
- `cobra.Command` (Go)
- `#[derive(Parser)]` (Rust)

## Agent Customizations

### User Experience
- Exit codes: 0 success, non-zero error
- Help text: clear, concise
- Error messages: actionable
- Color: respect NO_COLOR, FORCE_COLOR
- Piping: handle pipe/close events

### Distribution
- npm: `npx` support
- pip: PyPI publishing
- cargo: crates.io
- Homebrew formula
- Shell completions

### Testing
- Snapshot tests for output
- Argument parsing tests
- Integration tests with real execution
- Cross-platform testing
