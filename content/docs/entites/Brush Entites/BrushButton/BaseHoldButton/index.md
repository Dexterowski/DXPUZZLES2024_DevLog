---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Brush Hold Button
authors:
  - Dexterowski
---

# BrushHoldButton

The **BrushHoldButton** is a dynamic entity designed for interactive puzzle or gameplay mechanics in Unreal Engine 5. It inherits from the [**Base Hold Entity**](../BaseHoldEntity/) and provides functionality to create moving or rotating button-like objects within your level.

## Class Hierarchy
```
ABaseHoldEntity
└── ABrushHoldButton
```

## Properties

### Movement Properties

- **Button move distance** (`fMoveDistance`)
  - Defines how far the button moves from its initial position when activated.
  - Default value: `100.0`

- **Button move direction** (`MoveDirection`)
  - Vector direction in which the button moves.
  - Effective only if movement is enabled.

### Rotation Properties

- **Button rotation distance** (`fRotationDistance`)
  - Specifies the degree of rotation the button performs.

- **Button rotation axis** (`RotationAxis`)
  - Determines around which axis (X, Y, Z) the button rotates.
  - Set this axis carefully; normalization is handled internally.

## Editor Conditions

- **Enable Move Button** (`bIsMovingButton`)
  - Enable this to allow the button to move. When enabled, specify `Button move distance` and `Button move direction`.

- **Enable Rotation Button** (`bIsRotatingButton`)
  - Enables rotation functionality for the button.

## Usage

- Buttons are ideal for creating interactive puzzles or switches in your levels.
- Movement or rotation happens gradually based on progression, creating smooth transitions.
- Movement direction and rotation axis should be clearly defined in the editor to ensure proper functionality.

## Behavior

- Upon activation, the button will either move or rotate based on configured properties.
- Button smoothly transitions from the initial state to the specified moved or rotated state.

## Examples

- **Sliding Button**: Set `MoveDirection` to `(1,0,0)` and `Button move distance` to `50` to create a button sliding horizontally by 50 units.
- **Rotating Button**: Set `Rotation axis` to `(0,0,1)` and `Rotation distance` to `90` to create a button that rotates 90 degrees around the Z-axis.

Ensure all relevant properties are enabled (`bIsMovingButton`, `bIsRotatingButton`) to activate desired behaviors.

