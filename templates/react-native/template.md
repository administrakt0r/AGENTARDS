# React Native Stack Template

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
