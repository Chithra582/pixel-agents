# DUTIES — Pixel Agents

## Core Agent Duties

### 1. Agent Hook Telemetry Ingestion & Normalization
- Listen for hook signals emitted by AI coding agents (Claude Code, CLI agents, sub-agents) via WebSocket or extension messaging.
- Normalize heterogeneous agent event payloads into unified state representations (`tool_start`, `tool_end`, `waiting_input`, `subagent_spawn`, `terminated`).
- Parse tool signatures to classify agent activities into visual categories (Editing, Searching, Executing, Thinking).

### 2. Virtual Office & Spatial Navigation Management
- Maintain the office tile grid (floors, walls, furniture, designated room areas).
- Compute dynamic A* pathfinding routes for characters moving between entrance portals, desks, and shared spaces.
- Manage desk and chair allocations, mapping workspace project folders to designated office sectors.

### 3. Character Sprite Animation & Visual State Rendering
- Render animated character sprites with responsive directions (up, down, left, right) and situational animations (sitting, typing, reading).
- Display dynamic speech bubbles showing active thought summaries, prompt requests, and execution statuses.
- Distinguish between primary parent agents, persistent teammates, and ephemeral sub-agents visually.

### 4. Interactive Office Customization & Asset Management
- Provide an interactive drag-and-drop office layout editor for creating custom floor plans, walls, and furniture.
- Support importing and exporting layout JSON schemas and loading custom character/furniture sprite packs.
- Persist custom office designs across editor sessions and workspace windows.

### 5. Multi-Environment Synchronization
- Synchronize state seamlessly across VS Code extension webviews and standalone browser-based CLI servers.
- Maintain low-latency WebSocket connection loops with automatic reconnection and heartbeat supervision.
- Deliver audio chimes and toast alerts when agent workflows reach decision crossroads.
