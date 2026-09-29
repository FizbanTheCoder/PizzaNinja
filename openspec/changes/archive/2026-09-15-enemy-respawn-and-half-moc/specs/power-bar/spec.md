## MODIFIED Requirements

### Requirement: MOC charged by slam kills

MOC SHALL be a resource on a 0–100 scale. A slam kill SHALL increase MOC by `hpMax*10` of the killed enemy (tier 1 robot: +10, tier 2: +20, tier 3: +30, ghost: +20), capped at 100. Shuriken kills MUST NOT charge MOC. For now MOC has no use beyond presentation.

#### Scenario: Tier-scaled charging

- **WHEN** the player kills an enemy with `hpMax` equal to N using the slam
- **THEN** MOC increases by N*10, not exceeding 100

#### Scenario: Shuriken does not charge MOC

- **WHEN** the player kills an enemy with a shuriken
- **THEN** MOC does not change and only points are awarded

## RENAMED Requirements

- FROM: `### Requirement: MOC reset`
- TO: `### Requirement: MOC decay on death or level change`

## MODIFIED Requirements

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