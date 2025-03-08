---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Physics Entity
authors:
  - Dexterowski
categories: Entities
tags:
  - DX PUZZLES
  - Entities
  - Physics
---

# BasePhysicsEntity

The **BasePhysicsEntity** is a foundational entity class for physics-based objects within DX PUZZLES. It provides comprehensive support for interactions, physics, water interactions, event broadcasting, and pickup mechanics.

## Overview

The **BasePhysicsEntity** is an interactive physics entity designed to respond to player actions and environmental interactions like collisions and water contact. Level designers can leverage the entity's built-in interaction and physics behaviors through exposed settings.

## Primary Interaction Events

### Use Events

- **OnUsed**  
  Triggered when a player interacts with the entity, provided that the **Generate Use Output** flag is enabled.

- **OnPickedUp**  
  Triggered when a player picks up this entity.

- **OnMotionEnabled**  
  Triggered when the entity's physics motion is enabled.

- **OnMotionDisabled**  
  Triggered when the entity's physics motion is disabled.

- **OnWaterEnter**  
  Triggered when the entity first touches water, providing information about the water body, location, and velocity at the point of contact.

## Editable Properties

### Flags

- **Motion Disabled (physics)**  
  Disables physics simulation at game start if enabled.

- **Player can't pickup**  
  Prevents the player from picking up this entity under any circumstances.

- **Generate Use Output**  
  Enables output events when the entity is used.

- **Enable motion on touch**  
  Enables physics simulation automatically when the entity is touched.

- **Ignore LOS**  
  Allows the entity to be picked up regardless of line of sight (e.g., through walls).

### Pickup and Interaction Properties

- **Override Max Hold Distance**  
  Enables overriding of default max pickup distance.

- **Max Hold Distance Override**  
  Overrides the default max pickup distance (default is 350 units).

### Physics Properties

- **BuoyancyFactor**  
  Controls the buoyancy strength; higher values increase buoyancy effects.

  
{{% alert warning %}}
<strong>Note:</strong> This is not implemented at the moment.
{{% /alert %}}


## Functions

### Pickup and Interaction

- **IsPickedUp()**  
  Checks if the entity is currently held by any player.

- **CanPlayerPickup()**  
  Determines if the entity is pickable by the player.

- **IgnoreLOS()**  
  Determines whether the line of sight requirement is ignored for pickup.

- **GetMaxHoldDistance()**  
  Retrieves the maximum allowed distance for the entity to be picked up.

- **GetSoundAttachment()**  
  Returns the attachment point used for sound or particle effects.

### Interaction

- **ToggleMotion(bool bEnabled)**  
  Toggles the physics simulation of the entity on or off.

## Events

These events allow level designers to create dynamic gameplay by responding to entity interactions:

- **OnUsed (Actor Instigator)**  
  Called when the entity is used by a player.

- **OnPickedUp (Controller)**  
  Called when the entity is picked up.

- **OnWaterEnter (WaterBody, Location, Velocity)**  
  Called upon entering a water body, providing details about the entry point.

{{% alert warning %}}
<strong>Note:</strong> This is not implemented at the moment.
{{% /alert %}}

## Example Usage

- **Interactive Crates or Barrels**: Entities that players can pick up and manipulate.
- **Environmental Hazards**: Objects that interact dynamically with the environment and can trigger gameplay events upon collisions or interactions with water.
- **Puzzle Mechanics**: Physics entities that players must strategically interact with to solve puzzles.

{{% alert info %}}
<strong>Note:</strong> The physics and replication setup ensures consistent gameplay experiences across networked games, automatically synchronizing physics states between players.
{{% /alert %}}

