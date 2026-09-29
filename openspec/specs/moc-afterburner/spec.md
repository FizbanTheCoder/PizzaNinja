# moc-afterburner Specification

## Purpose

Adds a MOC-powered afterburner: an airborne third lift option (jump → double-jump → rocket) that spends the MOC bar for climb, hover, and safe repositioning, creating a second spend alongside the super shuriken.

## Requirements

### Requirement: Ignition

Pressing the jump key (↑/W) while airborne with no air-jumps remaining SHALL ignite the afterburner. Ignition SHALL NOT consume an air-jump, SHALL NOT trigger a jump, and SHALL only be possible while the afterburner is off and the player is not already slamming. Owning any MOC (including less than 10) SHALL permit ignition.

#### Scenario: Ignites after the air-jump is spent

- **WHEN** the player is airborne, has used their air-jump, and presses the jump key
- **THEN** the afterburner ignites and the player does not gain extra jump height

#### Scenario: Does not ignite when an air-jump remains

- **WHEN** the player presses the jump key midair with an air-jump still available
- **THEN** the air-jump is performed and the afterburner does not ignite

#### Scenario: Does not ignite during a slam

- **WHEN** the player is slamming and presses the jump key
- **THEN** the afterburner does not ignite

### Requirement: Flight and hover behavior

While the afterburner is active, holding the jump key SHALL propel the player upward at a constant rise speed, and releasing it SHALL switch to a hover that holds vertical speed at zero. Horizontal movement keys (A/D or ←/→) SHALL keep working with the normal horizontal speed while flying or hovering.

#### Scenario: Holding the jump key rises

- **WHEN** the afterburner is active and the player holds the jump key
- **THEN** the player moves upward at a constant speed

#### Scenario: Releasing the jump key hovers

- **WHEN** the afterburner is active and the player releases the jump key
- **THEN** the player's vertical speed becomes and stays zero until the key is held again

#### Scenario: Horizontal movement works in flight

- **WHEN** the afterburner is active and the player holds left or right
- **THEN** the player moves horizontally at the normal running speed

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

### Requirement: Disengagement

Pressing the down key (↓/S) while the afterburner is active SHALL start a slam dive and disengage the afterburner. Landing on a platform SHALL automatically disengage the afterburner. Slamming SHALL keep its existing behavior (downward dive, instant kill on contact, charging MOC on kill).

#### Scenario: Down starts a slam and disengages

- **WHEN** the afterburner is active and the player presses down
- **THEN** the afterburner disengages and a slam dive begins

#### Scenario: Landing turns it off

- **WHEN** the afterburner is active and the player lands on a platform
- **THEN** the afterburner disengages

### Requirement: Contact immunity while flying

While the afterburner is active, overlapping a robot or a ghost SHALL NOT damage the player and SHALL NOT knock the player back. A slam dive performed from the afterburner SHALL still instantly kill an enemy on contact as normal.

#### Scenario: Passing over an enemy is safe

- **WHEN** the afterburner is active and the player overlaps a robot or ghost without slamming
- **THEN** the player takes no damage and is not knocked back

#### Scenario: Slam from flight still kills

- **WHEN** the player slams while the afterburner is active and touches an enemy
- **THEN** the enemy is instantly destroyed

### Requirement: Height limit and presentation

The afterburner SHALL NOT carry the player above the top of the play field (y clamped so the player stays on screen). While active it SHALL render an orange flame beneath the player, visually distinct from existing cyan/blue effects, and the menu SHALL mention the afterburner with its cost in Polish.

#### Scenario: Cannot fly above the screen

- **WHEN** the afterburner lifts the player to the top of the play field
- **THEN** the player is clamped at the top edge and continues hovering there

#### Scenario: Menu hint shown

- **WHEN** the player views the menu
- **THEN** it displays a Polish line about the afterburner and its MOC cost