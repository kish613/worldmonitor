# AGENTS.md

## Cursor Cloud specific instructions

### Project overview

World Monitor is a real-time global intelligence dashboard (TypeScript/Vite SPA) with three variants: World (`full`), Tech, and Finance. See `README.md` for full documentation.

### Running the application

- **Development server**: `npm run dev` (port 3000). This starts only the Vite frontend with proxy rules for external APIs. RSS and API-dependent panels show "No news available" without `vercel dev`.
- **Full local stack** (frontend + edge functions): requires Vercel CLI (`npm i -g vercel`) then `vercel dev`. This is documented in the README under "Self-Hosting".
- Variant dev servers: `npm run dev:tech`, `npm run dev:finance`.

### Lint, typecheck, test, build

- **Typecheck**: `npm run typecheck`
- **Build**: `npm run build` (also: `build:full`, `build:tech`, `build:finance`)
- **Unit/data tests**: `npm run test:data` — runs Node.js test runner on `tests/*.test.mjs`
- **Sidecar/API tests**: `npm run test:sidecar` — tests CORS, embed, cyber-threats, local API server
- **E2E tests**: `npm run test:e2e` — Playwright tests against all variants. The Playwright config auto-starts a Vite dev server on port 4173. Requires Chromium browser installed (`npx playwright install --with-deps chromium`).
- There is no separate ESLint config; linting is limited to `npm run lint:md` (markdownlint).

### Non-obvious gotchas

- The Vite dev server uses `VITE_E2E=1` to suppress auto-open and HMR during E2E testing.
- E2E visual screenshot tests (`test:e2e:visual:*`) use SwiftShader for WebGL and may produce pixel differences in cloud/headless environments. Two pre-existing test failures are expected: a visual screenshot regression in map rendering and a keyword-spike flow test.
- The `test:data` suite has a pre-existing failure in `deploy-config.test.mjs` ("keeps PWA precache glob free of HTML files") due to a regex mismatch. The `test:sidecar` suite has a pre-existing failure in `cyber-threats.test.mjs` ("API aggregates from all 5 sources").
- All API keys are optional. The dashboard works without them but some panels display "No news available" or "Failed to load" placeholders.
- Playwright uses `baseURL: http://127.0.0.1:4173` and starts its own Vite dev server — do not start a conflicting server on that port.
