# Project Layout

Key directories and files

- `src/main.ts` — app entry; registers Vuetify, Router, Pinia; mounts App
- `src/plugins/` — framework and plugin setup
  - `index.ts` — registers plugins and fonts
  - `vuetify.ts` — theme and icon setup
  - `webfontloader.ts` — font loading
- `src/router/index.ts` — route configuration (default layout, ReplayProcessor, Git)
- `src/layouts/default/` — app shell and app bar
- `src/store/` — Pinia stores
  - `index.ts` — createPinia()
  - `dataStore.ts` — domain data (teams, players, matchData, logos/colours)
  - `app.ts` — general app store
- `src/components/` — UI components
  - `Processor.vue` — file input + processing flow (legacy/alt UI)
  - `Match/` — match visualization components (Wrapper, Overview, Logs, Stats)
- `src/views/` — route views
  - `ReplayProcessor.vue` — primary processing UI
  - `Home.vue` — placeholder
  - `Git.vue` — about/git view
- `src/composables/`
  - `helperFns/` — Base64, unzip, XML processing, HTML decoding, die roll helpers
  - `matchProcessingFunctions/` — replay → MatchData processors
- `src/types/` — type system for BB3 replay schema and app models
  - `BaseTags/ReplayStep.ts` — top-level step schema
  - `MatchData.ts` — normalized output model
  - `messageData/*`, `Match/*`, `Pitch/*`, `Inducements/*`, etc.
- `public/` — static assets
- `dist/` — build output (Vite)

Notes

- Avoid coupling UI components to parsing logic; prefer composables and stores.
- Assets include team/skill icons under `src/assets/` and are referenced by helper functions and stores.
