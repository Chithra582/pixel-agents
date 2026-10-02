---
name: "virtual-office-orchestration"
description: "Manages the virtual pixel-art workspace grid, furniture placement, desk allocation, and agent pathfinding navigation."
license: MIT
---

# Virtual Office Orchestration

## Overview
This skill controls the spatial layout, tile grid, furniture physics, and pathfinding navigation of the simulated pixel-art virtual office.

## Key Capabilities
- **Tile Grid Management**: Maintains 2D matrix representations of floors, walls, windows, and decorative assets.
- **A* Pathfinding**: Calculates smooth, collision-free walking paths around desks, chairs, and partitions.
- **Area-to-Folder Mapping**: Divides the office into named zones (e.g., Frontend, Backend, DevOps) mapped to workspace folders.
- **Interactive Editing**: Supports in-situ painting of tiles, placement of desks, and rearranging of office furniture.

## Operational Workflow
1. **Layout Initialization**: Load saved office layout JSON matrix via `office_grid_manager`.
2. **Obstacle Collision Baking**: Generate traversability graph marking solid walls and impassable furniture.
3. **Desk Assignment**: Route incoming agents to available desks using `desk_allocator`.
4. **Wayfinding Execution**: Interpolate character walking coordinates along calculated path nodes to destination.
