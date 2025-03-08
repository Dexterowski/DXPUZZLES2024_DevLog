---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Brush Button (not moving)
authors:
  - Dexterowski
---

# Brush Button

The `BrushButton` is a static button entity within DX PUZZLES 2024 that triggers events and plays specified sounds when interacted with. It doesn't move or change materials and is designed for straightforward event-based interactions.

{{< alert "info" >}}

For moving or rotating button see Brush Moving Button or Brush Rotating Button

{{</ alert>}}

---
## Exposed Properties

These properties can be edited directly in the Unreal Editor:


### Button Properties

- **Only Once (`bOnlyOnce`)**  
  Determines if the button can be pressed multiple times or only once.

- **Touch Activates (`bTouchActivates`)**  
  Enables the button to activate upon being touched by a player.

- **Time before reset (`fDelayBeforeReset`)**  
  The cooldown time in seconds before the button can be pressed again.

- **Locked (`bLocked`)**  
  Indicates whether the button is locked. When locked, interaction attempts trigger a separate locked event.

## Audio Properties

- **Press Sound (`PressSound`)**  
  The sound played when the button is successfully pressed.

- **Lock Sound (`LockSound`)**  
  Sound played when the button gets locked.

- **Unlock Sound (`UnlockSound`)**  
  Sound played when the button is unlocked.

---
## Events

### Output Events

- **OnPressed**  
  Fired when the button is successfully pressed (unlocked and not in cooldown).

- **OnUsed**  
  Fired every time interaction is attempted, regardless of whether the button is locked or in cooldown.

## Input Actions

The following input actions can be triggered from external scripts or Blueprints:

- **Lock(Instigator)**  
  Locks the button, preventing further interactions until unlocked.

- **Unlock(Instigator)**  
  Unlocks the button, allowing interactions again.

- **Press(Instigator)**  
  Attempts to press the button, triggering associated events and sounds if conditions (lock state, cooldown, and interaction flags) are met.

---

## Usage Example

A typical usage scenario includes linking this button with door or event systems, where pressing the button triggers specific game mechanics or state changes. The button's state (locked or unlocked) can dynamically change throughout gameplay via scripting or Blueprints.