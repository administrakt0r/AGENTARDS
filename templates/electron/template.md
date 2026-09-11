# Electron Stack Template

## Detected Technologies
- **Framework:** Electron
- **Language:** TypeScript/JavaScript
- **Frontend:** [React/Vue/Svelte/etc.]
- **Package Manager:** npm/yarn/pnpm
- **Build Tool:** electron-builder/electron-forge
- **Test Runner:** Jest/Spectron

## Common Stack Patterns

### Process Architecture
- Main process: `main.js`, `electron/main.ts`
- Renderer process: web app
- Preload: `preload.js` (context bridge)
- IPC: `ipcMain.handle()`, `ipcRenderer.invoke()`

### Security
- Context Isolation: enabled
- Node Integration: disabled
- Web Preferences: sandbox, contextIsolation
- CSP: Content Security Policy headers

### Auto-Update
- electron-updater
- electron-forge updater
- Release server integration

### Common File Locations
```
├── electron/
│   ├── main.ts           # Main process
│   ├── preload.ts        # Preload script
│   ├── ipc/              # IPC handlers
│   └── menu/             # Application menu
├── src/                  # Renderer app
├── assets/               # Icons, images
├── build/                # Build resources
└── electron-builder.yml  # Build config
```

## Project Type Detection Signals
- `package.json` with electron dependency
- `electron-builder.yml` or `forge.config.js`
- `electron/` directory
- `main.js` with `app.on('ready')`
- `BrowserWindow` usage

## Agent Customizations

### Security
- IPC channel allowlist
- Permission handling
- Navigation restrictions
- Certificate validation

### Performance
- Window lifecycle management
- Memory monitoring
- Startup optimization
- IPC message batching

### Distribution
- Code signing
- Auto-update channels
- Platform installers
- Crash reporting
