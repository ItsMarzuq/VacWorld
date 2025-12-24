# ASTRA Vacuum World Multi-Agent System

## Overview

This project implements a multi-agent vacuum cleaning system using the ASTRA agent programming language. The system consists of multiple vacuum cleaner robots (vacs) that work together to clean dust in a grid-based environment. The agents follow a leader-follower architecture where one agent acts as the leader coordinating tasks, while the remaining agents act as followers executing assigned cleaning tasks.

## Features

- **Multi-Agent Coordination**: Leader-follower architecture for efficient task distribution
- **Collision Avoidance**: Intelligent vac-to-vac collision prevention
- **Random Obstacle Avoidance**: Randomized direction selection when facing obstacles
- **Dust Detection**: Agents can detect dust in multiple directions (forward, left, right, diagonals)
- **Task Assignment**: Leader assigns dust locations to followers
- **Dynamic Exploration**: Agents explore the environment when no tasks are available

## File Structure
![alt text](images/FileStructure.jpg)

## Requirements

- ASTRA Runtime Environment
- Java Runtime Environment (JRE)
- Vacuum World JAR file (vacuumworld-1.3.0.jar)

## How to Run

1. Ensure all `.astra` files are in the same directory
2. Place `vacuumworld-1.3.0.jar` in the `dependency/` folder
3. Compile and run the ASTRA project with `Main.astra` as the entry point
4. Add the `java` folder with `graph` code and `GradientMap.java` and `Routing.java` files.
5. Run the `mvn` command in the terminal of the project folder.

## Agent Descriptions

### Main Agent
The entry point of the system. Responsible for:
- Launching the Vacuum World environment
- Initializing the grid (level 4)
- Creating leader and follower agents
- Assigning agent roles based on creation order

### Leader Agent
The coordinating agent that:
- Explores the environment and records dust locations
- Maintains a global list of dust locations
- Assigns cleaning tasks to followers
- Cleans dust it encounters directly
- Tracks task completion and reassigns failed tasks

### Follower Agent
Worker agents that:
- Request tasks from the leader
- Navigate to assigned dust locations
- Clean dust at target locations
- Report task completion to the leader
- Explore when no tasks are available

## Key Behaviors

### Collision Avoidance
All agents implement intelligent collision avoidance:
1. Check for other vacs before moving in any direction
2. If a vac is detected ahead, try alternative directions (left, right, back)
3. Always perform a movement action to prevent deadlocks

### Random Obstacle Avoidance
When facing obstacles, agents randomly select a direction:
- If only forward is blocked: randomly choose left, right, or back
- If forward and left are blocked: randomly choose right or back
- If forward and right are blocked: randomly choose left or back
- If all sides blocked: move back

### Dust Priority
Agents prioritize dust cleaning:
1. First check if current location has dust → clean it
2. Then check adjacent squares for dust → move toward it
3. Otherwise, explore or follow assigned tasks

## Communication Protocol

| Message Type | Sender | Receiver | Content | Description |
|--------------|--------|----------|---------|-------------|
| request | Follower | Leader | `requestTask()` | Request a new task |
| inform | Leader | Follower | `assignTask(X, Y)` | Assign dust location |
| inform | Leader | Follower | `noTask()` | No tasks available |
| inform | Follower | Leader | `taskComplete(X, Y)` | Task finished |
| inform | Follower | Leader | `taskUnreachable(X, Y)` | Cannot reach target |

## Configuration

In `Main.astra`, you can modify:
- `level("4")` - Grid size (1-5, where 5 is largest)
- `generation("no")` - Dust generation ("yes" for continuous dust)

## Modules Used

| Module | Purpose |
|--------|---------|
| EIS | Environment Interface Standard - connects to Vacuum World |
| Console | Debug output and logging |
| Routing | Obstacle mapping and pathfinding |
| System | Agent creation and management |
| Math | Random number generation |

## License

This project is for educational purposes as part of multi-agent systems coursework.
