---
name: "agent-team-lifecycle-supervision"
description: "Monitors spawn, active execution, pause, permission requests, and teardown of parent agents and ephemeral sub-agents."
license: MIT
---

# Agent Team Lifecycle Supervision

## Overview
This skill tracks and supervises the lifecycle dynamics of multi-agent teams, managing the visual representation of primary coding agents, persistent teammates, and short-lived sub-agents.

## Key Capabilities
- **Sub-Agent Spawning**: Instantiates distinct child character sprites when parent agents invoke sub-agents.
- **Role Differentiation**: Visually marks agent roles (Architect, Coder, Reviewer, Tester) with distinct accessories or badges.
- **Teardown Animation**: Animates graceful character exit (walking out of the office) when sub-agents terminate.
- **Heartbeat Monitoring**: Detects crashed or disconnected agent sessions and clears delinquent visual state.

## Operational Workflow
1. **Spawn Detection**: Detect subagent initialization signal via hook stream.
2. **Character Allocation**: Spawn new character entity and assign temporary workspace chair.
3. **Parent Linkage**: Render subtle visual indicators linking sub-agent to initiating parent character.
4. **Clean Exit**: Upon sub-agent completion, deallocate desk coordinates and trigger departure animation.
