# Developer Setup

Requirements

- Node.js LTS (recommended)
- pnpm/npm (project currently includes package-lock.json for npm)

Install

```bash
npm ci
```

Run dev server

```bash
npm run dev
```

Build

```bash
npm run build
```

Preview build

```bash
npm run preview
```

Linting/Type-Checking

- The project is TypeScript-based; ensure your editor uses the workspace TypeScript.
- Follow the existing code style and avoid unrelated reformatting.

Project structure (selected)

- `src/main.ts` — app bootstrap and plugin registration
- `src/plugins/*` — Vuetify, Router, Pinia, font loader
- `src/router/index.ts` — routes
- `src/store/*` — Pinia stores
- `src/composables/*` — helper functions and replay processors
- `src/types/*` — BB3 schema and app data models
- `src/views/ReplayProcessor.vue` — primary processing UI
- `src/components/Match/*` — match UI (WIP)

Development notes

- Prefer adding new message handlers to `processPlayerStep.ts`/`processDamageStep.ts` rather than embedding logic in components.
- Normalize arrays early when parsing XML (as seen in `processReplaySteps.ts`).
- Avoid catching errors without meaningful handling; surface errors to the UI if possible.
