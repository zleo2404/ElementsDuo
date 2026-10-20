# Elements Duo

Elements Duo is a 2D cooperative puzzle-platformer developed in Java.
The game builds on the classic two-character formula, Fireboy and Watergirl, both controlled from the same keyboard: each player
must cross the level, avoid obstacles and enemies, interact with the environment and reach their own exit.

The project was developed with a strong focus on separation of concerns, object-oriented modelling and the application of design
patterns, in order to keep the codebase modular, extensible and testable.

## Requirements

- Java 17 or higher
- Gradle Wrapper (included in the repository)

## Getting Started

To run the game from source:

```
./gradlew run
```

To run the prebuilt JAR included in the repository:

```
java -jar OOP24-elements-duo-all.jar
```

To build a new executable JAR:

```
./gradlew shadowJar
```

The JAR is generated in:

```
build/libs/OOP24-elements-duo-all.jar
```

## Controls

Fireboy:

- `A`: move left
- `D`: move right
- `W`: jump

Watergirl:

- `Left arrow`: move left
- `Right arrow`: move right
- `Up arrow`: jump

## Build and Test

Compile the project:

```
./gradlew assemble
```

Run the test suite:

```
./gradlew test
```

Run all verification tasks:

```
./gradlew check
```

The project uses JUnit 5 for testing and includes quality checks integrated into the Gradle build.

## Project Structure

```
.
├── .github/workflows              Continuous integration pipeline
├── gradle/wrapper                 Gradle Wrapper
├── save/                          Persisted player progress (JSON)
├── src/
│   ├── main/
│   │   ├── java/it/unibo/elementsduo/
│   │   └── resources/
│   │       ├── levels             Playable level maps (plain text)
│   │       └── test               Maps used by the test suite
│   └── test/java/it/unibo/elementsduo/
├── build.gradle.kts               Build configuration
├── OOP24-elements-duo-all.jar     Prebuilt executable JAR
└── report.pdf                     Project report
```

### Tests

```
src/test/java/it/unibo/elementsduo/model
├── player                         Fireboy and Watergirl
├── enemies                        Enemies and projectiles
├── obstacles                      Obstacle factories, platforms, pushable boxes
├── interactions                   Interaction handlers, commands, collision detection
├── map                            Map loading, validation and entity assembly
├── mission                        Missions and objectives
└── progressionstate               Progression state
```

## Architecture

The application follows an MVC-inspired architecture.

The `model` package contains the core game logic: players, enemies, obstacles, collisions, missions, game state and progression.
The `view` package contains the Swing panels responsible for rendering.
The `controller` package mediates between model and view, managing the game loop, keyboard input, screen navigation and game
progression.

This separation keeps the game behaviour independent from its graphical representation and makes the core components easier to
test.

## Design Patterns and Design Choices

Several design patterns and principles were applied to reduce coupling between classes and improve extensibility.

- **MVC**: overall application structure.
- **Factory / Factory Method**: centralised creation of players, enemies, obstacles and game entities.
- **Builder**: construction of interaction responses, collecting multiple actions before their execution.
- **Command**: modelling of actions produced by interactions, such as pushing crates or resolving physical and logical effects.
- **Observer / Event Listener**: notification of game events such as gem collection, enemy deaths, exit reached and activation of
interactive elements.
- **Composite**: mission management, where multiple objectives are aggregated into a main mission.
- **Template Method**: shared algorithms with specialised steps in subclasses, for example in objective and enemy handling.
- **Strategy**: separation of specific behaviours such as input, physics and character collision correction.
- **Handler chain**: collision management, where each handler is responsible for a specific family of interactions.

## Gameplay

Each level features obstacles, enemies, platforms, levers, buttons, gems and hazardous areas.
Fireboy and Watergirl behave similarly but not identically: some surfaces are safe for one character and deadly for the other,
which makes cooperation essential.

A level is completed when both characters reach their respective exits.
A secondary mission is also available, requiring the player to collect all gems, defeat all enemies and finish within a time limit.

## Progression and Persistence

Player progress is stored as JSON in:

```
save/savegame.json
```

For each level, the following data is kept:

- best completion time;
- secondary mission status;
- current level.

Serialisation is handled with Jackson.

## Levels

Levels are loaded from plain text files in the project resources.
A dedicated assembler converts map symbols into game entities.

Before a level starts, it is validated to ensure structural correctness, the presence of all required elements and the
reachability of the main objectives.

## Documentation

The full project report is available in [`report.pdf`](report.pdf) (written in Italian). It covers:

- **Analysis**: requirements and domain model;
- **Design**: overall architecture and detailed design of each component, with the patterns applied;
- **Development**: automated testing and development notes;
- **Final remarks**: self-assessment and future work;
- **User guide**: description of the software and the main menu.
