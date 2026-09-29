# Spec: slam-attack

## Purpose

Gives the player a close-quarters aerial attack (a downward slam) that kills any enemy in one hit and rewards risky play near enemies.

## ADDED Requirements

### Requirement: Slam armed by midair Space jump

The slam SHALL be armed only by performing a midair jump with the Space key. A midair jump with W/↑ MUST NOT arm the slam. Arming expires on touching the ground.

#### Scenario: Double Space jump arms the slam

- **WHEN** the player is airborne and performs a midair jump with Space
- **THEN** the slam is armed until landing

#### Scenario: Midair W/↑ jump does not arm the slam

- **WHEN** the player is airborne and performs a midair jump with W or ↑
- **THEN** the slam is not armed

#### Scenario: Landing disarms the slam

- **WHEN** the player with an armed slam lands on a platform
- **THEN** the slam is no longer armed

### Requirement: Slam activation

When the slam is armed, the player is airborne and `invuln <= 0`, pressing ↓ or S SHALL start the slam: the player dives straight down at high speed. The slam MUST NOT activate during invulnerability (`invuln > 0`) or when not armed.

#### Scenario: Activation while airborne

- **WHEN** the slam is armed, the player is airborne with `invuln <= 0`, and presses ↓ or S
- **THEN** the player dives straight down with increased falling speed

#### Scenario: No activation during invulnerability

- **WHEN** the slam is armed, the player is airborne with `invuln > 0`, and presses ↓ or S
- **THEN** the slam does not activate

#### Scenario: No activation when not armed

- **WHEN** the slam is not armed and the player presses ↓ or S while airborne
- **THEN** the slam does not activate

### Requirement: Slam kill

During a slam, contact with any enemy (a robot of any tier or a ghost) SHALL instantly kill the enemy and MUST NOT damage the player. The kill SHALL award standard points (`hpMax*50` for a robot, `+30` for a ghost). The player MUST NOT bounce after a kill — the dive continues and may hit further enemies. The slam ends on landing on a platform.

#### Scenario: Insta-kill a robot of any tier

- **WHEN** the slamming player touches a tier 1, 2 or 3 robot
- **THEN** the robot dies instantly, the player gains `hpMax*50` points, and takes no damage or time penalty

#### Scenario: Insta-kill a ghost

- **WHEN** the slamming player touches a ghost
- **THEN** the ghost dies instantly, the player gains 30 points, and takes no damage or time penalty

#### Scenario: No bounce and chain kills

- **WHEN** the slamming player kills an enemy and touches another enemy while still diving
- **THEN** the second enemy also dies and the player keeps diving without bouncing

#### Scenario: Landing ends the slam

- **WHEN** the slamming player lands on a platform
- **THEN** the slam ends and normal movement rules apply

### Requirement: Slam presentation

While slamming, the player sprite SHALL have a visible glow distinguishing the state from normal flight. The menu screen SHALL include a Polish instruction for performing the slam.

#### Scenario: Glow during slam

- **WHEN** the player is slamming
- **THEN** the player sprite is drawn with a glow

#### Scenario: Menu instruction

- **WHEN** the game is in the menu state
- **THEN** the screen shows a Polish description of how to perform the slam (double Space, then ↓/S while airborne)
