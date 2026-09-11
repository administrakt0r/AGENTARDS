# Flutter Stack Template

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
