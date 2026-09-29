# Spec Delta

## MODIFIED Requirements

### Requirement: MOC HUD presentation

The HUD SHALL show a "MOC" label and a bar reflecting the current value relative to 100. The bar SHALL carry a pulsing highlight whose color distinguishes the super shuriken threshold: a **silver** pulsing highlight from 50 up to 99 (a super shuriken can be fired), and a **gold** pulsing highlight at 100 (two super shuriken charges are held). Below 50 the bar SHALL carry no pulse. The gold highlight SHALL be visually distinct from the silver one so the two states are never ambiguous.

#### Scenario: Bar reflects value

- **WHEN** MOC equals N on the 0–100 scale
- **THEN** the bar fill corresponds to N/100 of its width

#### Scenario: Half-full bar highlight

- **WHEN** MOC is between 50 and 99
- **THEN** the bar is highlighted with a pulsing silver highlight

#### Scenario: Full bar highlight

- **WHEN** MOC reaches 100
- **THEN** the bar is highlighted with a pulsing gold highlight, visually distinct from the silver highlight used between 50 and 99

#### Scenario: No highlight below the threshold

- **WHEN** MOC is below 50
- **THEN** the bar carries no pulsing highlight
