# Data Structures

This project relies on a thorough type system describing the Blood Bowl 3 replay model and the app’s normalized output model.

Core output model: MatchData

- File: src/types/MatchData.ts
- Purpose: normalized summary produced by the processing pipeline for rendering and analytics.

MatchData fields (summary):

- `matchLog: Turn[]` — ordered list of turns
- `fanFactor`, `weather`, `kickoff` — kickoff context and rolls
- `ballPossession: { homeTeam: number; awayTeam: number }` — possession counters
- `inducements` — per-team inducements and purchased players (journeymen, stars)
- `playerData: { [playerId]: PlayerSummary }` — per-player aggregated metrics
- `conceded?: "0" | "1"` — which team conceded, if any

PlayerSummary (subset):

- Movement: `yardsMoved`, `yardsMovedWithBall`
- Passing: `passesAttempted`, `passesCompleted`, `passesCaught`, `passesDropped`, `passesIntercepted`
- Blocking: `blocksAttempted`, `blockDiceRolled`, `blockDiceTaken`, `blockingRerollsUsed`, `assistsReceived`, `pushFollowUps`, `timesPushed`, `timesRemovedFromPlay`
- Rolls: `dSixRolls` distribution
- Injuries: `armourRolls` and `injuryRolls` breakdowns

Replay input model: ReplayStep

- File: src/types/BaseTags/ReplayStep.ts
- Purpose: raw step-by-step state and events as deserialized from the XML.
- Contains: `BoardState`, phase changes, inducements, match starts, execute sequences with `StepResult[]`, and various event payloads.

Supporting types (examples):

- `messageData/*` — detailed payloads per `StepResult` message (e.g., `ResultRoll`, `ResultPlayerRemoval`, `DamageStep`, etc.)
- `Match/*` — `Turn`, `TurnAction`, and other match abstractions used by `MatchData`
- `Pitch/*` — `GamePhase`, coordinates (`XPos`, `YPos`), dice types
- `Inducements/*` — inducement types and turn structures
- `Teams/*` — `Player` and roster entities

Store-level team/player structures

- The `dataStore` uses `NotificationGameJoined`, `Roster`, `EndGame`, and `MatchData` to assemble `teamData`, `playerData`, and `competitionData` for UI accessors (logos, colours, team statistics).

Extensibility

- Add new message handlers in `processPlayerStep`/`processDamageStep` to enrich `MatchData`.
- Extend `processStep` for general board actions and kickoff sub-sequences.
- Add derived stats and summaries in the store for richer UI displays.
