---
type: docs 
title: Real world time sync
date: 2024-12-02T22:34:47+08:00
featured: false
draft: false
comment: false
toc: true
reward: false
pinned: false
carousel: false
series: 

 
categories: Point Actors
 
tags: 
  - Unreal Engine
  - Unreal Engine 5.5
  - Source Engine
  - Half Life 2
authors:
  - Dexterowski
---

{{% alert danger %}}

This entity is **UNUSED** because of **Ultimate Dynamic Sky** plugin introduced to the project. It has itself replication between clients and comes with out of the box real-world time replication and necessary functions and properties.

{{% /alert %}}

# ARealworldTimeSync Class Documentation

**Inheritance:**  
`AActor` → `ARealworldTimeSync`

## Overview

`ARealworldTimeSync` is an actor designed to synchronize real-world time to the game. Can be used to set some events based only on real time. 

{{% alert info %}}

Time is gathered from server or listen-server. (Hosting player)

{{% /alert %}}

## Key Features

- **Real-World Time Synchronization:**  
  Updates a replicated time variable that reflects server's real-world time.
  
- **Flexible Refresh Rate:**  
  The frequency of updates can be customized via `SyncRefreshTime`, controlling how often real-world time is synced to clients.

- **Blueprint Integration:**  
  Includes BlueprintCallable and BlueprintAssignable functions and events.

## Properties

### Time Synchronization

- **SyncTime** (`bool`, ReplicatedUsing=`OnRep_SyncTimeToggle`)  
  Determines if the actor should actively sync the in-game time-of-day with the server's real-world time.  
  - *Tooltip:* "That makes time of day system in the map to be synced with the server real time world clock"
  - *Default:* true

- **RealWorldTime** (`int32`, ReplicatedUsing=`OnRep_TimeChanged`)  
  An integer representing the server's current real-world time.  
  - *Note:* The exact meaning/format of this time (e.g., seconds since midnight, UTC timestamp) can be defined by game logic.  
  - Triggers `OnRep_TimeChanged` when updated.

- **SyncRefreshTime** (`int32`, EditInstanceOnly)  
  The frequency (in seconds) at which the actor will attempt to re-sync the real world time with the server.  
  - *Default:* 60 seconds


### Delegates and Events

- **OnTimeChanged** (`FOnTimeChanged`)  
  A multicast delegate called when the `RealWorldTime` changes, allowing listeners (e.g., UI Widgets, time-of-day managers) to react and update accordingly.  
  **Signature:** `void OnTimeChanged(int32 CurrentTime)`

## Public Methods

- **SyncNow()** (`UFUNCTION(BlueprintCallable)`)  
  Forces an immediate synchronization of the `RealWorldTime` with the server’s current real time.

- **GetRealWorldTime()** (`UFUNCTION(BlueprintGetter)`)  
  Returns the current value of `RealWorldTime`. This can be used to retrieve the last synchronized real-world time on the client side.

## Replication and Networking

- **OnRep_TimeChanged()** (`UFUNCTION`)  
  Called on clients whenever `RealWorldTime` is updated. Triggers `OnTimeChanged` delegate, allowing client-side logic to respond immediately.

- **OnRep_SyncTimeToggle()** (`UFUNCTION`)  
  Called on clients whenever `SyncTime` is toggled. Could be used to enable/disable client-side logic that depends on ongoing synchronization.

## Event Flow

1. **Server Initialization:**  
   Upon `BeginPlay()`, the server initializes the actor and sets `NextSync` to schedule the first synchronization.

2. **Periodic Syncing (Server-side):**  
   In `Tick(DeltaTime)`, if the current time exceeds `NextSync` and `SyncTime` is true, the server updates `RealWorldTime` to reflect the current real time

3. **Client Updates:**  
   When `RealWorldTime` or `SyncTime` changes, replication notifies clients via `OnRep_TimeChanged()` and `OnRep_SyncTimeToggle()`. Clients can then adjust their UI or time-of-day representations accordingly.

4. **Manual Sync:**  
   If the `SyncNow()` function is called (from server or client if the client has permission), the server updates `RealWorldTime` immediately and broadcasts the change to all clients.

## Usage Example

1. **Placement and Configuration:**  
   Place `ARealworldTimeSync` in the level. Set `SyncRefreshTime` to the desired frequency (e.g., every 60 seconds). Ensure `SyncTime` is true if real-world syncing is desired.

2. **Listening for Changes (Blueprint):**  
   In a Blueprint that manages the day/night cycle or displays the current time, bind to `OnTimeChanged`. When triggered, read `GetRealWorldTime()` to adjust the in-game clock accordingly.

3. **Forcing an Update:**  
   At any time, call `SyncNow()` to request an immediate time sync. This can be useful before scheduled in-game events, ensuring times are up-to-date.

This actor ensures that all players experience a game world closely tied to real-world timing, enhancing immersion and enabling real-time events or persistent world mechanics.
