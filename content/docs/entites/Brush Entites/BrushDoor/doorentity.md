---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Door Entity
categories: Entities
tags:
  - DX PUZZLES
  - Entities
authors:
  - Dexterowski
---


# Door Entity

The **Door** entity is a movable door actor used in DX PUZZLES, providing interactive functionalities like opening, closing, locking, and unlocking. It includes customizable movement behaviors, sound effects, and interaction options for seamless integration within the game's levels.



## Editor Visualizations

When selected in the editor, the door entity provides visual indicators to represent its movement direction and distance.

![Door Visualization Example](DXPuzzles2024_BrushDoorVisualization.png)



## Properties

### Primary Properties

- **Locked** (`bLocked`)  
  Determines whether the door is locked. A locked door cannot be opened through normal interactions.

- **Touch Opens** (`bTouchOpens`)  
  Enables door opening when touched by a player or another entity.

- **Player Use Opens**  
  Allows players to manually open the door using the interaction key (default "E").

- **Can't Use While Moving**  
  Prevents the door from being opened or closed while it is already moving.

- **Stay Open Forever** (`bStayForeverOpen`)  
  Once opened, the door remains permanently open and does not automatically close.

- **Don't Close** (`bDontClose`)  
  If enabled, the door won't close automatically after opening; it must be manually closed.

- **Use Moving Sound**  
  If enabled, the specified moving sound loops while the door is in motion.

## Movement Properties

- **Moving Direction**  
  Direction the door moves when opening, e.g., upward, sideways.

- **Moving Speed**  
  The speed at which the door moves.

- **Move Distance**  
  Distance the door travels from its starting position when opening.

- **Delay Before Close**  
  Delay before the door automatically closes after being opened. Effective only if automatic closing is enabled.

- **Delay Before Reset**  
  Delay after the door is used or locked, before it can be interacted with again.

## Blocking Damage Properties

- **Blocking Damage**  
  Amount of damage inflicted on objects obstructing the door's path.

- **Damage Interval**  
  Interval between consecutive damage applications to blocking objects.

- **Force Movement**  
  Forces door movement even when obstructed.

- **Stop on Block**  
  Stops door movement upon hitting an obstacle, resuming only when the path is clear.

## Audio Properties

- **Moving Sound**  
  Looped audio played while the door is moving.

- **Open Sound**  
  Sound played at the start of the door opening.

- **Close Sound**  
  Sound played at the start of the door closing.

- **Fully Open Sound**  
  Sound played when the door reaches a fully open state.

- **Fully Close Sound**  
  Sound played when the door fully closes.

- **Use Locked Sound**  
  Played when a locked door is attempted to be opened.

- **Lock Sound**  
  Played when the door locks.

- **Unlock Sound**  
  Played when the door unlocks.

## Inputs (Callable Actions)

These actions can be triggered within the DX PUZZLES editor or blueprints:

- **Lock(Instigator)**  
  Locks the door, preventing further interaction.

- **Unlock(Instigator)**  
  Unlocks the door, allowing interaction again.

- **Open(Instigator)**  
  Initiates the door opening sequence.

- **Close(Instigator)**  
  Initiates the door closing sequence.

- **Interact(Instigator)**  
  Toggles door state based on its current condition (opens if closed, closes if opened), respecting locked state and other properties.

## Output Events

Events emitted during door interactions, usable for scripting additional gameplay logic:

- **OnDoorOpen**  
  Triggered when door begins opening.

- **OnDoorClose**  
  Triggered when the door starts closing.

- **OnDoorFullyOpened**  
  Triggered when the door is completely open.

- **OnDoorFullyClosed**  
  Triggered when the door has fully closed.

- **OnDoorLocked**  
  Fired when the door is locked.

- **OnDoorUnlocked**  
  Fired when the door is unlocked.

- **OnUseLocked**  
  Triggered when an interaction is attempted on a locked door.
