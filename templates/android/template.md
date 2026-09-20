# Android Stack Template

## Detected Technologies

Record evidence for Kotlin/Java or a cross-platform host, Gradle wrapper, Android Gradle Plugin, JDK/SDK availability, modules/variants, `minSdk`/`targetSdk`/`compileSdk`, Compose/XML UI, persistence, and testing. Classify **Detected / Not detected / Unknown**. Do not assume an `app` module, `debug` variant, installed SDK, or connected device.

## Official Android Skills

Use the official Android agent skills from [`android/skills`](https://github.com/android/skills) when the task matches one of them. They are installed for Codex in `~/.codex/skills/` and for OpenCode in `~/.opencode/skills/`; load the relevant local `SKILL.md` before acting, and follow its required references or scripts. Select only relevant skills, preserve the repository's existing architecture, and report unavailable tooling or unrun device checks as **Unknown** rather than treating a skill's instructions as evidence.

## Project Type Detection Signals

```bash
rg --files -g 'settings.gradle*' -g 'build.gradle*' -g 'gradle-wrapper.properties' -g 'libs.versions.toml' -g 'AndroidManifest.xml' -g 'gradlew*' -g '!build' -g '!.gradle' -g '!node_modules'
rg -n 'com.android.application|com.android.library|org.jetbrains.kotlin.android|compileSdk|targetSdk|minSdk|compose' --glob '*.gradle' --glob '*.gradle.kts' --glob '*.toml' --glob '!**/build/**'
```

Inspect settings, convention plugins, version catalogs, manifests, and source sets to resolve aliases and actual modules. JVM Gradle alone is not Android evidence. For Expo/React Native/Flutter, identify generated native files before editing. Do not expose `local.properties`, keystores, signing passwords, or environment values.

## Common Stack Patterns

- Follow the existing Compose or Views architecture. Trace state through UI, ViewModel, data, and navigation; do not impose a new architecture for a small feature.
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
