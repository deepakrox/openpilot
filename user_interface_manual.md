# User Interface Manual

This manual provides a comprehensive guide to the openpilot user interface, explaining all the elements, their meanings, and how users can interact with them while driving.

## Table of Contents

1. [Introduction](#introduction)
2. [Main Display Elements](#main-display-elements)
3. [Status Indicators](#status-indicators)
4. [Interaction and Controls](#interaction-and-controls)
5. [Brightness and Display Settings](#brightness-and-display-settings)
6. [Alerts and Notifications](#alerts-and-notifications)

## Introduction

The openpilot user interface is designed to provide drivers with essential information and controls while maintaining a clean and intuitive layout. This manual will walk you through the various elements of the UI and explain how to interact with them effectively.

## Main Display Elements

### 1. Road View

The central part of the display shows a real-time view of the road ahead, enhanced with augmented reality elements to highlight lane markings, detected vehicles, and the projected path of your vehicle.

### 2. Speed Display

Located prominently, usually in the top-left corner, this shows your current speed. The display can be configured to show speed in either mph or km/h, depending on your preference.

### 3. Steering Wheel Icon

This icon indicates the current state of lateral control. When openpilot is actively steering, the icon will be highlighted.

### 4. Acceleration/Deceleration Indicator

Usually represented by vertical bars or arrows, this shows whether the system is currently accelerating or decelerating.

## Status Indicators

### 1. Engagement Status

The UI clearly indicates whether openpilot is currently engaged, disengaged, or in an override state. This is typically shown through color coding and/or text indicators.

- Engaged: Usually indicated by a green color
- Disengaged: Typically shown in white or gray
- Override: Often displayed in blue or yellow

### 2. System Health

Various icons or indicators show the health status of different components:

- Camera status
- GPS signal strength
- Network connectivity
- Device temperature

### 3. Light Sensor Reading

The UI adapts to ambient light conditions. The `light_sensor` value in the code determines the screen brightness and potentially adjusts UI elements for better visibility.

## Interaction and Controls

### 1. Touch Screen Interactions

While the primary interface is designed for minimal interaction during driving, certain elements may be touch-responsive for quick adjustments or information access.

### 2. Steering Wheel Controls

Many interactions with openpilot are done through steering wheel controls. These typically include:

- Engaging/disengaging openpilot
- Adjusting set speed
- Changing following distance

## Brightness and Display Settings

The display automatically adjusts its brightness based on ambient light conditions. This is handled by the `Device::updateBrightness` function in the provided code.

```cpp
void Device::updateBrightness(const UIState &s) {
  float clipped_brightness = offroad_brightness;
  if (s.scene.started && s.scene.light_sensor >= 0) {
    clipped_brightness = s.scene.light_sensor;
    // ... (brightness calculation)
  }
  // ... (apply brightness)
}
```

The brightness is scaled between 10% and 100% based on the light sensor reading.

## Alerts and Notifications

The UI provides various alerts and notifications to keep the driver informed about the system status and any required actions. These may include:

1. Takeover requests
2. System errors or malfunctions
3. Navigation instructions (if applicable)
4. Speed limit warnings

Alerts are typically displayed prominently on the screen, often with accompanying sounds to ensure the driver's attention.

---

This manual provides an overview of the main elements and functions of the openpilot user interface. As the system may receive updates, some elements might change or new features may be added. Always refer to the latest version of the manual and any supplementary documentation provided with system updates.