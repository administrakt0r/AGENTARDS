# Tauri Stack Template

## Detected Technologies
- **Framework:** Tauri
- **Language:** Rust (backend), [React/Vue/Svelte/etc.] (frontend)
- **Package Manager:** Cargo (Rust), npm/yarn/pnpm (JS)
- **Build Tool:** tauri-cli

## Common Stack Patterns

### Rust Backend
- Commands: `#[tauri::command]`
- State: `tauri::State<T>`
- Events: `app.emit()`, `window.listen()`
- Window management: `WebviewWindowBuilder`
- File system: `tauri::fs`

### Permissions
- Capabilities: `capabilities/*.json`
- Permission scoping
- Plugin permissions
- Allowlist configuration

### Frontend Integration
- `@tauri-apps/api` for IPC
- `invoke()` for command calls
- `listen()` for event handling
- `emit()` for sending events

### Common File Locations
```
src-tauri/
├── src/
│   ├── main.rs
│   ├── lib.rs
│   ├── commands/
│   ├── state/
│   └── utils/
├── capabilities/
├── icons/
├── Cargo.toml
└── tauri.conf.json
src/ or app/              # Frontend app
```

## Project Type Detection Signals
- `Cargo.toml` with `tauri` dependency
- `tauri.conf.json`
- `src-tauri/` directory
- `#[tauri::command]` annotations
- `@tauri-apps/api` in package.json

## Agent Customizations

### Security
- Capability-based permissions
- CSP configuration
- Allowlist scoping
- File system access control

### Performance
- Frontend bundle optimization
- Rust command efficiency
- Event system batching
- Window lifecycle management

### Distribution
- Platform installers
- Auto-update configuration
- Code signing
- Bundle size optimization
