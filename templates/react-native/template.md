# React Native Stack Template


## Stack Detection Signals

```bash
rg --files -g 'package.json' -g 'app.json' -g 'app.config.*' -g 'metro.config.*' -g 'android/**/build.gradle*' -g 'ios/**/*.pbxproj' -g '!node_modules' -g '!android/build' -g '!ios/Pods'
rg -n 'react-native|expo' package.json app.json app.config.ts app.config.js
```

## Detected Technologies
- **Framework:** React Native
- **Language:** TypeScript/JavaScript
- **Package Manager:** npm/yarn/pnpm
- **Navigation:** React Navigation
- **State:** Redux/Zustand/Context/MobX
- **Test Runner:** Jest/Detox

## Common Stack Patterns

### Navigation
- React Navigation: `@react-navigation/native`
- Stack: `createNativeStackNavigator()`
- Tab: `createBottomTabNavigator()`
- Drawer: `createDrawerNavigator()`

### Native Modules
- Bridge: native module communication
- Turbo Modules: new architecture
- Expo: managed workflow
- Expo Modules: new native module API

### Platform-Specific Code
- File extensions: `.ios.tsx`, `.android.tsx`
- Platform.select(): runtime branching
- Platform.OS: conditional logic

### Common File Locations
```
├── src/
│   ├── components/
│   ├── screens/
│   ├── navigation/
│   ├── store/
│   ├── services/
│   ├── hooks/
│   ├── utils/
│   └── types/
├── ios/                  # iOS native code
├── android/              # Android native code
├── assets/               # Images, fonts
└── __tests__/            # Tests
```

## Project Type Detection Signals
- `package.json` with react-native dependency
- `app.json` or `app.config.js` (Expo)
- `Podfile` (iOS)
- `build.gradle` (Android)
- `.xcodeproj`
- `metro.config.js`

## Agent Customizations

### Performance
- Hermes engine optimization
- FlatList/FlashList virtualization
- Image caching (react-native-fast-image)
- Reanimated animations
- Fabric renderer (new architecture)

### Platform-Specific
- iOS: Info.plist, entitlements
- Android: AndroidManifest.xml, ProGuard
- Permissions: react-native-permissions
- Push: @react-native-firebase/messaging

### Testing
- Component: React Native Testing Library
- E2E: Detox, Maestro
- Unit: Jest with react-native preset
- Snapshot: Jest snapshots

## Common Issues
- Native module autolinking / version mismatch
- New Architecture (Fabric/TurboModules) migration gaps
- Platform-specific (`Platform.OS`) regressions
- Listener/subscription memory leaks
- Oversized images / unchecked asset imports
- Metro cache / stale bundle issues

## Testing Patterns
- Jest + React Native Testing Library
- `npx react-native doctor` / `npx expo-doctor`
- E2E: Detox or Maestro
- `tsc --noEmit` for TypeScript
- Android/iOS debug build smoke

## Industry Standards (verified 2026-10 via Context7)

Sources: `reactnative.dev/docs/components-and-apis`, `docs.expo.dev/`, `reactnavigation.org/docs/getting-started`

- Use the navigation and native-module style already in the app. A new library needs the dependency policy the task allows.
- Lists that can grow use the list component the app already uses.
- Metro bundle success and a device or simulator run are separate evidence.
- Treat committed native projects as owned or generated based on the Expo or React Native config you read.

## Coding rules

Apply these to JS/TS UI. When the Android layer is hand-written Kotlin, also apply `templates/android/template.md` coding rules to that Kotlin only.

- A screen does one job. Extract a named component when layout wrappers nest past about three levels.
- Every wrapper needs a reason. Long lists use the list component the screen already uses, with a stable key.
- Hoist state to the owner. Ignore or cancel a request when the screen leaves or the inputs change. A late response must not overwrite a newer one.
- Loading, empty, offline, and error stay on every screen you touch. Error text is product copy, not a raw exception.
- User-visible strings go through the project's i18n, including alerts and permission reasons. Update every locale the app already ships.
- Colors and type come from the existing theme when it has a token. Check dark mode, font scale, and RTL on the surface you change.
- Do not add a navigation library, data library, or native UI toolkit the app does not already use. Do not rewrite a screen in Compose because an Android guide recommends Compose.
- Do not blanket-wrap components in `memo` or `useCallback` without a measured cost or an existing local pattern that requires it.
- Meaningful images need an accessibility label. Decorative ones get an explicit empty label.
- No invented React Native or Expo APIs. Check the version in the lockfile. Do not upgrade the SDK as a side effect of a screen fix.
