## MODIFIED Requirements

### Requirement: MOC charged by slam kills

MOC SHALL be a resource on a 0–100 scale. A slam kill SHALL increase MOC by `hpMax*10` of the killed enemy (tier 1 robot: +10, tier 2: +20, tier 3: +30, ghost: +20), capped at 100. Shuriken kills MUST NOT charge MOC.

#### Scenario: Tier-scaled charging

- **WHEN** the player kills an enemy with `hpMax` equal to N using the slam
- **THEN** MOC increases by N*10, not exceeding 100

#### Scenario: Shuriken does not charge MOC

- **WHEN** the player kills an enemy with a shuriken
- **THEN** MOC does not change and only points are awarded