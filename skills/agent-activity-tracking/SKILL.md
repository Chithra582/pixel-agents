---
name: "agent-activity-tracking"
description: "Intercepts agent tool calls (file editing, terminal execution, web searching) and maps them to animated character states."
license: MIT
---

# Agent Activity Tracking

## Overview
This skill intercepts and interprets real-time tool execution events from AI coding agents, classifying them into intuitive behavioral states and animating character sprites accordingly.

## Key Capabilities
- **Tool Telemetry Classification**: Maps specific tool invocations (e.g., `Edit`, `Replace`, `Write` to Typing; `Grep`, `View`, `Glob` to Reading; `Bash`, `Command` to Terminal).
- **Sprite Animation Transitions**: Drives frame-by-frame sprite animation loops (sitting, typing at computer, reading books, idling).
- **Speech Bubble Generation**: Formats concise, non-intrusive status bubbles reflecting active thoughts or pending questions.
- **Urgency Escalation**: Highlights characters with pulsating alerts when agents require immediate user approval.

## Operational Workflow
1. **Event Interception**: Receive raw tool event from active agent hook receiver.
2. **State Categorization**: Classify tool payload into visual activity state using `activity_state_dispatcher`.
3. **Sprite Update**: Trigger corresponding sprite sheet animation sequence on the character canvas.
4. **Speech Bubble Rendering**: If action requires user feedback, attach dynamic speech bubble above character head.
