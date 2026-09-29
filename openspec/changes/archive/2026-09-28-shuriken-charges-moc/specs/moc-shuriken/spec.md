# Spec Delta

## MODIFIED Requirements

### Requirement: One-hit kill

Colliding with an enemy SHALL destroy it instantly regardless of health tier (any robot tier or a ghost) and SHALL award standard points (`hpMax*50` for a robot, `+30` for a ghost). The super shuriken SHALL be consumed on that first collision and MUST NOT continue to another enemy. A super shuriken kill SHALL NOT charge MOC, even though a kill with a normal shuriken does: the shot already consumed the full bar when it was fired, so the whole transaction nets to zero.

#### Scenario: Kills any robot tier in one hit

- **WHEN** a super shuriken touches a robot of any tier
- **THEN** the robot is destroyed instantly and the player gains `hpMax*50` points

#### Scenario: Kills a ghost in one hit

- **WHEN** a super shuriken touches a ghost
- **THEN** the ghost is destroyed instantly and the player gains 30 points

#### Scenario: Super shuriken kill awards no MOC

- **WHEN** a super shuriken destroys an enemy
- **THEN** MOC does not increase, and remains 0 from the moment the shot was fired

#### Scenario: Consumed after the first kill

- **WHEN** a super shuriken kills an enemy and another enemy lies beyond it
- **THEN** the shuriken is consumed and does not hit the second enemy
