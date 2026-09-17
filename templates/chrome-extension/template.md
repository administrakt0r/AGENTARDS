# Chrome Extension Stack Template

## Detected Technologies

Record evidence for manifest version, minimum Chrome version, language, UI framework (if any), package manager, bundler, test runner, and unpacked/packaged output paths. Classify **Detected / Not detected / Unknown**; do not assume dependencies.

## Project Type Detection Signals

```bash
rg --files -g '*manifest*.json' -g 'package.json' -g '*lock*' -g '*wxt.config.*' -g '*plasmo*' -g '*vite.config.*' -g '!node_modules' -g '!dist' -g '!build'
rg -n 'manifest_version|host_permissions|optional_permissions|service_worker|content_scripts|externally_connectable' --glob '*.json' --glob '!node_modules/**' --glob '!dist/**'
```

Read the actual manifest or generator configuration. A web app manifest does not establish extension support. With WXT/Plasmo or another generator, identify source entry points and inspect emitted manifests after builds; do not patch generated output. Do not migrate existing manifest versions as incidental cleanup. New Chrome extension work should use Manifest V3 unless the user supplies a justified target constraint.

## Common Stack Patterns

- Map background worker, isolated content scripts, main-world injection, popup, options, side panel, and offscreen documents actually present. Their lifetimes, DOM access, and trust boundaries differ.
- Manifest V3 workers may stop between events. Register listeners at top level, persist necessary state, and use supported events/alarms for deferred work. Do not rely on global state, persistent timers, or a permanently running worker.
- Treat page-provided data as untrusted. Validate message shape, sender/context, and allowed operations before privileged actions. Never expose a generic privileged proxy for arbitrary URLs or commands.
- Request only required API and host permissions. Prefer temporary `activeTab` access or optional permissions when sufficient. Handle denial/revocation; justify any broadened permissions against the requested feature.
- Respect extension CSP and packaged-code constraints. Do not introduce remote executable code, `eval`, or unsafe HTML insertion as workarounds. Inspect web-accessible resources and external connection scope.
- Choose storage by lifetime, quota, and sensitivity; handle asynchronous errors, quota failures, and upgrades. Bundled code and extension storage are not secret vaults.

## Agent Customizations

| Owner | Focus |
|-------|-------|
| `hunter` | Bounded behavior repair or authorized feature within the existing architecture |
| `api` | Message contracts, sender validation, request/response failures, external APIs |
| `sentinel` | Permissions, host scope, page-to-extension trust, CSP, exposed resources, sensitive data |
| `picasso` | Popup/options/side-panel keyboard flow, focus, loading/error/empty states, constrained sizing |
| `bolt` | Measured content-script overhead, worker wakeups, messaging volume, bundle cost |
| `testing` | Contract/storage tests and integration with the actual packaged extension |
| `cicd`, `docs` | Packaging and load/test instructions; publication only when authorized |

Use selected roles only and hand off cross-domain findings.

## Development Task Pattern

Specify the requested feature's trigger, execution context, data flow, minimum permissions, persisted state, UI, and failures before editing. For example, “save this page” needs an explicit trigger, storage contract, visible success/failure, and reload persistence check; it does not inherently need all-host access. Follow existing architecture instead of adding a framework solely for the task.

## Verification

1. Run discovered scripts with the actual package manager. Validate emitted manifest and referenced worker/UI/content-script/icon files.
2. Exercise contracts and failure paths with available tests. Mocks do not prove browser permissions or worker lifecycle behavior.
3. Load the built extension in a dedicated test profile or supported extension-capable harness. Record browser version, artifact path, and flows. Do not assume a default headless browser supports extensions.
4. Test relevant UI/content-script interaction, worker stop/restart, persisted state, denied/revoked access, and errors. Close worker DevTools when testing normal suspension because debugging can keep it alive.
5. Inspect the actual distributable when packaging is in scope. Exclude secrets, profiles, and unrelated development files. Consult current official Chrome documentation for version-sensitive APIs and store rules.

Keep static/build, browser, lifecycle, and store-review results separate. Missing browser tooling means runtime **UNKNOWN**. Loading unpacked code does not prove Chrome Web Store acceptance. Do not submit, publish, or change the user's everyday browser profile without authorization.
