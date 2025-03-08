---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Generic Trigger
authors:
  - Dexterowski
---

# DXTrigger Entity (Generic Trigger)

The **DXTrigger** is a basic trigger entity derived from the [DXBaseTrigger](../basetrigger). It's primarily used to handle straightforward triggering events within DX PUZZLES without additional functionalities.

## Overview

**DXTrigger** entities detect overlaps with actors to initiate predefined events, suitable for straightforward triggering scenarios.

## Usage

Place this trigger in areas where simple activation of game logic upon actor entry or exit is required, such as starting events, enabling/disabling gameplay elements, or simple puzzles.

## Inherited Properties

The **DXTrigger** inherits all primary properties, filters, and events from the [DXBaseTrigger](../basetrigger).

### Inherited Primary Properties

- **Is Enabled**
- **Trigger only once**
- **Next trigger delay**

### General Filters

- **Everything**
- **Players**
- **Physics**
- **NPCs**

## Events

- **OnStartTouch**  
  Triggered when an actor enters the trigger.

- **OnEndTouch**  
  Triggered when an actor exits the trigger.

- **On Enabled**  
  Fired when the trigger is enabled.

- **On Disabled**  
  Fired when the trigger is disabled.

---

This documentation is aimed at level designers and modders using basic triggers in DX PUZZLES.

