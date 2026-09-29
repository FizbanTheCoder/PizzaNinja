# shuriken-controls Specification

## Purpose

Defines the player's shuriken fire binding: which keys throw, the cooldown between shots, and which keys are explicitly not bound, so the ground attack is reachable from the same hand cluster as movement.

## Requirements

### Requirement: Fire binding

The player SHALL fire a shuriken by pressing `Q` or `X`. `Q` and `X` SHALL be fully interchangeable — either key SHALL produce the same shot. Pressing `J` SHALL NOT fire a shuriken and SHALL have no gameplay effect while held. The fire binding SHALL be active in every game state in which firing is currently permitted, and SHALL be independent of which variant the shot resolves to (normal or super).

#### Scenario: Q fires a shuriken

- **WHEN** the player presses Q while the fire cooldown is ready
- **THEN** a shuriken is fired

#### Scenario: X still fires a shuriken

- **WHEN** the player presses X while the fire cooldown is ready
- **THEN** a shuriken is fired, identical to the one Q produces

#### Scenario: J is unbound

- **WHEN** the player presses J
- **THEN** no shuriken is fired and nothing else in the game changes

#### Scenario: Q and X are treated as one binding

- **WHEN** the player holds Q and X at the same time
- **THEN** at most one shuriken is fired per cooldown window, as if a single key had been pressed

### Requirement: Fire cooldown

After a shuriken is fired, a cooldown of 0.35 seconds SHALL elapse before the next shuriken can be fired by any bound key. A press made during the cooldown SHALL be ignored and SHALL NOT be buffered or replayed once the cooldown expires.

#### Scenario: Rate-limited to one shot per cooldown

- **WHEN** the player presses Q, then presses X again 0.2 seconds later
- **THEN** only the first shuriken is fired

#### Scenario: Press during cooldown is not replayed

- **WHEN** the player presses Q, then presses X again 0.2 seconds later, and the cooldown expires without further input
- **THEN** no second shuriken is fired

#### Scenario: Fires again once the cooldown expires

- **WHEN** the player presses Q and then presses X more than 0.35 seconds later
- **THEN** a second shuriken is fired

### Requirement: Menu hint names the bound keys

The menu SHALL state in Polish that `Q`/`X` fire the shuriken, and the MOC line SHALL state in Polish that `Q`/`X` fire the super shuriken. Neither line SHALL name `J` as a shuriken key.

#### Scenario: Menu lists the current binding

- **WHEN** the player reads the menu
- **THEN** the shuriken hint names Q and X, and the super-shuriken hint names Q and X
