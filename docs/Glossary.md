# Glossary

This glossary explains common Blood Bowl 3 and app-specific terms used in the codebase.

- Game Phase: A numbered phase of the match flow (setup, kickoff, play, etc.).
- Inducements: Temporary enhancements purchased pre-match (e.g., Bribes, Wizard, Star Players, Journeymen).
- Journeyman: Temporary player added to a team to meet minimum roster size.
- Star Player: Special named player hired for a single match.
- Turn: A single team’s opportunity to act; two turns per full round (home/away).
- TurnAction: A collection of events associated with a single player’s action during a turn.
- Step / StepResult: Atomic message structures emitted in replays that describe actions and outcomes.
- Block Dice: Special dice in blocking actions (e.g., Attacker Down, Both Down, Push, Defender Stumbles, Defender Down).
- KO: Knocked Out; player removed to KO box and may return later.
- Casualty: Serious injury outcome; may include lasting effects.
- Possession: Whether a team controls the ball; tracked as a counter per turn.
- BBR: Blood Bowl Replay container format used by BB3 for saved replays.
