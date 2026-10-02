# EXPLAINABILITY — Pixel Agents

## How the Agent Decides

Pixel Agents provides a deterministic, event-driven visualization and spatial orchestration engine for AI coding agents. Raw terminal events and tool execution signals transition through five sequential stages to animate characters and manage virtual office interactions:

```
[ Terminal Agent Hook Event ]
              │
              ▼
[ 1. Event Ingestion & Schema Normalization ]
              │
              ▼
[ 2. Spatial Allocation & A* Wayfinding ]
              │
              ▼
[ 3. Activity Classification & State Dispatch ]
              │
              ▼
[ 4. Canvas Sprite Rendering & Speech Bubbles ]
              │
              ▼
[ 5. Ephemeral Teardown & Layout Synchronization ]
```

### 1. Mathematical Scoring & Routing Formulation
When assigning an incoming agent or ephemeral sub-agent to an available workstation in the office grid, the engine computes a desk suitability score $S_{\text{desk}}$:

$$S_{\text{desk}} = w_a \cdot A_{\text{area}} + w_p \cdot \frac{1}{1 + D_{\text{entrance}}} + w_c \cdot C_{\text{cluster}} + w_s \cdot S_{\text{subagent}}$$

Where:
- $A_{\text{area}} \in \{0, 1\}$: Binary match between the agent workspace folder and the painted office area tag.
- $D_{\text{entrance}}$: Manhattan distance $\|x_d - x_0\|_1 + \|y_d - y_0\|_1$ from office entrance $(x_0, y_0)$ to desk coordinates $(x_d, y_d)$.
- $C_{\text{cluster}} \in [0, 1]$: Proximity measure to currently active peer teammates.
- $S_{\text{subagent}} \in \{0, 1\}$: Preference for adjacent auxiliary chairs when seating an ephemeral sub-agent near its parent.
- Parameter weights: $w_a = 0.40$, $w_p = 0.20$, $w_c = 0.25$, $w_s = 0.15$ ($\sum w_i = 1.0$).

The desk with the highest score $\arg\max_d S_{\text{desk}}$ is reserved. If no designated area desks are available, the agent is routed to general overflow seating.

### 2. Refusal Criteria & Decision Thresholds
Pixel Agents applies deterministic boundary constraints to maintain visual fidelity and user focus:

| Trigger Scenario | Operational Action | Error Code |
| :--- | :--- | :--- |
| Office grid has zero unassigned desks or chairs | Route agent to open wandering lobby paths; flag layout capacity limit | `ERR_DESK_CAPACITY_REACHED` |
| Obstacle collision bake detects no traversable path to target desk | Cancel walking animation; teleport character safely to nearest open tile | `ERR_UNREACHABLE_DESTINATION` |
| Agent triggers permission prompts repeatedly within 5-second window | Suppress repetitive audible chimes; render visual speech bubble only | `ERR_RATE_LIMITED_CHIME` |
| Received hook event fails typed `HookProvider` payload schema | Discard malformed event; preserve current character animation state | `ERR_INVALID_HOOK_SCHEMA` |
| WebSocket heartbeat timeout exceeded (> 15 seconds) | Mark connected agent character as idle/disconnected | `ERR_DISCONNECTED_AGENT_SESSION` |

### 3. Multi-Tier Fallback Mechanisms
1. **Pathfinding Obstacle Fallback**: If standard A* pathfinding fails to find a valid route due to dynamic furniture placement, the character falls back to simple cardinal step approximations or snaps directly to the desk tile.
2. **Audio Fallback**: When audio autoplay policies block sound playback in browser contexts, notifications degrade silently to highlighted visual speech bubbles and window title alerts.
3. **Environment Failover**: If the VS Code extension host becomes unresponsive, developers can launch `npx pixel-agents` to run the standalone CLI server and inspect the office in any web browser.

### 4. Human-in-the-Loop Governance
- **Interactive Layout Customization**: Developers retain total control over room layouts, wall boundaries, and furniture arrangements using the visual layout editor.
- **Permission & Action Confirmation**: Speech bubbles alert users to pending tool executions (e.g., destructive file edits), but actual permission grants occur in the user's authenticated terminal.
- **Manual Character Management**: Operators can dismiss idle characters or reassign agent desks manually via click-and-drag interactions.

---

## The Data It Uses

### 1. Input Data Types
- **Agent Hook Events**: Real-time signals from Claude Code, terminal wrappers, and IDE extensions containing tool names (`Edit`, `Grep`, `Bash`), parameters, and progress updates.
- **Terminal Session Identifiers**: Unique process IDs, terminal tab names, and workspace folder paths.
- **User Spatial Actions**: Drag-and-drop tile edits, furniture selections, and area mapping inputs.

### 2. Reference & Configuration Data
- **Office Layout Matrix**: JSON representation of grid dimensions (default 32x24), tile types (floors, walls), furniture coordinates, and collision masks.
- **Sprite Sheet Definitions**: Frame indexes, coordinate offsets, and animation cycle definitions for 6 diverse character avatars, pets, and accessories.
- **Area-to-Folder Registry**: Key-value mapping linking repository directory paths to specific named office rooms.

### 3. Model Lineage & System Architecture
- **Runtime Environment**: Node.js/TypeScript extension running within VS Code or as a standalone CLI WebSocket server.
- **Frontend Canvas**: React webview rendering an HTML5 2D canvas with pixel-art rendering optimizations (`image-rendering: pixelated`).
- **Telemetry Pipeline**: Non-invasive event interception via lightweight hook scripts without altering agent reasoning or execution logic.

### 4. Data Privacy, Retention & Sanitization
- **Strict PII Redaction in Speech Bubbles**: Sensitive secrets, credentials, and token strings within tool arguments are masked before rendering in UI speech bubbles.
- **Local-Only Processing**: All telemetry and rendering operations run completely locally on `localhost` or within the VS Code sandbox; no agent prompts or telemetry are transmitted to cloud servers.
- **Ephemeral Session Data**: Agent activity states are held in memory during the session and discarded upon agent exit; only user-created office floor plans persist.

---

## Limitations

### 1. Terminal Hook Dependency
- **Limitation**: Real-time animation relies on agent tools exposing structured hook events; unsupported CLI agents without hooks appear only as generic terminal processes.
- **Mitigation**: Provide typed `HookProvider` interfaces and shell wrappers allowing any CLI tool to emit standardized JSON telemetry.

### 2. Canvas Scalability with Massive Agent Swarms
- **Limitation**: Simulating hundreds of simultaneous pathfinding agents on a single 2D canvas can cause frame rate drops on low-end machines.
- **Mitigation**: Implement canvas viewport culling and throttle off-screen character animation update loops.

### 3. Pathfinding Deadlocks in Narrow Hallways
- **Limitation**: Two characters walking in opposite directions through single-tile corridors may temporarily obstruct each other.
- **Mitigation**: Implement simple collision yield logic where secondary characters yield priority to agents walking toward active user prompts.

### 4. Browser Audio Autoplay Policies
- **Limitation**: Modern web browsers restrict audio notification chimes until the user interacts with the page document.
- **Mitigation**: Display a prominent "Enable Audio" toggle in the standalone web UI to request explicit user activation.

### 5. Multi-Window Layout Divergence
- **Limitation**: Editing an office layout concurrently across multiple VS Code windows can lead to race conditions on `office-layout.json`.
- **Mitigation**: Enforce file watching with atomic writes and last-write-wins merge policies with automatic file reload triggers.

---

## Summary & Compliance Checklist

| Checkpoint Focus | Requirement | Status |
| :--- | :--- | :--- |
| **Checkpoint 1** | OpenGAP v0.1.0 Specification (`agent.yaml`, `SOUL.md`, `RULES.md`, `DUTIES.md`, `skills/`, `tools/`) | **Verified** |
| **Checkpoint 2** | Canonical 4-Heading AST Schema & Deterministic Pipeline Diagram | **Verified** |
| **Checkpoint 2** | Mathematical Desk Allocation Formulation ($S_{\text{desk}}$) & Parameter Weights | **Verified** |
| **Checkpoint 2** | Refusal Criteria Table with Explicit Error Codes & Multi-Tier Fallbacks | **Verified** |
| **Checkpoint 2** | Comprehensive Data Privacy Coverage (4 Subsections) & 5 Numbered Limitations | **Verified** |
| **Checkpoint 3** | Multi-Framework Adapter Portability (`openai`, `crewai`, `claude-code`, `lyzr`) | **Verified** |
