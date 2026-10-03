# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **Pixel Agents Simulator** (`pixel-agents`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** Pixel Agents Simulator (`pixel-agents`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Multi-Agent Visual Orchestration & Workspace Gamification  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

Pixel Agents transforms background AI coding agent trajectories into an interactive, visual pixel art office simulator within VS Code. Rather than opaque terminal logs or hidden processes, agents appear as animated sprites occupying office desks, walking to water coolers, typing when writing code, and celebrating when tests pass. The decision engine maps asynchronous agent hook events to deterministic spatial and visual states.

### 1. Decision Architecture

The runtime telemetry intake, state classification, desk allocation, animation rendering, and sound dispatch operate across a deterministic, five-stage pipeline:

```
[ Inbound Agent Hook Telemetry / Shell Output Stream ]
                         │
                         ▼
[Stage 1: Agent Telemetry & Hook Ingestion Gate]
  - Intercepts pre/post tool execution hooks from CLI agents (Claude Code, Cline, Roo)
  - Parses process IDs, command exit codes, and active file paths
  - Normalizes event types into standard activity payloads
                         ▼
[Stage 2: Activity State Mapping & Mood Classification Gate]
  - Classifies agent state (Thinking, Coding, Reading, Testing, Idle, Error)
  - Evaluates mood modifier based on recent tool success/failure history
  - Dispatches activity payload to spatial layout coordinator
                         ▼
[Stage 3: Spatial Office Layout & Desk Allocation Gate]
  - Computes optimal desk placement using spatial collision avoidance
  - Schedules pathfinding waypoints between conference rooms and desks
  - Enforces office capacity ceilings (N_agents <= 32)
                         ▼
[Stage 4: Sprite Animation & Chime Dispatch Gate]
  - Triggers pixel animation frame sequences (16x16 / 32x32 sprite sheets)
  - Emits contextual retro sound chimes for completed milestones
  - Updates React webview canvas state via bidirectional postMessage
                         ▼
[Stage 5: Multi-Agent Synchronization & Trajectory Logging Gate]
  - Synchronizes multi-agent positions across active editor panes
  - Commits agent session trace metrics to local extension storage
  - Broadcasts state updates to connected external companion webviews
                         ▼
[ Animated Virtual Office Canvas Rendered to Developer ]
```

### 2. Decision Logic & Routing Formulations

Pixel Agents evaluates activity urgency, desk affinity, and layout density using deterministic mathematical models:

1. **Activity Urgency & Animation Score ($S_{\text{activity}}$)**:
   $$S_{\text{activity}}(e) = (w_t \cdot T_{\text{type}}) + (w_c \cdot C_{\text{duration}}) + (w_s \cdot S_{\text{status}})$$
   where:
   - $T_{\text{type}} \in [0, 1]$ represents priority of event (Write: 1.0, Exec: 0.8, Read: 0.5, Idle: 0.1).
   - $C_{\text{duration}} = \min(1.0, \Delta t / 60)$ scales with command runtime duration.
   - $S_{\text{status}} \in \{0, 1\}$ indicates active execution vs completed state.
   - Weights: $w_t = 0.50, w_c = 0.30, w_s = 0.20$ ($\sum w_i = 1.0$).

2. **Workspace Congestion & Layout Index ($I_{\text{layout}}$)**:
   $$I_{\text{layout}} = \frac{N_{\text{active}}}{N_{\text{desks}}} \cdot \left(1 + \frac{D_{\text{collisions}}}{D_{\text{total}}}\right)$$
   where $N_{\text{active}}$ is currently spawned agents and $N_{\text{desks}}$ is available furniture tiles. If $I_{\text{layout}} \ge 0.85$, the engine triggers desk clustering and automatic canvas room expansion.

### 3. Thresholding & Refusal Decision Criteria

Pixel Agents enforces strict operational safeguards to protect developer workspace performance:
- **Refusal on Capacity Overflow**: Requests to spawn more than 32 active agents in a single workspace are refused to preserve webview frame rates (`ERR_DESK_CAPACITY_EXCEEDED`).
- **Refusal on Unknown Event Schema**: Inbound hook payloads failing JSON schema validation are dropped with code `ERR_UNKNOWN_ACTIVITY_STATE`.
- **Refusal of Unverified External Sockets**: WebSocket connections lacking local authorization tokens are rejected (`ERR_UNAUTHORIZED_HOOK_DISPATCH`).
- **Audio Chime Throttling**: Sound effects occurring within 250ms of each other are deterministically throttled (`WARN_CHIME_RATE_LIMITED`).
- **Render Timeout Enforcement**: Canvas redraw operations exceeding 16ms (60 FPS budget) trigger LOD sprite simplification (`WARN_FRAME_BUDGET_EXCEEDED`).

### 4. Fallback Decision Mechanism

Operational stability across diverse IDE installations is maintained through layered fallbacks:
- **Stateless Webview Degradation**: If WebGL hardware acceleration is disabled in VS Code, rendering falls back gracefully to standard HTML5 2D canvas context.
- **Silent Mode Audio Fallback**: When audio devices are unavailable or muted in extension settings, audio synthesis degrades to subtle visual notification badges.
- **Polling Fallback**: If WebSocket push streams fail, the extension polls local agent state files via standard filesystem watchers.
- **Model Fallback Cascade**: High-level activity description generation defaults to `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.

### 5. Human-in-the-Loop Governance

Developers retain complete authority over the visual simulation environment:
- **Interactive Desk Reassignment**: Developers can drag and drop agent sprites to assign specific tasks, desks, or conference rooms.
- **Privacy & Do-Not-Disturb Controls**: A global toggle instantly mutes audio chimes, hides animations, and collapses the webview into a minimal status bar item.
- **Agent Process Termination**: Clicking on an errant agent sprite exposes a direct process kill button to safely abort hanging terminal tasks.

---

## The Data It Uses

Pixel Agents operates under strict principles of data minimization, local environment isolation, and developer privacy.

### 1. Ingested Input Data

The framework processes only operational telemetry necessary to visualize developer agents:
- **Hook Payloads**: Tool names, executed shell command strings, exit codes, and file paths.
- **Agent Identity Data**: Agent names, roles (Coder, Reviewer, Tester), and associated process IDs.
- **User IDE Events**: Active editor switches, workspace window resize events, and theme changes.

### 2. Configuration & Reference Data

- **Sprite Asset Sheets**: PNG pixel art character sprites, desk furniture tiles, and office décor.
- **Sound Asset Packs**: 8-bit retro audio chimes and notification sound samples.
- **Office Layout Presets**: JSON floorplan matrices mapping passable tiles, desks, and walls.

### 3. Base Model & Inference Lineage

- **Deterministic State Engine**: TypeScript state machine, A* pathfinding algorithm, and 2D physics kernel execute 100% deterministically.
- **Extension API Integration**: Built on official VS Code Extension API and React 18 Webview runtime.
- **Zero Training on User Code**: Ingested file paths, command names, and agent metadata are never used for model training.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against indirect prompt injection in agent status labels, malicious hook broadcasts, and unauthorized websocket command execution.
- **Local Sandbox Confinement**: Sprite state, floorplan files, and audio assets reside strictly within the local machine.
- **Automated Secret Scrubbing**: API tokens, private keys, and sensitive environment variables in command strings are redacted before visual display.
- **Zero Commercial Monetization**: Developer activity patterns, workspace names, and tool trajectories are never commercialized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of Pixel Agents is essential for effective deployment.

### 1. Canvas Webview Frame Budget Limits
- **Limitation**: Simulating dozens of animated characters simultaneously can stress VS Code webview memory limits on low-spec hardware.
- **Mitigation**: Pixel Agents caps maximum active sprite count at 32 and implements automatic off-screen sprite culling.

### 2. Headless CLI Tooling Without Extension Hooks
- **Limitation**: Standalone CLI scripts executed outside configured terminal wrappers cannot emit visual telemetry hooks.
- **Mitigation**: The extension provides automated shell wrapper scripts and `.env` exports to hook background CLI tools.

### 3. Audio Focus Bottlenecks on High-Concurrency Bursts
- **Limitation**: Highly concurrent parallel subagents finishing tasks simultaneously can create disruptive overlapping sound chimes.
- **Mitigation**: Audio queue manager enforces exponential backoff and limits simultaneous sound channels to 2.

### 4. Non-Standard Monorepo Sub-Process Tracking
- **Limitation**: Deeply nested sub-processes spawned across separate tmux sessions may evade process tree tracking.
- **Mitigation**: Users can bind agents explicitly using PID declarations or named socket endpoints.

### 5. Multi-Window VS Code Context Synchronization
- **Limitation**: Running multiple VS Code workspace windows concurrently creates independent webview instances without shared memory.
- **Mitigation**: The extension utilizes local WebSocket broadcast channels to synchronize office state across open editor windows.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested agent telemetry, hook events & streams | Section 1 | Verified |
| - Configuration, sprite sheets & floorplan maps | Section 2 | Verified |
| - Base model lineage & deterministic state engine | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Canvas webview frame budget limits | Section 1 | Verified |
| - Headless CLI tooling without extension hooks | Section 2 | Verified |
| - Audio focus bottlenecks on high-concurrency bursts | Section 3 | Verified |
| - Non-standard monorepo sub-process tracking | Section 4 | Verified |
| - Multi-window VS Code context synchronization | Section 5 | Verified |
