# Spec Delta

## REMOVED Requirements

### Requirement: MOC charged by slam kills

**Reason**: The requirement defined the slam as the only source of MOC and stated `Shuriken kills MUST NOT charge MOC` with a scenario asserting the same. Charging shurikens at half the slam rate is the point of this change, so the requirement is replaced rather than edited — its exclusive framing no longer describes the game.

**Migration**: Superseded by "MOC charged by kills" below, which keeps the slam rate (`hpMax*10`) unchanged and adds the shuriken rate (`hpMax*5`), the rule that only a killing hit charges, and the super shuriken's zero charge. No player-facing migration is needed; the MOC scale, cap, and decay are untouched.

## ADDED Requirements

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
