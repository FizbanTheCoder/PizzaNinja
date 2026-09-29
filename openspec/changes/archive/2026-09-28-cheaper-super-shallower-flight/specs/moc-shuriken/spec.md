# Spec Delta

## REMOVED Requirements

### Requirement: Fired from a full MOC bar

**Reason**: The requirement made the full bar the sole trigger and zeroed MOC on firing. The threshold is now 50 and firing subtracts 50, so both the name and the rule are false. The requirement is replaced rather than edited because its title asserts a condition that no longer exists.

**Migration**: Superseded by "Fired at half MOC" below, which fires at 50, subtracts 50, and allows two shots from a full bar. Its homing, one-hit-kill, and passes-through-platforms requirements are unchanged and still govern the shot itself.

## ADDED Requirements

### Requirement: Fired at half MOC

Pressing Q/X SHALL fire a super shuriken when MOC is at least 50 and at least one enemy is visible in the camera view (a robot or a ghost within the current camera bounds); firing it SHALL subtract 50 from MOC. When MOC is below 50, or when MOC is at least 50 but no enemy is visible in the camera view, pressing Q/X SHALL fire a normal shuriken and MOC SHALL remain unchanged. The threshold SHALL be the only gate: a player holding the full 100 MAY fire two super shurikens in succession, and the second SHALL NOT require MOC to drop to 50 or below first.

#### Scenario: Half MOC fires a super shuriken

- **WHEN** the player presses Q/X with MOC between 50 and 100 inclusive and a robot or ghost visible in the camera view
- **THEN** a super shuriken is fired and MOC decreases by 50

#### Scenario: Below half MOC fires a normal shuriken

- **WHEN** the player presses Q/X with MOC below 50
- **THEN** a normal shuriken is fired and MOC does not change

#### Scenario: A full bar holds two charges

- **WHEN** the player presses Q/X twice with MOC at 100 and a robot or ghost visible in the camera view for both presses
- **THEN** a super shuriken is fired each time and MOC becomes 0

#### Scenario: Half MOC without a visible enemy fires a normal shuriken

- **WHEN** the player presses Q/X with MOC of at least 50 but no robot or ghost is visible in the camera view
- **THEN** a normal shuriken is fired and MOC does not change
