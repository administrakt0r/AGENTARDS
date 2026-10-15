# Desktop Stack Template


## Stack Detection Signals

```bash
rg --files -g 'package.json' -g 'src-tauri/**' -g 'electron-builder.*' -g '*.csproj' -g '!node_modules' -g '!dist'
rg -n 'electron|contextBridge|@tauri-apps|Window\b' -g '!node_modules' -g '!dist'
```

## Detected Technologies
- **Framework:** [Electron/Tauri/WPF/Qt/Swing/etc.]
- **Language:** [JavaScript/TypeScript/C#/C++/Java/etc.]
- **Package Manager:** [npm/yarn/pnpm/cargo/nuget/etc.]
- **Build Tool:** [electron-builder/tauri-cli/msbuild/cmake/etc.]
- **Frontend:** [React/Vue/Svelte/vanilla/etc.]

## Common Stack Patterns

### Electron
- Main process: `main.js`, `electron/main.ts`
- Renderer: React/Vue/Svelte app
- IPC: `ipcMain`, `ipcRenderer`
- Preload: `preload.js`
- Menu: `Menu.buildFromTemplate()`
- Auto-update: electron-updater

### Tauri
- Rust backend: `src-tauri/`
- Frontend: web framework
- Commands: `#[tauri::command]`
- Events: `emit()`, `listen()`
- State: `tauri::State`
- Permissions: `capabilities/`

### WPF
- XAML: `.xaml` files
- MVVM: Models, ViewModels, Views
- Data Binding: `{Binding}`
- Commands: `ICommand`
- NuGet packages

### Common File Locations
```
├── src/ or app/          # Main source
├── main/                 # Main process (Electron)
├── renderer/             # Renderer process
├── src-tauri/            # Tauri Rust backend
├── components/           # UI components
├── services/             # Business logic
├── store/                # State management
├── assets/               # Icons, images
└── build/                # Build configuration
```

## Project Type Detection Signals
- `package.json` with electron dependency
- `Cargo.toml` with tauri dependency
- `*.csproj` with WPF references
- `CMakeLists.txt` for Qt
- `electron-builder.yml` or `tauri.conf.json`

## Agent Customizations

### Security
- IPC channel validation
- Context isolation
- Node integration disabled
- Content Security Policy
- Permission scoping (Tauri)

### Performance
- Window management
- Memory usage monitoring
- Startup optimization
- IPC message batching

### Distribution
- Code signing
- Auto-update mechanism
- Platform-specific installers
- Crash reporting

## Common Issues
- Oversized installers / unused deps bundled
- No code signing or notarization (OS warnings)
- Auto-update failure with no rollback
- Native dependencies not bundled per-OS
- Insecure IPC between renderer and main
- No crash reporting

## Testing Patterns
- Unit: `npm test` / `cargo test` / `flutter test`
- E2E: Playwright, WebdriverIO
- Package smoke: launch the built artifact per OS
- IPC handler tests with a mocked bridge
- `tsc --noEmit` / `cargo clippy`

## Industry Standards (verified 2026-10 via Context7)

Sources: `www.electronjs.org/docs/latest/tutorial/context-isolation`, `v2.tauri.app/security/capabilities/`, `learn.microsoft.com/dotnet/desktop/wpf/`

- Isolate untrusted web content from privileged APIs. Electron keeps `contextIsolation` on and `nodeIntegration` off in renderers that load remote or user content.
- Tauri commands are allow-listed by the capability files the app already ships.
- Desktop permissions and auto-update signing follow the packager the repository uses.
