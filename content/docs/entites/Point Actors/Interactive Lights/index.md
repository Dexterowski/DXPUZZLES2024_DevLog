---
type: docs
date: 2025-03-08T08:26:47+02:00
title: Interactive Lights
authors:
  - Dexterowski
categories: Entities
tags:
  - DX PUZZLES
  - Lighting
  - Interactive Entities
  - Networking
---

# Interactive Lights

Interactive Lights allow level designers to create dynamic, and responsive lighting within their maps. These lights can change states, colors, intensities, and even animate intensity over time based on interactions or scripted events.

## Class Hierarchy
```
AActor
└── AInteractiveLight (Abstract)
    ├── AInteractivePointLight
    ├── AInteractiveRectLight
    └── AInteractiveSpotLight
```

## Common Properties
All Interactive Lights share the following core properties:

### Light Settings

- **Light Enabled** (`bIsLightEnabled`):
  - Toggles visibility of the light.
  - Network-synchronized.

- **Light Color** (`LightColor`):
  - Defines the color of the emitted light.
  - Network-synchronized.

- **Light Intensity** (`LightIntensity`):
  - Defines how strong the emitted light is.
  - Network-synchronized.

- **Animated Light Intensity Curve** (`IntensityCurve`):
  - Allows animating the intensity over time.
  - If unset, the intensity remains constant.

## Editable Properties (Level Editor)

- **Animated Light Float Curve** (`IntensityCurve`):
  - Allows animation of light intensity over time using a custom float curve.
  - Editable per instance.

## Interactive Functions
These functions can be triggered through gameplay interactions or scripted events within the editor:

- **Set Light Enabled** (`SetLightEnabled`):
  - Enables or disables the light.

- **Set New Light Color** (`SetNewLightColor`):
  - Changes the color dynamically during gameplay.

- **Set Light Intensity** (`SetNewIntensityCurve`):
  - Sets or replaces the intensity animation curve during gameplay.

- **Toggle Light**:
  - Toggles the current visibility state of the light.

## Networking Behavior

Interactive Lights are fully networked entities, ensuring all changes (state, intensity, color, animation curves) are replicated to clients.

- **Replication**:
  - Light state (`bIsLightEnabled`), color (`LightColor`), intensity (`LightIntensity`), and intensity curve (`IntensityCurve`) are synchronized across the network.

- **Authority-Only Functions**:
  - Changes to the lights can only be initiated by the authoritative server.
  - All state changes are replicated immediately to all connected clients, ensuring synchronization.

## Events

Interactive lights expose events to allow designers to integrate lights with broader game logic.

- **OnLightEnabledChanged**:
  - Fired when the light is enabled or disabled.

- **OnLightColorChanged**:
  - Triggered when the light color is changed.

- **OnIntensityCurveChanged**:
  - Fired when the intensity animation curve is updated.

## Usage Examples

- **Dynamic Lighting Effects**:
  - Use animated intensity curves to create flickering or pulsing lights, ideal for creating immersive environments.

- **Interactive Gameplay**:
  - Lights can react to player interactions or puzzle completion, enhancing gameplay feedback.

## Performance Considerations
- Animating intensity can have performance implications. Optimize curve complexity and frequency of updates.
- Static lights without an intensity curve are optimized automatically by disabling unnecessary ticking.

## Specific Light Types

### Interactive Point Light (`AInteractivePointLight`)
- Uses standard point light behavior, illuminating in all directions from a point.
- Ideal for general-purpose illumination and dynamic environments.

- **Components**:
  - `UPointLightComponent`: Configurable standard point light settings.

### Interactive Rect Light
- Emits rectangular area lighting, suitable for detailed lighting scenarios such as indoor environments or specific surfaces.

- **Components**:
  - `URectLightComponent`: Configurable rectangular light settings.

### Interactive Spot Light
- Emits a focused beam of light, useful for spotlight effects, stage lights, or targeted illumination.

- **Components**:
  - `USpotLightComponent`: Configurable spotlight properties.

---

This documentation outlines functionality specifically intended for level designers and modders, enabling them to integrate dynamic, network-synchronized lighting into their maps effectively.

