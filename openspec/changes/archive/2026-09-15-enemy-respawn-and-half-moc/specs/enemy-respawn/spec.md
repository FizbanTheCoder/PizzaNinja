## Purpose

Restores enemies to their starting state when the player respawns, so the world stays alive across death and the MOC bar can be refilled in the same level.

## ADDED Requirements

### Requirement: Enemies restored on pit-fall respawn

When the player falls below the level bounds and respawns, all hazards and robots SHALL be restored to their starting state — full health, starting positions, patrol ranges and speeds — exactly one set of each enemy from the level definition. Enemies killed with shurikens or slams SHALL also return on the next pit-fall respawn.

#### Scenario: Hazards restored after a pit fall

- **WHEN** the player kills some hazards, falls below the level bounds and respawns
- **THEN** all hazards from the level definition are present again at their starting positions with full health

#### Scenario: Robots restored after a pit fall

- **WHEN** the player kills some robots, falls below the level bounds and respawns
- **THEN** all robots from the level definition are present again at their starting positions with full health

#### Scenario: No duplicates on respawn

- **WHEN** the player respawns after falling into a pit
- **THEN** each hazard and robot from the level definition exists exactly once in the level