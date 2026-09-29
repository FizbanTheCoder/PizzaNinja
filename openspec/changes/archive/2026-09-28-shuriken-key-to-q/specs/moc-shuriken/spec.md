# Spec Delta

## Purpose

Gives MOC a real use: a full bar fires a homing, one-hit-kill shuriken with a blue flame trail that closes the slam → MOC → super-ability loop.

## MODIFIED Requirements

### Requirement: Fired from a full MOC bar

Pressing Q/X SHALL fire a super shuriken when MOC is full (100) and at least one enemy is visible in the camera view (a robot or a ghost within the current camera bounds); firing it SHALL set MOC to 0. When MOC is not full, or when MOC is full but no enemy is visible in the camera view, pressing Q/X SHALL fire a normal shuriken and MOC SHALL remain unchanged.

#### Scenario: Full bar fires a super shuriken

- **WHEN** the player presses Q/X with MOC equal to 100 and a robot or ghost visible in the camera view
- **THEN** a super shuriken is fired and MOC becomes 0

#### Scenario: Bar not full fires a normal shuriken

- **WHEN** the player presses Q/X with MOC below 100
- **THEN** a normal shuriken is fired and MOC does not change

#### Scenario: Full bar without a visible enemy fires a normal shuriken

- **WHEN** the player presses Q/X with MOC equal to 100 but no robot or ghost is visible in the camera view
- **THEN** a normal shuriken is fired and MOC stays at 100
