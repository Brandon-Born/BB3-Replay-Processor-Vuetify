# Processing Pipeline

This describes how a `.bbr` file is processed into XML and then into `MatchData`.

1) File selection and validation

- User selects a `.bbr` file via UI (ReplayProcessor view / Processor component).
- If no file is selected, the UI shows a hint and blocks processing.

2) Decode and unzip

- `decodeBase64File(file)` => Base64 decode the `.bbr` file contents.
- `unzipFile(decoded)` => extract embedded replay XML payload(s).
- `processXML(unzipped)` => recursively Base64-decode nested fields; return a `Document`.

3) Sanitize and download (optional)

- Before download, IP addresses (`<IpAddress>`) are removed from the XML DOM.
- XML is serialized and HTML entities decoded via `decodeHtml`.

4) Step parsing and normalization

- The XML is read as a sequence of replay steps (`ReplayStep`).
- Arrays are normalized (e.g., `EventExecuteSequence`, `StepResult`, `StringMessage`) to simplify iteration.

5) Initialize MatchData

- `MatchData` initialized with:
  - `matchLog: Turn[]` (empty)
  - `playerData` map
  - `inducements` (home/away objects)
  - `ballPossession` counters
- Seed players from initial `BoardState` using `addBasePlayerData`.

6) Iterate steps and process by type

- Each step updates current phase, turn state, and possession.
- `EventBuyMercenary`/inducement-related events enrich `matchData.inducements` and player roster.
- `EventMatchStart`, `EventKickingChoice`, `EventEndTurn`, `EventTouchdown`, etc., update match context.
- `EventExecuteSequence` contains `StepResult` items of three main kinds:
  - `PlayerStep` => handled by `processPlayerStep`
  - `DamageStep` => handled by `processDamageStep`
  - `Step` => general board actions (placeholder in `processStep`)

7) PlayerStep processing

- Builds/updates `TurnAction` entries per acting player.
- Tracks movement, pass/handoff attempts, catches, pickups, block outcomes, rerolls (future), and sent offs.
- Derives success/failure from `ResultRoll` messages and message data.

8) DamageStep processing

- Aggregates armour/injury rolls and outcomes.
- Updates per-player d6 distributions, armour/injury counters.
- Records injuries/removals on turns and actions.

9) Turn finalization and possession tracking

- On `EventEndTurn`, finalize the current/next `TurnAction` entries and push `Turn` to `matchLog`.
- Increment team possession by checking the ball carrier at turn end.

10) Output

- The resulting `MatchData` contains a summarized, queryable structure for UI rendering and analysis.

Notes and caveats

- `processStep.ts` is a stub for general board actions and can be extended.
- Phase handling is present but some early-phase nuances are commented for future refinement.
- Some result message types are parsed only as needed to support current analytics.
