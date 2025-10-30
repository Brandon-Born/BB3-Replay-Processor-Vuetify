# File Formats

Input: .bbr (Blood Bowl 3 replay file)

- Container: Base64-encoded content that contains a zip archive
- Contents: replay XML and related data

Processing steps

- Base64 decoding of the .bbr payload
- Unzipping to extract XML
- Recursively Base64-decoding nested message payloads inside XML
- Converting XML fragments into JSON for message-specific parsing

Output: processed XML (optional download)

- Before download, `<IpAddress>` nodes are removed
- XML is serialized and HTML entities decoded
- Filename used by the app: `processed_replay.xml`

Output: MatchData (internal)

- A normalized TypeScript structure (`src/types/MatchData.ts`) that summarizes turns, actions, dice outcomes, injuries, inducements, and ball possession.
- Used by UI and stores for rendering and analytics.
