# Getting Started with openpilot

Welcome to the openpilot getting started guide! This comprehensive guide will walk you through the system requirements, installation process, and basic usage of the openpilot system.

## System Requirements

Before installing openpilot, ensure that your vehicle and hardware meet the following requirements:

1. Compatible vehicle: Check the [vehicle compatibility list](https://github.com/commaai/openpilot/blob/master/docs/CARS.md) to see if your car is supported.
2. Comma device: You'll need a Comma Two, Comma Three, or compatible hardware to run openpilot.
3. Smartphone: An Android or iOS device to run the Comma Connect app.

## Installation

Follow these steps to install openpilot on your Comma device:

1. Connect your Comma device to your vehicle's OBD-II port.
2. Power on the device and connect it to Wi-Fi.
3. The device will automatically download and install the latest version of openpilot.
4. Once installation is complete, the device will reboot.

## Basic Usage

### Starting a Drive

1. Start your vehicle and ensure the Comma device is powered on.
2. Wait for the openpilot system to initialize (you'll see the driving visualization on the screen).
3. Enable openpilot by pressing the "Enable" button on the device screen or by engaging your vehicle's cruise control.

### During a Drive

- openpilot will handle steering, acceleration, and braking in supported situations.
- Always remain alert and ready to take control of the vehicle at any time.
- Monitor the device screen for any alerts or notifications.

### Ending a Drive

1. Disengage openpilot by pressing the brake pedal or turning off cruise control.
2. Park your vehicle and turn off the ignition.
3. The Comma device will automatically shut down after a short period.

## Key Components

Understanding the main components of openpilot will help you get the most out of the system:

### Controls

The `Controls` class in `selfdrive/controls/controlsd.py` is responsible for managing the overall control of the vehicle. It handles:

- Initialization of car parameters and interfaces
- State control and actuator commands
- Publishing control messages

Key methods include:

- `update()`: Updates sensor data and calibration
- `state_control()`: Manages the control state and generates actuator commands
- `publish()`: Sends control messages to the vehicle

### Lateral Control

openpilot uses different lateral control methods depending on the vehicle's configuration:

- Angle-based control (`LatControlAngle`)
- PID-based control (`LatControlPID`)
- Torque-based control (`LatControlTorque`)

These are implemented in the `LaC` (Lateral Control) object within the `Controls` class.

### Longitudinal Control

The `LongControl` class handles longitudinal control (acceleration and braking). It uses a PID controller to maintain desired speeds and following distances.

## Next Steps

Now that you've got the basics, here are some suggestions for further exploration:

1. Familiarize yourself with the [openpilot documentation](https://github.com/commaai/openpilot/tree/master/docs) for more detailed information.
2. Join the [comma.ai Discord](https://discord.comma.ai) to connect with other openpilot users and developers.
3. Explore the [openpilot GitHub repository](https://github.com/commaai/openpilot) to learn more about the codebase and contribute to the project.

Remember, safe driving is your responsibility. Always stay alert and be ready to take control of your vehicle at any time while using openpilot.