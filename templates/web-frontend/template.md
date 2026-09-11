# Web Frontend Stack Template

## Detected Technologies
- **Framework:** [React/Vue/Angular/Svelte/Next.js/Nuxt/etc.]
- **Language:** [JavaScript/TypeScript]
- **Package Manager:** [npm/yarn/pnpm/bun]
- **Build Tool:** [Vite/Webpack/Parcel/Turbopack/etc.]
- **Test Runner:** [Jest/Vitest/Playwright/Cypress/etc.]
- **CSS Approach:** [Tailwind/CSS Modules/styled-components/Sass/etc.]
- **Linting:** [ESLint/Prettier/etc.]

## Common Stack Patterns

### Routing
- Client-side: React Router, Vue Router, file-based (Next.js, Nuxt)
- Routes are typically in `src/routes/`, `app/`, `pages/`, or `src/pages/`
- Route definitions: `src/routes.tsx`, `src/App.tsx`, or file-system based

### State Management
- React: useState, useReducer, Context, Redux, Zustand, Jotai, Recoil
- Vue: ref/reactive, Vuex, Pinia
- Store files: `store/`, `stores/`, `src/store/`, `context/`

### Styling
- Tailwind: `tailwind.config.js/ts`
- CSS Modules: `*.module.css`
- styled-components: inline
- Sass: `*.scss`, `*.sass`

### Components
- React: Functional components with hooks (`.tsx`, `.jsx`)
- Vue: SFC (`.vue`)
- Angular: Components (`.component.ts`)
- Svelte: SFC (`.svelte`)
- Component directories: `components/`, `src/components/`, `app/components/`

### Testing
- Unit: Jest, Vitest → `*.test.ts`, `*.spec.ts`, `__tests__/`
- E2E: Playwright, Cypress → `e2e/`, `tests/e2e/`, `cypress/`
- Component: React Testing Library, Vue Test Utils

### Performance Concerns
- Bundle splitting: dynamic import, React.lazy, defineAsyncComponent
- Image optimization: next/image, nuxt-image, sharp
- Font optimization: next/font, @fontsource
- Lazy loading: IntersectionObserver, dynamic imports
- Bundle analysis: webpack-bundle-analyzer, vite-plugin-visualizer

### Common File Locations
```
src/
├── components/          # Shared components
├── hooks/               # Custom hooks (React)
├── pages/ or app/       # Route-level components
├── layouts/             # Layout components
├── styles/              # Global styles
├── utils/               # Utility functions
├── services/            # API calls
├── store/               # State management
├── types/               # TypeScript types
└── assets/              # Static assets
```

## Project Type Detection Signals
- `package.json` with react/vue/angular/svelte dependency
- `next.config.js/ts/mjs` or `nuxt.config.ts` present
- `vite.config.ts/js` or `webpack.config.js` present
- `tsconfig.json` present
- `src/` directory with components
- HTML entry point: `index.html`, `public/index.html`

## Agent Customizations by Sub-Type

### React
- Component patterns: functional components, hooks
- State: useState, useEffect, useContext, custom hooks
- Styling: CSS Modules, Tailwind, styled-components
- Key files: `src/App.tsx`, `src/index.tsx`, `src/main.tsx`

### Vue
- Component patterns: SFC, Composition API, Options API
- State: ref, reactive, Pinia stores
- Styling: scoped styles, Tailwind
- Key files: `src/App.vue`, `src/main.ts`

### Next.js / Nuxt
- File-based routing
- SSR/SSG/ISR patterns
- API routes: `pages/api/` or `server/api/`
- Data fetching: getServerSideProps, useFetch, useAsyncData

### Angular
- Component patterns: decorators, services, dependency injection
- State: NgRx, Akita, services
- Styling: component styles, SCSS
- Key files: `src/app/app.module.ts`, `angular.json`

### Svelte
- Component patterns: reactive declarations, stores
- State: writable, readable, derived stores
- Styling: scoped styles
- Key files: `src/routes/+page.svelte`, `src/app.html`

## Common Issues
- XSS via `dangerouslySetInnerHTML` / unsanitized HTML
- Stale closures and effect cleanup/memory leaks
- Unnecessary re-renders / missing memoization
- Bundle bloat and duplicate dependencies
- Accessibility gaps (focus, labels, contrast)
- Hydration mismatches in SSR frameworks

## Testing Patterns
- Unit: Jest / Vitest
- Component: React Testing Library / Vue Test Utils
- E2E: Playwright / Cypress
- `tsc --noEmit`, ESLint, stylelint
- Build smoke: `npm run build` then preview
