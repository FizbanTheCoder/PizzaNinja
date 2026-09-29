# Spec Delta

## REMOVED Requirements

### Requirement: MOC consumption

**Reason**: The requirement states a 10 points-per-second drain and a 10-second full-bar budget, both of which become false. Its scenario titles name the old rate outright ("A full bar lasts ten seconds"), so the block is replaced rather than edited to avoid a scenario whose title contradicts its own text.

**Migration**: Superseded by "MOC consumption at 12.5 per second" below, which keeps every other rule: the drain applies identically while climbing and hovering, reaching 0 disengages the afterburner, and any MOC above 0 permits ignition.

## ADDED Requirements

### Requirement: MOC consumption at 12.5 per second

The afterburner SHALL drain MOC at a rate of 12.5 points per second while active, regardless of climbing or hovering (a full 100 bar SHALL sustain 8 seconds of flight). Reaching exactly 0 MOC while airborne SHALL disengage the afterburner and return the player to normal gravity.

#### Scenario: Drained while active

- **WHEN** the afterburner is active for 1 second
- **THEN** MOC decreases by 12.5

#### Scenario: Hover also drains

- **WHEN** the afterburner is active and the player is hovering for 1 second
- **THEN** MOC decreases by 12.5

#### Scenario: Runs out of MOC mid-flight

- **WHEN** MOC reaches 0 while the afterburner is active
- **THEN** the afterburner disengages and the player falls under normal gravity

#### Scenario: A full bar lasts eight seconds

- **WHEN** the afterburner is activated with 100 MOC
- **THEN** it sustains flight for 8 seconds before MOC reaches 0
