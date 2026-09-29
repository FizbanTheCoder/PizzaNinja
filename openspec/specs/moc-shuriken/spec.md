# moc-shuriken Specification

## Purpose

Gives MOC a real use: a full bar fires a homing, one-hit-kill shuriken with a blue flame trail that closes the slam → MOC → super-ability loop.

## Requirements

### Requirement: Fired at half MOC

Pressing Q/X SHALL fire a super shuriken when MOC is at least 50 and at least one enemy is visible in the camera view (a robot or a ghost within the current camera bounds); firing it SHALL subtract 50 from MOC. When MOC is below 50, or when MOC is at least 50 but no enemy is visible in the camera view, pressing Q/X SHALL fire a normal shuriken and MOC SHALL remain unchanged.

#### Scenario: Half MOC fires a super shuriken

- **WHEN** the player presses Q/X with MOC equal to 50 and a robot or ghost visible in the camera view
- **THEN** a super shuriken is fired and MOC becomes 0

#### Scenario: Bar below half fires a normal shuriken

- **WHEN** the player presses Q/X with MOC below 50
- **THEN** a normal shuriken is fired and MOC does not change

#### Scenario: Half MOC without a visible enemy fires a normal shuriken

- **WHEN** the player presses Q/X with MOC at or above 50 but no robot or ghost is visible in the camera view
- **THEN** a normal shuriken is fired and MOC is unchanged

#### Scenario: A full bar holds two charges

- **WHEN** the player presses Q/X twice with MOC equal to 100 and a robot or ghost visible in the camera view for both presses
- **THEN** two super shurikens are fired and MOC becomes 0

### Requirement: Homing guidance

The super shuriken SHALL initially travel toward the enemy nearest to the player (robots and ghosts counted together) and SHALL steer toward that enemy's current position on every frame while it lives. If the locked target is destroyed, the shuriken SHALL steer toward the nearest remaining visible enemy; if no enemy remains, it SHALL continue in a straight line until removed.

#### Scenario: Homes in on the nearest enemy

- **WHEN** a super shuriken is fired with several enemies present
- **THEN** it curves toward the enemy closest to the player

#### Scenario: Re-targets after its target is destroyed

- **WHEN** the enemy the super shuriken is homing toward is destroyed mid-flight
- **THEN** the shuriken steers toward the nearest remaining visible enemy

#### Scenario: Flies straight with no targets left

- **WHEN** a super shuriken's target is destroyed and no other enemy is visible
- **THEN** it continues in a straight line

### Requirement: One-hit kill

Colliding with an enemy SHALL destroy it instantly regardless of health tier (any robot tier or a ghost) and SHALL award standard points (`hpMax*50` for a robot, `+30` for a ghost). The super shuriken SHALL be consumed on that first collision and MUST NOT continue to another enemy. A super shuriken kill SHALL NOT charge MOC: the shot already spent 50 MOC when it was fired, so the kill is a net loss on the bar.

#### Scenario: Kills any robot tier in one hit

- **WHEN** a super shuriken touches a robot of any tier
- **THEN** the robot is destroyed instantly and the player gains `hpMax*50` points

#### Scenario: Kills a ghost in one hit

- **WHEN** a super shuriken touches a ghost
- **THEN** the ghost is destroyed instantly and the player gains 30 points

#### Scenario: Super shuriken kill awards no MOC

- **WHEN** a super shuriken destroys an enemy
- **THEN** MOC does not increase, and remains at the value it held after the 50-point spend when the shot was fired

#### Scenario: Consumed after the first kill

- **WHEN** a super shuriken kills an enemy and another enemy lies beyond it
- **THEN** the shuriken is consumed and does not hit the second enemy

### Requirement: Passes through platforms

The super shuriken SHALL NOT be stopped by platforms; it SHALL be removed only by colliding with an enemy or leaving the camera bounds.

#### Scenario: Passes through a platform

- **WHEN** a super shuriken crosses a platform on its way to a target
- **THEN** it passes through and keeps flying

#### Scenario: Removed only by hit or leaving the view

- **WHEN** a super shuriken has no enemy to hit
- **THEN** it is removed only when it leaves the camera bounds

### Requirement: Blue flame presentation

A super shuriken SHALL be visibly distinct from a normal shuriken: it SHALL be rendered with a blue flame trail.

#### Scenario: Blue trail in flight

- **WHEN** a super shuriken is flying
- **THEN** it is drawn with a blue flame trail distinct from the normal cyan shuriken