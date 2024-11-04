---
type: docs 
title: Brush Button
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

# BrushButtonComponent

The **BrushButtonComponent** is a customizable button component for interactive gameplay. It allows designers to set up buttons with locking, unlocking, pressing, and resetting capabilities based on various conditions. It is applied for **StaticMesh** actor for example model created in **Unreal Engine Modeling Tools**. 

Of course you can also use imported model but **animations will be not supported!**

{{% alert info %}}
<strong>TODO: </strong>
<p>This component should have texture animation handling. When button is pressed, texture should change</p>
{{% /alert %}}

## Component Properties

### Primary Properties

- **bLocked**  
  Controls if the button is locked. When `true`, interactions are restricted, and the `OnUseLocked` event is triggered if interaction is attempted.

- **bOnlyOnce**  
  Determines if the button can only be used once. When `true`, the button disables further interactions after the first use, and `OnPressed` will not fire again.

- **fDelayBeforeReset**  
  Defines the delay (in seconds) after the button can be used again.

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
