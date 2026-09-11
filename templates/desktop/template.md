# Desktop Stack Template

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
