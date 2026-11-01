# Android Stack Template

## Detected Technologies

Record evidence for Kotlin/Java or a cross-platform host, Gradle wrapper, Android Gradle Plugin, JDK/SDK availability, modules/variants, `minSdk`/`targetSdk`/`compileSdk`, Compose/XML UI, persistence, and testing. Classify **Detected / Not detected / Unknown**. Do not assume an `app` module, `debug` variant, installed SDK, or connected device.

## Official Android Skills

Use the official Android agent skills from [`android/skills`](https://github.com/android/skills) when the task matches one of them. Resolve them in the runtime catalog, `$CODEX_HOME/skills` when set, `~/.codex/skills/`, or `~/.opencode/skills/`. Also check `~/.claude/skills/` and `~/.agents/skills/` when those exist. Read `SKILL.md` and its required references or scripts before acting. Fetch a missing skill's complete package from the upstream repo when the workflow needs those scripts. Do not execute an uninspected remote installer. Select only the matching skill. A skill's instructions are not evidence that a check was run. Report unavailable tooling or an unrun device check as **Unknown**.

| Task | Skill |
|---|---|
| SDK, emulator, project diagnostics | `android-cli` |
| AGP 9 migration that the task actually requires | `agp-9-upgrade` |
| R8 / ProGuard keep-rule failures | `r8-analyzer` |
| Test strategy and harness setup | `testing-setup` |
| System bars, IME, edge-to-edge | `edge-to-edge` |
| Exported components, intents, PendingIntent | `android-intent-security` |
| Permissions and IPC | `android-permissions-security` |
| Large screens, foldables, window size | `adaptive` |
| Measured traces, heap, jank | `android-profiler` |
| Formal Play policy audit | `play-policy-insights` |

`engage-sdk-integration` is Play Engage content publishing. Do not apply it to Play Integrity, billing, or a general Play-readiness task. Do not load an unrelated skill just because it is installed.

## Project Type Detection Signals

```bash
rg --files -g 'settings.gradle*' -g 'build.gradle*' -g 'gradle-wrapper.properties' -g 'libs.versions.toml' -g 'AndroidManifest.xml' -g 'gradlew*' -g '!build' -g '!.gradle' -g '!node_modules'
rg -n 'com.android.application|com.android.library|org.jetbrains.kotlin.android|compileSdk|targetSdk|minSdk|compose' --glob '*.gradle' --glob '*.gradle.kts' --glob '*.toml' --glob '!**/build/**'
```

Inspect settings, convention plugins, version catalogs, manifests, and source sets to resolve aliases and actual modules. JVM Gradle alone is not Android evidence. For Expo/React Native/Flutter, identify generated native files before editing. Do not expose `local.properties`, keystores, signing passwords, or environment values.

## Common Stack Patterns

- Follow the UI and architecture already in the repo (Compose, Views, React Native, or Flutter). Trace state through the layers that exist. Do not impose a new architecture for a small feature. The default stack later in this file applies only when the project has not chosen one.
- Distinguish recreation from process death. Preserve appropriate state using saved-state mechanisms or durable storage; scope asynchronous work to the correct lifecycle. Rotation alone does not prove process-death recovery.
- Check cancellation and dispatchers: blocking work must not run on the main thread, and callbacks/collectors must not outlive owners. Avoid blanket coroutine or dependency rewrites.
- Handle permissions by supported OS version, including denial, repeated denial, and revocation. Inspect exported components, intent inputs, deep links, URI grants, and PendingIntent behavior where relevant.
- Choose background mechanisms appropriate to timing and OS constraints, such as WorkManager for deferrable persistent work. Do not use foreground services to bypass restrictions.
- Cover offline/error/retry behavior and existing Room/DataStore migration contracts when storage changes. Keep credentials out of code, logs, and packaged assets; use platform security facilities appropriate to the threat model.
- Preserve navigation/back behavior, insets, keyboard handling, font scaling, rotation/window sizes, TalkBack semantics, and adequate touch targets for changed UI.

## Agent Customizations

| Owner | Focus |
|-------|-------|
| `mobile` | Bounded Android feature or lifecycle/platform repair in the authorized scope |
| `hunter` | Reproducible crashes, state loss, navigation and concurrency defects |
| `picasso` | Compose/Views interaction, accessibility, adaptive layouts, loading/error/empty states |
| `sentinel` | Component exposure, permissions, intents, WebView boundaries, sensitive storage/logging |
| `bolt` | Measured startup, jank, memory, battery, background-work cost |
| `testing` | Host JVM tests versus instrumented Compose/Espresso/device tests |
| `database`, `api` | Persistence/migrations and network contracts when relevant |
| `cicd`, `docs` | Reproducible builds, SDK/JDK prerequisites, artifacts and release instructions |

Use selected owners and explicit handoffs. Requested platform work authorizes routine scoped implementation; publishing and signing-key changes are separate decisions.

## Development Task Pattern

Specify the user journey, module/source set, UI/data boundaries, state across recreation/relaunch, permissions, and acceptance cases before editing. An offline saved-items screen, for example, needs success/empty/error states and persistence/relaunch checks; it does not inherently need a new navigation or dependency-injection framework.

For an empty project, establish native versus cross-platform choice, purpose, supported versions, and UI approach. Label planned architecture as intended, not detected. Generate implementation instructions without silently bootstrapping an app during setup.

## Verification

Use the repository wrapper (`./gradlew` or `gradlew.bat`) from its actual directory. Inspect build logic before execution; Gradle configuration runs code and may download dependencies. Derive commands from actual modules/variants and CI; inspect available tasks only when needed to resolve uncertainty.

Examples for a confirmed `app` module and `debug` variant are `./gradlew :app:testDebugUnitTest`, `./gradlew :app:lintDebug`, and `./gradlew :app:assembleDebug`. These are not universal commands. Preserve exit status and distinguish pre-existing failures from regressions.

- **Host tests/lint:** focused logic, transitions, validation, and static Android checks.
- **Build:** compile/package the relevant variant and identify its APK/AAB. Assembly proves no device behavior.
- **Device/emulator:** use an available authorized target and record API level/configuration. Use the discovered instrumentation task (for example `:app:connectedDebugAndroidTest` only if it exists) or a documented manual flow. Confirm the target before installation; do not casually wipe data or uninstall apps.
- **Behavior:** test the changed journey, relevant denial/offline paths, background/resume, recreation, and process death when claiming state preservation. Check relevant sizes/font scales and accessibility tools; compilation cannot prove accessibility.
- **Release:** when requested, inspect actual release variant, shrinker behavior, and packaging. Signing, Play submission, and policy compliance require separate evidence and authority. Consult current official Android documentation for version-sensitive platform/Play requirements.

Report missing SDK/JDK/device/tooling as **UNKNOWN**, identifying the prerequisite. Host, emulator, physical-device, signed-artifact, and Play-acceptance results are separate. Do not claim performance improvements without comparable measurements.

## Industry Standards (verified 2026-10 via Context7)

Sources: `developer.android.com/studio/build`, `developer.android.com/develop/ui/compose`, `developer.android.com/topic/libraries/architecture/viewmodel`

- Resolve modules, variants, and SDK levels from Gradle files before choosing a task. JVM Gradle alone is not Android evidence.
- Scope coroutines and flows to the lifecycle that owns them. Blocking work stays off the main thread.
- Permissions include denial and revocation. Exported components and intent extras are untrusted input.
- Host unit tests, lint, and device or emulator runs are separate evidence. Report a missing SDK or device as **Unknown**.

## Coding rules

Read these before writing code. User instructions and the target repo's `AGENTS.md` win on conflict; mention the conflict. Simple, flat, readable, verifiable. Write the least code that correctly solves the evidenced problem.

Generated prompts that edit Android or native code must include this section, adapted to the detected UI. Do not paste the empty-project defaults over a stack you already found.

### How to work

1. Read the existing module, naming, and a similar screen before creating anything. Match them.
2. One task, one logical diff. Do not refactor, rename, or reformat unrelated files.
3. Do not invent APIs, parameters, or library artifacts. Check the code, the version catalog when one exists, or the official docs for the version this repo uses.
4. Do not invent dependency versions. Use `gradle/libs.versions.toml` when the project has one. Otherwise add a version the way that project already pins versions, and say why the dependency is needed.
5. Build the variant you changed. Run the relevant unit tests and lint when they exist. Do not claim success for a command you did not run.
6. No `TODO()` crashes, no fake data on a production path, no stub that silently returns a default.
7. Ask when the requirement is ambiguous. Do not build a large speculative design.
8. Do not delete or weaken tests, lint rules, or error handling to make a build pass.

### Flat code

This is the usual failure mode of generated Android code.

- At most three indentation levels inside a function. Extract a function or return early past that.
- Use guard clauses. Use `when` instead of a chain of `if/else if`.
- One scope function is enough. Do not nest `let`, `also`, `apply`, or `run`.
- A function longer than about 40 lines is doing too much. A file longer than about 300 lines should be split only when the split matches the existing package layout.
- Compose: at most about three nested `Column` / `Row` / `Box` containers in one composable. Extract a named composable. Every container needs a reason. Prefer `Modifier` over a wrapper. Long lists use `LazyColumn` / `LazyRow` with a stable `key` and `contentType`, not `Column` plus `forEach`.
- Views: deep layout XML gets the same treatment. Extract an include or a custom view when the hierarchy is only there for nesting.
- React Native or Flutter UI does not become Compose. Apply the Kotlin rules to hand-written native code only. Keep the host's existing navigation, state, and list components. Do not edit generated `android/` as if it were the source of truth when a config plugin or the host framework owns it.

```kotlin
fun load(id: String?) {
    val userId = id ?: return
    val user = repo.get(userId) ?: return
    if (!user.isActive) return
    show(user)
}
```

### Layers

Do not add a layer that does not earn its keep.

- Official shape when the app is already native UI: UI (Compose or Views, plus ViewModel) → optional domain → data (repository and data sources).
- A use case exists only when more than one ViewModel shares the logic, or the logic is genuinely complex. A class that only forwards one repository call is not a use case.
- An interface needs a second implementation or a real test seam. Do not create an interface for every class.
- Do not map `Dto` → `Entity` → `Domain` → `UiModel` when the shapes are the same.
- No `Manager`, `Helper`, `Handler`, `Util`, or `Processor` dumping grounds. Name the type by what it does.
- Do not wrap Room, Retrofit, DataStore, or the existing network client in another wrapper without a reason you can point at.
- Do not create a Gradle module unless the task asks for one or build time already forces the split.
- Rule of three: do not abstract until the same code exists three times.

### Defaults for an empty native project

Use these only when detection found no UI toolkit, DI, database, or network stack. A detected stack always wins.

| Concern | Default |
|---|---|
| Language | Kotlin. No new Java |
| UI | Jetpack Compose + Material 3 |
| Architecture | MVVM, unidirectional data flow |
| Async | Coroutines + Flow. Not RxJava, not `LiveData` in new code |
| DI | Hilt, or Koin if that is what you find |
| Navigation | Navigation Compose with type-safe routes |
| Local DB | Room |
| Preferences | DataStore. Not a new raw `SharedPreferences` store |
| Network | Retrofit/OkHttp + kotlinx.serialization, or Ktor if that is what you find |
| Images | Coil |
| Background work | WorkManager |
| Build | Gradle Kotlin DSL + version catalog |

If the host is React Native, Expo, or Flutter, ignore this table for the UI layer. Kotlin defaults still apply to new hand-written native code, using the bridge style that project already has.

### State and coroutines

- State flows down. Events flow up. One immutable `UiState` per screen, exposed as `StateFlow`. The mutable flow stays private. The UI does not mutate that state.
- ViewModels hold UI state and call repositories. They do not hold `Context`, `View`, `Activity`, or `NavController`. Use `viewModelScope`. Never `GlobalScope`. Use `@ApplicationContext` only where a context is actually required, and never store an Activity in a singleton or static field.
- Prefer `collectAsStateWithLifecycle()` over `collectAsState()` when the UI is Compose. Hoist state. Use `rememberSaveable` for state that must survive recreation. New reusable composables take `modifier: Modifier = Modifier` as the first optional parameter. Add a `@Preview` when the composable can render without a device.
- One-off effects (navigation, snackbar) are consumed state, or a carefully used `Channel` / `SharedFlow`. Prefer state.
- Repositories are the single source of truth for their data. Expose `Flow` for observable data and `suspend` for one-shot work. Do not leak Retrofit or Room types into the UI. Inject dispatchers; do not hardcode `Dispatchers.IO` in every call.
- No `!!`. Prefer `val`. No `lateinit` for a constructor dependency. No `runBlocking` or `Thread.sleep` on a production path.
- Catching `Exception` in a coroutine must rethrow `CancellationException`. No empty `catch`.
- Use `coroutineScope` / `supervisorScope`. Do not use `AsyncTask`, `Handler.postDelayed` for business logic, or `onActivityResult` / `startActivityForResult` in new code.

### Platform, security, and performance

- Assume recreation and process death. Rotation does not prove process death. Keep state in the ViewModel, `rememberSaveable`, `SavedStateHandle`, or the durable store the app already uses.
- Request a permission at the point of use, with a rationale. Handle denial and "don't ask again." Request only what the feature uses. Match the API level (notifications on 33+, foreground-service types on 34+). Declare the permission and any `uses-feature` in the manifest source the project actually ships.
- WorkManager for deferrable guaranteed work. A foreground service only for user-visible ongoing work, with the correct type. No long work in `onReceive`, `onStart`, or on the main thread.
- Respect `minSdk`. Guard newer APIs or use AndroidX. Keep `targetSdk` / `compileSdk` current only when the task is that upgrade. Do not bump them to silence a warning.
- No hardcoded user-visible strings. Use `strings.xml` / `stringResource`, including plurals. No hardcoded colors or dimensions when the theme has a token. Support dark theme, font scale, and RTL. Meaningful icons need a content description; decorative ones use `null`.
- No secrets in source. HTTPS only. No trust-all `TrustManager`. No disabled certificate checks. Sensitive values use Keystore or the encrypted store the project already has, not plain text.
- `android:exported="false"` unless an external caller is required. `PendingIntent.FLAG_IMMUTABLE` unless mutation is required. Validate deep-link and intent extras. WebView JavaScript stays off unless the task needs it. Never expose `addJavascriptInterface` to untrusted content.
- Do not log tokens, passwords, or personal data. Strip verbose logs from release. Enable R8 for release and keep rules minimal. A keep rule needs an entry point you can name. Do not add a lint baseline line or a blanket `-dontwarn` to hide a new warning.
- No disk, network, or heavy work on the main thread. Stable Compose parameters, `remember`, and `derivedStateOf` where recomposition is the measured problem, not by default on every lambda. Coil (or the app's image loader) decodes at display size. Do not poll when `Flow`, a callback, or WorkManager can observe. Do not add a library for one small function.
- If the app itself calls a model: keys stay on a backend, model output is untrusted input, calls cancel with `viewModelScope`, side-effecting tools need explicit confirmation, and prompts are not logged in release. Do not add that feature because this template mentions it.

### Errors, tests, and Gradle

- Model failure as a sealed result or `UiState.Error`, not `null` or a magic value. Expected failures are visible and recoverable. Unexpected bugs crash in debug and are reported in release. User-facing text is localized, never raw exception text.
- A behavior change updates a test. Unit-test ViewModels, repositories, and pure logic with JUnit, Turbine, and `runTest` when those are the project's tools. Prefer fakes over mocks that only assert they were called. Compose UI tests use semantic matchers or test tags. Tests inject dispatchers and clocks. Do not `@Ignore` or loosen an assertion to go green.
- Version catalog and Kotlin DSL when the project already uses them. Compose BOM for Compose artifacts when Compose is already in use. Do not upgrade AGP, Kotlin, the Compose compiler, or Gradle as a side effect. Remove unused imports and dependencies your change added.
- Follow the project's formatter (ktlint, spotless, detekt) when it is configured. Do not add one mid-task. Package by feature unless the repo is already packaged by type. Comments explain why.

### Self-check

- [ ] No nesting deeper than three levels, and no giant function or composable
- [ ] No pass-through use case, single-implementation interface, or `Utils` / `Manager` type
- [ ] No new module or package the task did not ask for
- [ ] No invented API, version, `!!`, empty catch, swallowed `CancellationException`, `GlobalScope`, or `runBlocking`
- [ ] No leaked `Context` / `Activity` / `View` in a ViewModel or singleton
- [ ] Loading, empty, and error exist on the surface you changed
- [ ] Permission, manifest, and API-level guards match the new call
- [ ] No hardcoded user-visible string, secret, or unrelated formatting
- [ ] Tests cover the new behavior and were not weakened
- [ ] The build was actually run

### Done

1. The changed variant compiles.
2. Relevant tests pass, and the new behavior is covered.
3. Lint and the project's formatter pass when they exist.
4. The diff is only the task.
5. Loading, empty, and error are handled where the UI changed.
6. No new warning, hardcoded string, secret, or deprecated API.
7. The report says what changed, what you assumed, and what stayed **Unknown**.

When in doubt, choose the simpler design, the flatter structure, and the official Android approach that fits the stack you detected.
