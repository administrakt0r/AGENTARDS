# Flutter Stack Template


## Stack Detection Signals

```bash
rg --files -g 'pubspec.yaml' -g 'analysis_options.yaml' -g 'lib/**/*.dart' -g '!build' -g '!.dart_tool'
rg -n 'flutter:|sdk:\s*flutter|Widget build' pubspec.yaml lib -g '!build'
```

## Detected Technologies
- **Framework:** Flutter
- **Language:** Dart
- **Package Manager:** pub (pubspec.yaml)
- **State Management:** Provider/Riverpod/Bloc/GetX/etc.
- **Test Runner:** flutter_test

## Common Stack Patterns

### State Management
- Provider: `ChangeNotifierProvider`
- Riverpod: `Provider`, `StateProvider`, `FutureProvider`
- Bloc: `BlocProvider`, `Cubit`, `Bloc`
- GetX: `Get.put()`, `Obx()`

### Navigation
- Navigator 2.0: declarative routing
- go_router: `GoRouter`
- AutoRoute: code generation
- Named routes

### Common File Locations
```
lib/
├── main.dart
├── app/
│   ├── app.dart
│   └── routes.dart
├── features/
│   └── [feature]/
│       ├── data/
│       ├── domain/
│       └── presentation/
├── core/
│   ├── theme/
│   ├── utils/
│   └── widgets/
└── generated/            # Generated code
test/
├── unit/
├── widget/
└── integration/
```

## Project Type Detection Signals
- `pubspec.yaml`
- `lib/main.dart`
- `dart` in pubspec
- `flutter` in pubspec
- `.dart` files
- `analysis_options.yaml`

## Agent Customizations

### Dart/Flutter-Specific
- Null safety patterns
- Widget lifecycle
- Animation controllers
- Custom painting
- Platform channels

### Performance
- Widget rebuild optimization
- Image caching
- List virtualization
- Memory leak detection
- GPU profiling

### Testing
- Unit: `test()`, `group()`
- Widget: `testWidgets()`
- Integration: `integration_test/`
- Golden tests: `matchesGoldenFile()`

## Common Issues
- Outdated `pubspec.lock` / SDK constraint drift
- Platform channel errors on one platform only
- Large image/font assets not downscaled
- Missing R8/ProGuard or obfuscation config
- iOS CocoaPods / `Podfile.lock` drift
- Unbounded `setState` causing rebuild churn

## Testing Patterns
- `flutter analyze`
- `flutter test` (unit + widget)
- `flutter test integration_test/` for device e2e
- Golden tests for UI regressions
- `flutter build apk --debug` smoke

## Industry Standards (verified 2026-10 via Context7)

Sources: `docs.flutter.dev/data-and-backend/state-mgmt/options`, `docs.flutter.dev/ui/adaptive-responsive`, `riverpod.dev/` and `bloclibrary.dev/` when those packages are already dependencies

- Use the state approach already in `pubspec.yaml`. Do not introduce a second one for a small fix.
- Dispose controllers and close streams with the widget that owns them.
- Check overflow and text scale on the layouts the change touches.
- `flutter analyze` and `flutter test` are the default checks when the Flutter SDK is present. A missing SDK is **Unknown**.
