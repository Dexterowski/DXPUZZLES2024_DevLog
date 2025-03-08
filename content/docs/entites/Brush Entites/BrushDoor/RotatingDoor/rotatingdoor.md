---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Rotating Door
categories: Entities
tags:
  - DX PUZZLES
  - Entities
authors:
  - Dexterowski
---

# DoorRotating Entity

The **DoorRotating** entity extends the functionality of the basic **Door** entity by adding rotation-based movement. Ideal for doors that pivot on hinges, it provides features for interactive opening, closing, locking, unlocking, and bidirectional opening.

## Properties

### Rotation Properties

- **Rotation Axis**  
  Defines the axis around which the door rotates. Typically set to vertical (`Z`) for standard doors.

- **Two Way** (`bTwoWay`)  
  Allows the door to open in both directions based on the instigator's position relative to the door. If enabled, the door dynamically determines the direction to swing open when activated.

## Inherited Primary Properties (from Door)

- **Locked**  
  Determines whether the door is locked, preventing interactions when active.

- **Touch Opens**  
  Enables automatic opening when touched by an actor.

- **Can open door with interact key? (E)**  
  Allows manual door operation through interaction input.

- **Can't Use While Moving**  
  Disables interactions while the door is currently in motion.

- **Stay Open Forever**  
  Prevents automatic closing once the door is opened.

- **Don't Close**  
  If true, doors remain open and must be manually closed.

- **Use Moving Sound**  
  Enables looping sounds during door movement.

## Movement-Specific Properties

- **Moving Speed**  
  Controls the speed of the door's rotational movement.

- **Move Distance**  
  Specifies the angular distance of rotation from the closed position to the fully open position.

- **Delay Before Close**  
  Defines how long the door stays open before automatically closing.

- **Delay Before Reset**  
  Sets a cooldown period between consecutive door interactions.

## Blocking Damage Properties

- **Blocking Damage**  
  Damage dealt to any actor blocking door movement.

- **Damage Interval**  
  Time between damage applications when blocked.

- **Force Movement**  
  Forces door movement to continue despite obstructions.

- **Stop on Block**  
  Door stops when blocked and continues automatically upon clearance.

## Inputs (Callable Actions)

These inputs can be called from DX PUZZLES editor scripts or blueprints:

- **Lock(Instigator)**  
  Locks the door, restricting interactions.

- **Unlock(Instigator)**  
  Unlocks the door, allowing interactions.

- **Open(Instigator)**  
  Opens the door, rotating it based on the rotation axis and actor's position.

- **Close(Instigator)**  
  Closes the door, returning it to its initial rotation.

- **Interact(Instigator)**  
  Toggles the door state (open/close), respecting locked states and current status.

## Output Events

Events available for additional level logic and scripting:

- **OnDoorOpen**  
  Triggered at the start of door opening.

- **OnDoorClose**  
  Triggered at the start of door closing.

- **OnDoorFullyOpened**  
  Fired when the door reaches the fully open rotation.

- **OnDoorFullyClosed**  
  Triggered when the door fully closes.

- **OnDoorLocked**  
  Fires when the door becomes locked.

- **OnDoorUnlocked**  
  Fires when the door is unlocked.

- **OnUseLocked**  
  Activated when interaction is attempted on a locked door.

---

This documentation is crafted to assist level designers and modders in implementing and configuring rotating doors within DX PUZZLES gameplay environments.

