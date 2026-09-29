# power-bar Specification

## Purpose

Introduces the MOC resource (0–100) charged by kills and shown in the HUD — the foundation for a future super-ability.

## Requirements

### Requirement: MOC charged by kills

MOC SHALL be a resource on a 0–100 scale. A slam kill SHALL increase MOC by `hpMax*10` of the killed enemy (tier 1 robot: +10, tier 2: +20, tier 3: +30, ghost: +20), capped at 100. A kill delivered by a normal shuriken SHALL increase MOC by `hpMax*5` of the killed enemy (tier 1 robot: +5, tier 2: +10, tier 3: +15, ghost: +10), also capped at 100. Only the hit that destroys the enemy SHALL charge MOC: a shuriken that strikes a robot still alive SHALL award points and reduce that robot's health but SHALL NOT charge MOC, and a shuriken consumed by a platform SHALL award nothing at all. Firing a shuriken SHALL never itself cost MOC, regardless of whether the shot connects.

#### Scenario: Tier-scaled charging

- **WHEN** the player kills an enemy with `hpMax` equal to N using the slam
- **THEN** MOC increases by N*10, not exceeding 100

#### Scenario: Shuriken kill charges half the slam rate

- **WHEN** the player kills an enemy with `hpMax` equal to N using a normal shuriken
- **THEN** MOC increases by N*5, not exceeding 100

#### Scenario: Only the killing hit charges MOC

- **WHEN** a shuriken damages a robot that survives the hit
- **THEN** the robot loses health and the player gains no MOC, and the MOC increase arrives only on the shot that destroys it

#### Scenario: A shuriken absorbed by a platform charges nothing

- **WHEN** a shuriken hits a platform before reaching an enemy
- **THEN** MOC does not change

#### Scenario: Firing alone does not cost MOC

- **WHEN** the player fires a shuriken that hits nothing
- **THEN** MOC does not change

### Requirement: MOC decay on death or level change

MOC SHALL NOT reset to 0 when the player falls into a pit, retries a level after a loss, restarts the current level, or advances to a new level — instead it SHALL be halved (rounded down, so 1 decays to 0). A fresh run (a new game from the menu or after finishing all levels) SHALL start MOC at 0.

#### Scenario: Falling into a pit halves MOC

- **WHEN** the player falls below the level bounds and respawns with MOC equal to N
- **THEN** MOC is floor(N/2)

#### Scenario: Retrying after a loss halves MOC

- **WHEN** a lost level is retried with MOC equal to N
- **THEN** MOC is floor(N/2)

#### Scenario: Restarting the current level halves MOC

- **WHEN** the player restarts the current level with MOC equal to N
- **THEN** MOC is floor(N/2)

#### Scenario: Advancing to a new level halves MOC

- **WHEN** the player beats a level with MOC equal to N and the next level begins
- **THEN** MOC is floor(N/2)

#### Scenario: A fresh run starts with zero MOC

- **WHEN** a new game begins (from the menu or after finishing all levels)
- **THEN** MOC is 0

### Requirement: MOC HUD presentation

The HUD SHALL show a "MOC" label and a bar reflecting the current value relative to 100. The bar SHALL carry a pulsing highlight in silver from 50 MOC upward, and a pulsing highlight in gold at 100 MOC; the gold highlight SHALL be visually distinct from the silver one. Below 50 MOC the bar SHALL carry no pulsing highlight.

#### Scenario: Bar reflects value

- **WHEN** MOC equals N on the 0–100 scale
- **THEN** the bar fill corresponds to N/100 of its width

#### Scenario: Half-full bar highlight

- **WHEN** MOC is between 50 and 99
- **THEN** the bar is highlighted with a pulsing silver highlight

#### Scenario: Full bar highlight

- **WHEN** MOC reaches 100
- **THEN** the bar is highlighted with a pulsing gold highlight, visually distinct from the silver highlight used between 50 and 99

#### Scenario: No highlight below the threshold

- **WHEN** MOC is below 50
- **THEN** the bar carries no pulsing highlight