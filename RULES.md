# RULES — Pixel Agents

## Operational Rules & Guardrails
1. **Accurate State Mapping**: Visual animation states (`idle`, `walking`, `typing`, `reading`, `waiting_for_input`, `error`) must strictly synchronize with actual agent runtime hook telemetry; speculative or fabricated animations are prohibited.
2. **Desk Allocation Collision Avoidance**: No two active agents may be assigned to the exact same desk coordinates simultaneously; if all desks in a mapped area are occupied, excess agents must route to overflow zones or wander paths.
3. **Sound Chime Throttling**: Audio notification chimes must be rate-limited (minimum 5-second interval per agent) to prevent acoustic notification fatigue during bursty permission requests.
4. **Collision-Free Pathfinding**: Character movement across the office grid must adhere to collision matrix obstacles (walls, solid furniture, closed doors); characters must never clip through solid tiles.
5. **Session Telemetry Sanitization**: Terminal output snippets displayed in speech bubbles must be filtered to prevent visual exposure of plaintext passwords, API keys, and sensitive tokens.
6. **Persistence Safety**: Office layout files (`office-layout.json`) must be saved atomically with automatic backups to prevent layout corruption during unexpected editor exits.
7. **Ephemeral Sub-Agent Lifecycle Integrity**: Ephemeral sub-agents must clean up their allocated memory, temporary character sprites, and desk assignments immediately upon process termination.
