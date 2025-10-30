# Architecture

This project is a Vue 3 + TypeScript application using Vuetify for UI, Pinia for state management, and Vue Router for navigation.

Core entry points:

- src/main.ts: creates the Vue app and mounts it. Registers plugins.
- src/plugins/index.ts: registers Vuetify, Router, and Pinia and loads fonts.
- src/plugins/vuetify.ts: sets up Vuetify theme and icons.
- src/router/index.ts: configures routes, including the ReplayProcessor view.
- src/store/index.ts: initializes Pinia.

State stores:

- src/store/dataStore.ts (Pinia Composition API):
  - Holds `notificationGameJoined`, `rosters`, `endGame`, and `matchData`.
  - Derives per-team `teamData`, `competitionData`, and per-player `playerData`.
  - Provides getters like `getPlayerName`, `getTeamName`, `getTeamColours`, `getTeamLogo`, and `getCompetitionLogo`.
  - `setTeamData()` fuses roster info with end-game stats and inducement-added players.
- src/store/app.ts: placeholder store for app-level state.

Views and components:

- src/views/ReplayProcessor.vue: primary UI for loading/processing `.bbr` files, exposing download of sanitized XML. Renders `Match/Wrapper.vue` for processed output.
- src/components/Processor.vue: legacy/alternate processor UI (file input + processing functions) — demonstrates end-to-end decode/unzip/process pipeline.
- src/components/Match/*: match visualization and subcomponents (WIP).

Processing composables (core logic):

- src/composables/helperFns/*: utilities for Base64 decoding, unzip, XML handling and conversions.
- src/composables/matchProcessingFunctions/*: transforms replay steps into `MatchData`.
  - `processReplaySteps.ts`: orchestrates per-step processing, manages phases, turns, ball possession, and inducements. Seeds `MatchData` and delegates to specialized processors.
  - `processPlayerStep.ts`: processes player actions (move, pass, handoff, catch), dice outcomes, and updates current/next `TurnAction` structures.
  - `processDamageStep.ts`: processes armour/injury/casualty-related rolls and removals, updating player and turn statistics.
  - `processStep.ts`: placeholder for general board actions (currently returns early; documented as future work).
  - `addBasePlayerData.ts`: initializes per-player metrics in `matchData.playerData`.

Types and domain modeling:

- src/types/**: comprehensive type system for BB3 replay schema.
  - `BaseTags/ReplayStep.ts`: top-level per-step payload (BoardState, Events...).
  - `MatchData.ts`: the app's normalized output model aggregating match summary.
  - `messageData/*`, `Match/*`, `Pitch/*`, `Inducements/*`, etc.: fine-grained types for event/message structures.

Routing:

- `/` => `ReplayProcessor.vue`
- `/git` => `views/Git.vue`

Theming:

- Vuetify theme defines `primary: #03045E` and `secondary: #5CBBF6`.

Build/output:

- Vite-based project; built assets in `dist/`.
