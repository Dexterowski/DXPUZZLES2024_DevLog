---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Base Brush Breakable
authors:
  - Dexterowski
---

# BaseBrushBreakable

The **BaseBrushBreakable** is an abstract entity designed for creating static, breakable objects within DX PUZZLES 2024. It allows designers to define specific behaviors related to breaking conditions, health management, and customizable visual and audio effects.

## Entity Properties

### General Settings

- **Starting Health**:
  - Initial health of the breakable object.

- **Max Health**:
  - The maximum allowable health.

### Break Conditions

- **Break On Touch** (`bool`):
  - When enabled, object breaks upon contact with another actor.

- **Don't Take Physics Damage** (`bool`):
  - Prevents the object from taking damage from physics interactions.

- **Break on Physics Contact** (`bool`):
  - Allows the object to break when colliding with other physics-simulated objects.

- **Override Break Impulse** (`bool`):
  - Enables the use of a custom impulse threshold for breaking the object.

- **Break Impulse Threshold** (`float`):
  - Custom impulse threshold required to break the object (effective if Override is enabled).

### Damage Filters

- **Damage Filters**:
  - Specifies which damage types are allowed to affect this object.

### Gib Settings

- **Gibs Size** (`float`):
  - Determines the size of debris spawned when the object breaks.

- **Use Damage Velocity for Gibs** (`bool`):
  - Determines whether debris velocities are influenced by the damage force.

## Events

### Outputs

These events can trigger other actions within the level or Blueprint scripts:

- **OnDamaged**:
  - Triggered when the object receives damage.

- **OnBreak**:
  - Fired when the object breaks.

- **OnHealthChanged**:
  - Triggered whenever the object's health value changes.

- **OnHealed**:
  - Fires when the object receives healing.

### Inputs

These actions can be triggered via scripting or interactions in the Unreal editor:

- **Break**:
  - Instantly breaks the object, ignoring its current health.

- **AddHealth** (`float HealAmount`):
  - Increases the object's health.

- **RemoveHealth** (`float RemoveAmount`):
  - Decreases the object's health by the specified amount.

- **SetNewHealth** (`float NewHealth`):
  - Sets the object's health to a specified value.

## Gameplay Features

### Damage and Interaction

- Objects can be configured to receive or ignore damage from physics objects or interactions.
- Customizable damage filters allow level designers to precisely control damage interactions.

### Visual and Audio Effects

- Upon breaking, customizable particle and sound effects can be triggered, providing visual and auditory feedback for breaking actions.

## Usage Guidelines

- Use breakable brushes to create destructible environments or interactive elements within your levels.
- Configure the object's properties clearly to fit the gameplay scenario, controlling how easily or under what conditions the object will break.

This documentation helps ensure consistent gameplay behavior and facilitates creative and precise level design.

