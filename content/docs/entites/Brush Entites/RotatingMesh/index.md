---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Rotating Mesh
authors:
  - Dexterowski
---

# RotatingMesh

The **RotatingMesh** is an interactive entity used for creating rotating obstacles or dynamic environmental elements with customizable rotation behaviors, interaction possibilities, and sound feedback.

## Primary Functionality

`RotatingMesh` entities can continuously rotate, accelerate/decelerate, and respond to collisions and interactions initiated by players or other actors.

## Editable Properties

### Rotation Settings
- **InitialState**: Determines the starting state of the rotating mesh:
  - *Idle*: No initial rotation.
  - *Accelerating*: Gradually speeds up to full rotation speed.
  - *Decelerating*: Gradually slows down to a stop.
  - *FullSpeed*: Immediately rotates at full speed.
- **FullRotationSpeed**: Maximum rotation speed (degrees per second).
- **AccelerationSpeed**: Rate at which the mesh accelerates or decelerates.
- **RotationAxis**: Defines the axis around which the mesh rotates (modifiable by reversing direction).
- **bUseAcceleration**: Enables or disables gradual acceleration/deceleration.
- **bReverseDirection**: Reverses the rotation axis direction at start.

### Blocking and Damage Settings
- **bStopWhenBlocked**: Stops rotation immediately upon collision.
- **SlowdownMultiplier**: Reduces speed upon collision. `0` stops instantly; higher values slow gradually.
- **BlockingDamage**: Damage dealt to objects blocking the mesh.
- **DamageToPlayer**: Damage applied to players on collision.
- **bDamagePlayerOnCollision**: Enables damage application to players upon collision.

### Sound Settings
- **RotatingSound**: Looping sound during rotation.
- **PitchModifier**: Multiplier for sound pitch relative to rotation speed.

## Events

Events trigger based on interactions and state changes:

- **OnUsed**: Triggered upon interaction with the mesh (`Instigator` actor initiating the action).
- **OnStart**: Fires when rotation begins.
- **OnStopped**: Triggered when the rotation starts to decelerate or stops immediately.
- **OnFullyStopped**: Triggered when the rotation completely stops.
- **OnBlocked**: Fired when rotation is obstructed by another actor.

## Inputs
- **Start(Instigator, bReverse)**: Initiates rotation; optionally reverses direction.
- **Stop(Instigator)**: Begins deceleration or immediately stops rotation based on settings.
- **ForceStop(Instigator)**: Immediately stops rotation regardless of settings.
- **Toggle(Instigator)**: Toggles rotation on or off depending on the current state.
- **SetSpeed(NewSpeed, Instigator)**: Updates the rotation speed.
- **StopAtStartPosition(Instigator)**: Requests rotation to cease upon returning to initial orientation.

## Debug Functions
(Available for debugging in-editor)
- **DebugStart**: Starts rotation.
- **DebugStop**: Stops rotation.
- **DebugToggle**: Toggles rotation state.
- **DebugStopAtStartPosition**: Stops rotation at the initial position.
- **DebugInstantStop**: Immediately halts rotation.

## Usage Example
Place a rotating platform in the level, set desired rotation speed, and manage interactions and damage through provided events and inputs. Ideal for dynamic obstacles or interactive puzzle elements in levels.