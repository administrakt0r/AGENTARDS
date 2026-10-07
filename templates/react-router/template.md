# React Router 7 SSR Stack Template

## Detected Technologies

- **Framework:** React Router 7 (`react-router`, `@react-router/dev`) with Server-Side Rendering (SSR)
- **Bundler:** Vite 8 (`vite`, `@react-router/dev/vite`)
- **Language:** TypeScript
- **Styling:** Tailwind CSS v4 (`@tailwindcss/vite`, `tailwindcss`)
- **Testing:** Vitest 5 (`vitest`)
- **Components:** Radix UI primitives, shadcn-style architecture, Lucide icons

## Stack Detection Signals

```bash
rg --files -g 'app/root.tsx' -g 'app/routes.ts' -g 'app/routes/**' -g 'vite.config.*'
rg -n '"react-router":|"@react-router/dev":' package.json 2>/dev/null
```

## Common Stack Patterns & Anti-Patterns

### React Router 7 Architecture
- **Route Registration:** Explicit routes in `app/routes.ts` or file-based routing in `app/routes/`.
- **Server/Client Separation:** Files ending in `.server.ts` are strictly compiled into the server bundle. Ensure server-only dependencies (e.g. database clients, cryptographic secrets, admin auth) are never imported directly into client components.
- **Loaders and Actions:** Loaders run on the server for initial SSR and client transitions. Actions handle mutations with redirect or JSON data envelopes.
- **Type Generation:** Run `react-router typegen` to generate strongly-typed route params, loader data, and action results into `.react-router/types/`.

### Bundle Size & Performance Budgets
- **Budget Thresholds:** Enforce strict bundle budgets (e.g. via `scripts/check-budgets.mjs`) for main CSS, route JS, shared chunks, and entry client JS.
- **Code Splitting:** Heavy libraries (e.g. phone parsing, syntax highlighters) should be dynamically imported or confined to specific route chunks.
- **Tailwind CSS v4:** Uses `@tailwindcss/vite` without requiring `postcss.config.js`. Avoid dynamic class strings that prevent static class name purging.
- **Hydration Mismatches:** Avoid rendering client-only state (e.g. `window.innerWidth`, client timestamps) during initial SSR render without mounted guards.

### Testing Strategy
- **Vitest Unit Tests:** Test route loaders and actions by mocking the `LoaderFunctionArgs` and `ActionFunctionArgs` request contexts.
- **Component Tests:** Test critical UI features with React Testing Library or Vitest DOM tests.

## Industry Standards (verified 2026-10 via Context7)

Sources: `reactrouter.com/en/main`, `tailwindcss.com/docs`, `vitest.dev/`

- **React Router 7**: Successor to Remix. Uses Vite plugin `@react-router/dev/vite`. Route data loading is managed through `loader` and `action` functions with automatic revalidation.
- **React 19 Compatibility**: React 19 supports Actions, `useActionState`, `useFormStatus`, and automatic compiler memoization.
- **Tailwind CSS v4**: High-performance CSS engine written in Rust. Configured directly in CSS using `@theme` and `@utility` directives.

## Agent Mapping

| Owner | Focus |
|---|---|
| `bolt` | Route loader parallelization, client bundle budgets, code-splitting, SSR streaming |
| `picasso` | Responsive layouts, WCAG 2.1 AA accessibility, tap targets, Tailwind v4 design tokens |
| `custodian` | Unused route exports, obsolete UI primitives, dead styles in global CSS |
| `testing` | Route loader/action unit tests, component render tests, hydration safety |
| `sentinel` | Form CSRF/same-origin validation in actions, input sanitization, error boundaries |
| `seo` | Meta function definitions, canonical links, JSON-LD structured data, sitemaps |

## Verification Commands

```bash
react-router typegen && tsc -b
react-router build
node scripts/check-budgets.mjs
pnpm exec vitest run
```
