---
name: "cross-environment-hook-integration"
description: "Provides typed HookProvider bridges across VS Code extension terminals and standalone CLI browser servers."
license: MIT
---

# Cross-Environment Hook Integration

## Overview
This skill provides universal agent-to-UI connectivity through a typed `HookProvider` architecture, supporting both embedded VS Code webviews and external browser sessions via standalone CLI servers.

## Key Capabilities
- **Typed Hook Interface**: Exposes standardized event emitters (`onAgentStart`, `onToolCall`, `onAgentWait`, `onAgentExit`).
- **VS Code Extension Binding**: Interfaces directly with VS Code terminal instances and webview message passing channels.
- **Standalone CLI Server**: Operates an independent Node.js WebSocket server serving the office web application for terminal/tmux workflows.
- **Bi-Directional Messaging**: Allows the visual interface to send focus, inspect, and terminal-switching commands back to the editor.

## Operational Workflow
1. **Provider Handshake**: Initialize `HookProvider` instance matching current runtime environment (VS Code vs. CLI).
2. **Listener Registration**: Hook into agent execution streams (Claude Code hooks, shell wrappers, or IPC sockets).
3. **Telemetry Streaming**: Dispatch normalized JSON events to the webview client via `hook_telemetry_receiver`.
4. **Heartbeat Management**: Maintain low-latency synchronization and manage client reconnections.
