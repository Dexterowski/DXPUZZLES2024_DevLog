---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Momentary Door
authors:
  - Dexterowski
---

# MomentaryDoor Entity

The **MomentaryDoor** entity provides a controlled, incremental door movement mechanism. It is specifically designed for integration with other brush-based entities (such as buttons or levers) that dynamically control the door’s opening progression.

## Properties

### Primary Properties

- **Locked**  
  Controls if the door is initially locked, preventing interactions.

- **Moving Direction**  
  The direction in which the door moves when activated (e.g., upward).

- **Move Distance**  
  Total distance the door moves when fully open.

- **Max Acceptable Progression**  
  Maximum progression value the door accepts, controlling how far the door can open.

- **Blocking Damage**  
  Damage inflicted upon an actor obstructing the door’s path.

- **Damage Interval**  
  Interval in seconds between applying consecutive damage to blocking actors.

## Sound Properties

- **Moving Sound**  
  Looping sound played while the door moves.

- **Stop Sound**  
  Sound played when the door stops moving.

- **Fully Open Sound**  
  Sound triggered upon fully opening.

- **Fully Close Sound**  
  Sound triggered when the door is completely closed.

- **Use Locked Sound**  
  Played when attempting interaction with a locked door.

- **Lock Sound**  
  Sound when door becomes locked.

- **Unlock Sound**  
  Sound when door is unlocked.

## Interaction Inputs

These inputs can be triggered by level scripting or blueprint logic:

- **SetProgression(Instigator, NewProgression)**  
  Sets the door's opening progression, allowing fine control over how far the door opens.

- **Lock(Instigator)**  
  Locks the door, preventing further interaction.

- **Unlock(Instigator)**  
  Unlocks the door for interaction.

## Events

Use these events for scripting interactions and responding dynamically:

- **OnProgressionChanged(Instigator, Progression)**  
  Triggered whenever door progression changes, providing current progression value.

- **OnUseLocked(Instigator)**  
  Fired when an interaction is attempted on a locked door.

- **OnDoorFullyOpen(Instigator)**  
  Triggered when the door reaches its maximum open position.

- **OnDoorFullyClosed(Instigator)**  
  Triggered when the door returns to its fully closed position.

## Usage Notes

The `MomentaryDoor` is best utilized in scenarios requiring partial door movements, such as puzzles involving incremental progression or controlled opening by other in-game mechanics.

