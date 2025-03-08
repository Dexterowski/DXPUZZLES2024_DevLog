---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Brush Button (Moving & Rotating)
authors:
  - Dexterowski
---

# Brush Button Variants

This documentation covers two interactive button variants derived from the base `BrushButton` class: the **Moving Button** and the **Rotating Button**.

---

<center>
<video autoplay="true" loop="true" width="100%" src="DXPUZZLES_MovingButton.mp4" title="Moving Button"></video>
</center>

---

## Brush Button Moving

The **BrushButtonMoving** is an interactive button that physically moves to a specified position when activated. It triggers events and plays sounds similarly to the base button but adds physical movement.

## Exposed Properties

### Movement Properties

- **Move Direction (`MoveDirection`)**  
  Direction in which the button moves upon activation. Default is forward.

- **Move Distance (`fMoveDistance`)**  
  Distance the button moves when activated.

- **Move Speed (`fMoveSpeed`)**  
  Speed at which the button moves to its pressed position.

- **Stay Time (`fStayTime`)**  
  Time (in seconds) the button stays pressed before returning to its original position. Setting to `-1` means it stays pressed indefinitely.

## Events

- **OnUnpressed**  
  Triggered when the button returns to its initial position.

## Behavior

Upon interaction, the moving button travels in the defined direction by the specified distance and triggers the `OnPressed` event once it reaches its destination. After the defined stay time (`fStayTime`), it returns to its original position, triggering the `OnUnpressed` event.

---

# Brush Button Rotating

The **BrushButtonRotating** inherits from `BrushButtonMoving`, providing rotational movement rather than linear translation.

## Exposed Properties

### Rotation Properties

- **Rotation Distance (`RotationDistance`)** *(Degrees)*  
  Defines how much (in degrees) the button rotates when activated.

### Inherited Movement Properties

- **Stay Time (`fStayTime`)**  
  Duration the button stays in the rotated state before rotating back.

- **Move Speed (`fMoveSpeed`)**  
  Speed of rotation.

- **Move Direction (`MoveDirection`)**  
  Determines axis of rotation. The axis with the highest absolute value determines the rotation axis (Roll, Pitch, or Yaw).

## Behavior

Upon activation, the rotating button rotates around the axis determined by `MoveDirection`, achieving the specified `RotationDistance`. After reaching its rotation goal, it triggers the `OnPressed` event. If a reset (`fStayTime`) is defined, the button rotates back to its original orientation and triggers `OnUnpressed` upon completion.

---

Use these button variants to add dynamic interactivity within your levels, creating visually and mechanically appealing interactions for players.