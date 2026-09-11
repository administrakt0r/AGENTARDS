# Mobile Stack Template

## Detected Technologies
- **Framework:** [React Native/Flutter/Kotlin/Swift/etc.]
- **Language:** [TypeScript/Dart/Kotlin/Swift/Java/etc.]
- **Package Manager:** [npm/yarn/pnpm/pub/cocoapods/gradle/etc.]
- **Build Tool:** [Xcode/Gradle/Metro/CocoaPods/etc.]
- **Test Runner:** [Jest/Detox/XCTest/JUnit/etc.]

## Common Stack Patterns

### React Native
- Components: `.tsx`, `.jsx`
- Navigation: React Navigation (`@react-navigation/`)
- State: Redux, Zustand, Context, MobX
- Native modules: bridging to iOS/Android
- Platform-specific: `.ios.tsx`, `.android.tsx`
- Entry: `index.js`, `App.tsx`

### Flutter
- Widgets: `.dart` files in `lib/`
- State: Provider, Riverpod, Bloc, GetX
- Navigation: Navigator 2.0, go_router
- Platform channels: MethodChannel
- Entry: `lib/main.dart`

### Native iOS (Swift)
- ViewControllers: `.swift` files
- SwiftUI: `@View` structs
- Storyboards: `.storyboard`, `.xib`
- Package manager: CocoaPods, SPM
- Entry: `AppDelegate.swift`

### Native Android (Kotlin)
- Activities/Fragments: `.kt` files
- Jetpack Compose: `@Composable` functions
- View Binding: `.xml` layouts
- Build: Gradle
- Entry: `AndroidManifest.xml`

### Common File Locations
```
├── src/ or lib/          # Main source
├── components/           # Shared components
├── screens/ or pages/    # Screen components
├── navigation/           # Navigation config
├── store/ or state/      # State management
├── services/             # API calls
├── utils/                # Utility functions
├── assets/               # Images, fonts
├── ios/                  # iOS native code
├── android/              # Android native code
└── __tests__/            # Tests
```

## Project Type Detection Signals
- `package.json` with react-native dependency
- `pubspec.yaml` (Flutter)
- `Podfile` (iOS)
- `build.gradle` (Android)
- `.xcodeproj` or `.xcworkspace`
- `android/` and `ios/` directories

## Agent Customizations

### Performance
- Startup time optimization
- Image loading and caching
- FlatList/ListView virtualization
- Memory leak detection
- Battery usage monitoring

### Platform-Specific
- iOS: Core Data, HealthKit, Push Notifications
- Android: Room, WorkManager, Firebase
- Cross-platform: Expo, Capacitor

### Testing
- Component: React Native Testing Library
- E2E: Detox, Appium, Maestro
- Unit: Jest, flutter_test
- Snapshot: Jest snapshot tests

## Common Issues
- Missing runtime permission handling
- App Store / Play Store rejection (privacy, SDKs)
- Lifecycle leaks on background/rotation
- Fixed sizes ignoring density and safe areas
- No offline / poor-network handling
- Signing/provisioning config drift

## Testing Patterns
- RN: Jest + React Native Testing Library
- Native: XCTest (iOS), JUnit/Espresso (Android)
- Flutter: `flutter test` + `integration_test`
- E2E: Detox, Maestro, Appium
- Debug/release build smoke per platform
