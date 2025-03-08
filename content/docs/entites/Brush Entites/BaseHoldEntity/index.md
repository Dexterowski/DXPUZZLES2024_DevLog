---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Base Hold Entity
authors:
  - Dexterowski
---

# Base Hold Entity

Base **abstract** class for entities that require the player to hold down a button to interact. Suitable for buttons, levers, or other interactable objects.

## Properties

### General Properties

- **Don't return interaction** (`bool`):  
  When enabled, the entity does not return to its original state after completing an interaction.

- **Must Look At**  (Not implemented yet) 
  Requires the player to directly look at the entity to interact.

- **End Progress Value**  
  Determines the interaction's completion point. When the progress reaches this value, the interaction is considered complete.

- **Actual Progression** (Read-only)  
  Shows the current interaction progress. Level designers can observe but not directly modify this value.

- **Don't return after completion**  
  Prevents the entity from automatically returning to the initial state after the interaction is completed.

- **Delay before next use**  
  Specifies a cooldown period after the entity has been used before it can be interacted with again.

### Return Mechanics

- **Don't Return** (`bDontReturnWhenComplete`)  
  Determines if the entity remains at its completed state indefinitely after an interaction.

- **Return Speed**  
  Controls how quickly the entity returns to its initial state after interaction completion.

- **Delay after start returning**  
  The delay before the entity begins returning to its original state after interaction ends.

### Sound Properties

- **Lock Sound**  
  Plays when the entity becomes locked.

- **Unlock Sound**  
  Plays when the entity is unlocked.

- **Started Returning Sound**  
  Plays when the entity begins to return to its initial state.

- **Returning Sound (looped)**  
  Continuously plays while the entity is returning to its original state.

- **Complete Progression Sound**  
  Sound played upon interaction completion.

- **Use Progression Sound As Return**  
  If enabled, the sound used during progression is reused during the return phase.

## Events

### Output Events
These events allow designers to link entity interactions to other gameplay systems.

- **OnLocked**  
  Triggered when the entity is locked by an actor.

- **OnUnlocked**  
  Triggered when the entity is unlocked by an actor.

- **OnProgressionCompleted**  
  Triggered when an actor successfully completes interaction.

- **OnStartedProgression**  
  Triggered when interaction begins progressing.

- **OnStartedReturning**  
  Triggered when the entity begins to return to its original state after interaction.

- **OnProgressionChanged**  
  Fires continuously as the interaction progresses, providing the current progress value.

- **OnStateChanged**  
  Fires whenever the entity changes states (e.g., idle, progressing, returning).

## States

- **Idle**  
  The default state, awaiting interaction.

- **Progress**  
  Indicates that an interaction is currently in progress.

- **Completed**  
  The interaction is completed, awaiting further instructions based on return settings.

- **Returning**  
  The entity is returning to its original state.

- **Wait For Return**  
  The entity waits for a specified delay before beginning to return.

## Interaction Methods

These methods manage entity interactions:

- **Lock**  
  Locks the entity, preventing further interactions.

- **Unlock**  
  Unlocks the entity for interaction.

- **SetProgression**  
  Manually sets the interaction progress.

