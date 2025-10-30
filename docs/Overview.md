# Overview

The BB3 Replay Processor is a Vue 3 + Vuetify application that lets users upload Blood Bowl 3 replay files (.bbr), decode and unzip them to XML, and process the replay events into structured MatchData for analysis and visualization.

High-level workflow:

- User selects a .bbr replay file in the UI
- The app decodes (Base64) and unzips to obtain the embedded XML
- The XML can be sanitized (e.g., IP addresses removed) and downloaded
- Replay events are parsed and summarized into MatchData (turns, actions, rolls, injuries, inducements, possession)

Key capabilities:

- Decode .bbr files and reconstruct replay XML
- Recursively decode nested Base64 in XML payloads
- Transform XML fragments to JSON for step-level processing
- Build comprehensive MatchData from replay steps
- Aggregate player metrics and team possession
- Track inducements (journeymen, star players, purchases)

Non-goals:

- Full rules engine simulation (this is a parser/processor)
- Live match telemetry (operates on saved replays)

Current status: the UI view (views/ReplayProcessor.vue) shows the app is under reconstruction; core processing and models are implemented and evolving.
