# Spec: power-bar

## Purpose

Introduces the MOC resource (0–100) charged by slam kills and shown in the HUD — the foundation for a future super-ability.

## ADDED Requirements

### Requirement: MOC charged by slam kills

MOC SHALL be a resource on a 0–100 scale, starting each level attempt at 0. A slam kill SHALL increase MOC by `hpMax*10` of the killed enemy (tier 1 robot: +10, tier 2: +20, tier 3: +30, ghost: +20), capped at 100. Shuriken kills MUST NOT charge MOC. For now MOC has no use beyond presentation.

#### Scenario: Tier-scaled charging

- **WHEN** the player kills an enemy with `hpMax` equal to N using the slam
- **THEN** MOC increases by N*10, not exceeding 100

#### Scenario: Shuriken does not charge MOC

- **WHEN** the player kills an enemy with a shuriken
- **THEN** MOC does not change and only points are awarded

### Requirement: MOC reset

MOC SHALL reset to 0 on every player death (falling into a pit and retrying after a loss) and when a new level starts.

#### Scenario: Falling into a pit resets MOC

- **WHEN** the player falls below the level bounds and respawns
- **THEN** MOC is 0

#### Scenario: New level resets MOC

- **WHEN** a new level or a retry after a loss begins
- **THEN** MOC is 0

### Requirement: MOC HUD presentation

The HUD SHALL show a "MOC" label and a bar reflecting the current value relative to 100. A full bar SHALL be visually highlighted (pulsing).

#### Scenario: Bar reflects value

- **WHEN** MOC equals N on the 0–100 scale
- **THEN** the bar fill corresponds to N/100 of its width

#### Scenario: Full bar highlight

- **WHEN** MOC reaches 100
- **THEN** the bar is visually highlighted (pulsing)
