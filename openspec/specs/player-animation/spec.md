# player-animation Specification

## Purpose

Gives the player sprite scripted visual animations. The first member is a backflip on the big double jump that sells momentum without changing any gameplay behavior.

## Requirements

### Requirement: Big double jump plays a backflip

When the player performs a double jump with the big-jump key (Space) while airborne with an air-jump available, the sprite SHALL rotate through a full 360° backwards flip over approximately 0.38 seconds, easing with a smoothstep curve. The flip SHALL then clear itself with no residual rotation.

#### Scenario: Big double jump flips
- **WHEN** the player is airborne with an air-jump available and presses Space
- **THEN** the sprite performs a full 360° backward rotation that finishes without residual angle

#### Scenario: Flip is backward in both facing directions
- **WHEN** the player performs the big double jump facing right
- **THEN** the sprite rotates backward relative to the direction of travel
- **WHEN** the player performs the big double jump facing left
- **THEN** the sprite also rotates backward relative to the direction of travel

### Requirement: Small double jump does not flip

A double jump performed with the small-jump key (W/↑) MUST NOT rotate the sprite.

#### Scenario: Small double jump stays upright
- **WHEN** the player is airborne with an air-jump available and presses W or ↑
- **THEN** the sprite plays no flip rotation

### Requirement: Flip is purely visual

The backflip SHALL have zero effect on gameplay state: the collision box, position, velocity, jump height and any slam arming MUST remain exactly as without the animation.

#### Scenario: Collision box unchanged during flip
- **WHEN** the player performs a big double jump while flipping
- **THEN** the collision box is identical to a non-flipping double jump

### Requirement: Flip is cleared by slam and landing

Starting a slam or landing on a platform SHALL immediately clear the flip so no partial rotation persists.

#### Scenario: Slam cancels the flip
- **WHEN** the player is flipping and starts a slam
- **THEN** the flip is cancelled and the sprite no longer rotates

#### Scenario: Landing cancels the flip
- **WHEN** the player is flipping and lands on a platform
- **THEN** the flip is cancelled and the sprite no longer rotates