---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Breakable Physics Entity
authors:
  - Dexterowski
categories: Entities
tags:
  - DX PUZZLES
  - Entities
  - Physics
  - Breakable
---

# BreakablePhysProp

The **BreakablePhysProp** is an entity designed for creating interactive, breakable physics props. These entities respond to impacts, can break into smaller pieces, and interact dynamically within the environment.

{{% alert danger %}}
<strong>Breaking the entity is very buggy at the moment, it is not replicated and it will be not. It consumes a lot of network traffic. Breaking the entity can cause at the moment a lot of lags!
{{% /alert %}}


## Overview

**BreakablePhysProp** entities allow for realistic breakage behavior upon taking damage or impact. These entities utilize geometry collections for advanced destruction effects, suitable for creating interactive physics puzzles or dynamic environment elements.

## Primary Properties

- **Health**  
  Defines the initial and maximum health of the object. When health reaches zero, the object breaks.

- **DisableCollisionOnBreak**  
  If enabled, fractured pieces lose collision upon breaking, preventing further interactions.

## Events

- **OnBreak**  
  Fired when the object breaks due to damage or impact, allowing designers to trigger additional game events or effects.

## Functions

### Damage Handling

- **GetHealth()**  
  Retrieves the current health of the entity.

- **GetMaxHealth()**  
  Returns the maximum health value of the entity.

- **IsAlive()**  
  Checks if the entity is still intact (not yet broken).

- **CanApplyDamage(Filter)**  
  Checks if the specified damage type can be applied to the entity based on its damage filters.

- **ApplyDamage(DamageInfo)**  
  Applies damage to the entity. If health reaches zero or below, the entity breaks.

- **Heal(Amount)**  
  Restores health to the entity.

### Geometry Collection

- **GetGeometryCollection()**  
  Provides access to the entity's geometry collection component responsible for the breakable physics behavior.

### Break Event

- **Break(DamageInfo)**  
  Immediately triggers the breaking behavior, including visual effects, and physics-based fracture simulation.

## Flags

- **DisableCollisionOnBreak**  
  Disables collision for fractured elements after breaking, improving performance or gameplay flow.

## Events

### Break Event

- **OnBreak(DamageInfo)**  
  Triggered upon breaking, providing information about the damage event for scripting additional responses.

## Example Usage

- **Fragile puzzle props** that break under specific conditions to advance puzzles.
- **Destructible environment elements** such as crates, pots, or vases for realistic interaction.

{{% alert info %}}
<strong>Note:</strong> Physics and replication are fully managed, ensuring consistent behavior in multiplayer scenarios.
{{% /alert %}}

