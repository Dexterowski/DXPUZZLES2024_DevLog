---
type: docs 
title: Brush Button Moving
date: 2024-11-04T08:26:47+02:00
featured: false
draft: false
comment: false
toc: true
reward: false
pinned: false
carousel: false
series: 
 
categories: Entites
 
tags: 
  - Unreal Engine
  - Unreal Engine 5.5
  - Entities
authors:
  - Dexterowski
---

# BrushMovingButtonComponent

The **BrushMovingButtonComponent** is an extended version of the **BrushButtonComponent**, adding functionality for the button to move between positions when pressed and return after a delay. It includes properties to configure movement speed, distance, direction, and events specific to its motion.

<center>
<video autoplay="true" loop="true" width="100%" src="DXPUZZLES_MovingButton.mp4" title="Title"></video>
</center>

## Component Properties

### Primary Properties

- **bLocked**  
  Controls if the button is locked. When `true`, interactions are restricted, and the `OnUseLocked` event is triggered if interaction is attempted.

- **bOnlyOnce**  
  Determines if the button can only be used once. When `true`, the button disables further interactions after the first use, and `OnPressed` will not fire again.

- **fDelayBeforeReset**  
  Defines the delay (in seconds) before the button can reset after being pressed.

### Movement-Specific Properties

- **fMovingSpeed**  
  Sets the speed at which the button moves between positions.

- **fMoveDistance**  
  The distance the button will move from its start position when pressed.

- **MovingDirection**  
  The direction in which the button will move when pressed. This is defined as a vector (e.g., `(0, 1, 0)` for movement along the Y-axis).

- **fStayTime**  
  Defines the time (in seconds) the button remains in its "pressed" state before beginning to return to the start position.


  {{% alert warning %}}

  <strong>Movement is not predicted!</strong>
  <p>You can use <strong>SMOOTH SYNC</strong> component with BrushMovingComponent to smooth movement! Without it can be laggy when player has poor connection</p>
  
  {{% /alert %}}

### Audio Properties

- **PressSound**  
  The sound that plays when the button is successfully pressed.

- **LockSound**  
  The sound that plays when the button is locked.

- **UnlockSound**  
  The sound that plays when the button is unlocked.

- **UseLockedSound**  
  The sound that plays when an attempt is made to interact with a locked button.

## Events

### Output Events

These events trigger under specific conditions and can be linked to other gameplay elements.

- **OnPressed**  
  Fired only when the button is successfully pressed, provided it’s not locked and has reset based on `fDelayBeforeReset`. The `Instigator` parameter represents the actor interacting with the button.

- **OnUseLocked**  
  Triggered when an interaction attempt is made on a locked button. `Instigator` refers to the actor that tried to interact.

- **OnLocked**  
  Fires when the button is locked. `Instigator` refers to the actor that initiated the lock.

- **OnUnlocked**  
  Fires when the button is unlocked. `Instigator` refers to the actor that initiated the unlock.

- **OnUsed**  
  Fires whenever an interaction attempt occurs, regardless of the button’s state (locked or unlocked). This event bypasses `fDelayBeforeReset`, meaning it triggers on every interaction attempt. `Instigator` represents the actor attempting interaction.

- **OnStartMoving**  
  Triggers when the button begins moving from its start position to the end position. `Instigator` is the actor initiating the movement.

- **OnStopMoving**  
  Triggers when the button stops moving after reaching the end position. `Instigator` is the actor initiating the movement.

- **OnReturned**  
  Fires when the button returns to its start position after the stay time elapses. `Instigator` is the actor who initially pressed the button.

### Input Actions

These are actions designers can use to control the button’s state and behavior.

- **Lock(Instigator)**  
  Locks the button, preventing interactions. Fires the `OnLocked` event and plays the `LockSound`. `Instigator` is the actor initiating the lock.

- **Unlock(Instigator)**  
  Unlocks the button, allowing interactions. Fires the `OnUnlocked` event and plays the `UnlockSound`. `Instigator` is the actor initiating the unlock.

- **Press(Instigator)**  
  Simulates pressing the button. Sets the button’s state to "pressed" and fires the `OnPressed` event if it’s unlocked. `Instigator` is the actor pressing the button.

- **Interact(Instigator)**  
  Attempts an interaction with the button. If the button is locked, it triggers `OnUseLocked`; otherwise, it calls `Press`. Fires `OnUsed` regardless of the button’s state. `Instigator` is the actor attempting interaction.
