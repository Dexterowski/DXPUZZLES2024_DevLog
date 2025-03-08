---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Damage Types
authors:
  - Dexterowski
---

# Damage Types

The **Damage Types** system defines various forms of damage that entities within DX PUZZLES can receive. Each damage type can trigger different effects and responses in gameplay (such as different labels in killfeed).

## Available Damage Types

### Generic Damage
Represents basic, unspecified damage. Often used for generic health reductions without special behaviors.

### Bullet Damage
Caused by bullets or projectile-based weaponry. Typically triggers visual effects like sparks or bullet impact decals.

### Crush Damage
Occurs when entities are crushed between two objects or by environmental hazards. This can trigger special animations or instant death depending on the severity.

### Bullet Damage
Specifically tied to projectile weapons. Typically applies force upon impact, affecting physics objects or characters.

### Fall Damage
Damage type applied when falling from significant heights, scaling with fall distance.

### Physics Damage
Applied by physical objects or forces in the world (e.g., thrown items or heavy collisions). Often associated with ragdoll effects and physical interactions.

## Damage Information Structure

The following details are available when damage is applied:

- **DamageType**  
  Specifies the type of damage (e.g., Generic, Bullet, Physics).

- **Damaged Entity**
  Reference to the actor receiving damage.

- **Instigator**
  Controller responsible for causing damage, useful for tracking kills or scoring.

- **Damage Causer**
  Actor that caused the damage, such as a weapon or environmental hazard.

- **Damage Force**
  Vector force applied by damage, influencing ragdoll or physics reactions.

### Special Properties

- **Damage Force**
  A vector defining the direction and strength of the force applied to the entity when damage occurs.

## Usage Notes

- Damage types can be associated with various game logic such as unique effects, sounds, or reactions (e.g., fire causing burning effects).
- Custom damage types can be created based on existing classes to extend or customize interactions and effects.

{{% alert info %}}
**Info:** The properties listed are primarily intended for use within the editor to visually differentiate and manage damage behaviors in gameplay.
{{% /alert %}}