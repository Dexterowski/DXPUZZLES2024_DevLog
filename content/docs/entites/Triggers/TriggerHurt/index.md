---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Trigger Hurt
authors:
  - Dexterowski
---

# DXTriggerHurt Entity

The **DXTriggerHurt** entity applies continuous damage to actors within its bounds at specified intervals. Ideal for hazards such as environmental traps, damaging fields, or hazardous areas.

![Example of trigger hurt usage](UnrealEditor_z8pNlJEg5q.jpg)
*Example of usage - hazardous water*

## Hurt Properties

- **Damage Type**  
  Defines the type of damage applied (e.g., fire, electric, crushing).

- **Random Damage**  
  Enables randomization of damage within the specified range.

- **Damage Range**  
  Specifies the minimum and maximum damage applied when random damage is enabled.

- **Damage Amount**  
  Fixed amount of damage applied per interval when random damage is disabled.

- **Damage Interval**  
  Interval (in seconds) between consecutive damage applications.

- **Override Damage Force**  
  Enables applying a specific force upon damaging an actor.

- **Damage Force**  
  The vector force applied to an actor upon damage if overriding damage force is enabled.

## Inherited Properties (from DXBaseTrigger)

- **Is Enabled**
- **Trigger Only Once**
- **Next Trigger Delay**

### General Filters

- **Everything**
- **Players**
- **Physics**
- **NPCs**

## Events

- **OnDamageApplied**  
  Triggered each time damage is applied to an actor within the trigger area.

- **OnStartTouch** (Inherited)  
  Fired when an actor starts overlapping the trigger area.

- **OnEndTouch** (Inherited)  
  Fired when an actor stops overlapping the trigger area.

## Inputs (Callable Actions)

- **Set New Hurt Properties**  
  Dynamically changes the hurt properties during gameplay.

## Usage Notes

Place **DXTriggerHurt** entities in areas intended to damage actors, such as hazards like spikes, toxic pools, or electric fields.