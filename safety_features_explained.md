# Safety Features Explained

This document provides a detailed explanation of the safety features implemented in openpilot, including how they work and what users should be aware of when using the system.

## Table of Contents

1. [Lane Departure Warning (LDW)](#lane-departure-warning-ldw)
2. [Forward Collision Warning (FCW)](#forward-collision-warning-fcw)
3. [Adaptive Cruise Control (ACC)](#adaptive-cruise-control-acc)
4. [Automated Lane Centering](#automated-lane-centering)
5. [Driver Monitoring](#driver-monitoring)

## Lane Departure Warning (LDW)

The Lane Departure Warning system is designed to alert the driver when the vehicle is unintentionally drifting out of its lane.

### How it works

1. The system uses the `modelV2` data from the vision system to detect lane markings.
2. The `LaneDepartureWarning` class in `selfdrive/controls/lib/ldw.py` processes this data.
3. When the vehicle approaches or crosses a lane marking without an active turn signal, the system triggers a warning.

### Implementation details

```python
ldw = LaneDepartureWarning()
ldw.update(sm.frame, sm['modelV2'], sm['carState'], sm['carControl'])
msg.driverAssistance.leftLaneDeparture = ldw.left
msg.driverAssistance.rightLaneDeparture = ldw.right
```

### User awareness

- Ensure that lane markings are clearly visible for optimal performance.
- The system may not work in all weather conditions or on all road types.
- LDW is an alert system and does not actively control the vehicle.

## Forward Collision Warning (FCW)

Forward Collision Warning alerts the driver of potential frontal collisions with vehicles ahead.

### How it works

1. The radar and vision systems continuously monitor the area in front of the vehicle.
2. The `RadarD` class in `selfdrive/controls/radard.py` processes radar data and fuses it with vision data.
3. When a potential collision is detected, the system issues a warning to the driver.

### Implementation details

```python
def is_potential_fcw(self, model_prob: float):
    return model_prob > .9
```

### User awareness

- FCW is designed as a warning system and does not automatically apply brakes.
- The system's effectiveness may be reduced in low visibility conditions.
- Maintain a safe following distance and stay alert at all times.

## Adaptive Cruise Control (ACC)

Adaptive Cruise Control automatically adjusts the vehicle's speed to maintain a safe distance from the vehicle ahead.

### How it works

1. The `RadarD` class processes radar and vision data to detect and track leading vehicles.
2. The `LongitudinalPlanner` in `selfdrive/controls/lib/longitudinal_planner.py` uses this information to plan speed adjustments.
3. The `LongControl` class in `selfdrive/controls/lib/longcontrol.py` executes the planned speed adjustments.

### Implementation details

```python
lead_dict = get_lead(self.v_ego, self.ready, self.tracks, leads_v3[0], model_v_ego, low_speed_override=True)
```

### User awareness

- ACC does not respond to stationary objects or oncoming vehicles.
- The system may not detect all vehicles in all conditions.
- Drivers must remain ready to take control at any time.

## Automated Lane Centering

This feature helps keep the vehicle centered in its lane.

### How it works

1. The vision system (`modelV2`) provides lane positioning data.
2. The `LatControl` classes (PID, Angle, or Torque) in `selfdrive/controls/lib/latcontrol_*.py` calculate steering commands.
3. The `Controls` class in `selfdrive/controls/controlsd.py` integrates these commands with other systems.

### Implementation details

```python
actuators.curvature = self.desired_curvature
steer, steeringAngleDeg, lac_log = self.LaC.update(CC.latActive, CS, self.VM, lp,
                                                   self.steer_limited_by_controls, self.desired_curvature,
                                                   self.calibrated_pose, curvature_limited)
```

### User awareness

- The system requires clear lane markings for optimal performance.
- Drivers must keep their hands on the steering wheel and remain attentive.
- Weather conditions and road quality can affect system performance.

## Driver Monitoring

The Driver Monitoring system ensures that the driver remains attentive while using openpilot.

### How it works

1. A camera monitors the driver's face and eyes.
2. The system detects signs of distraction or drowsiness.
3. If the driver is not paying attention, the system issues warnings and may disengage automated features.

### Implementation details

```python
cs.forceDecel = bool((self.sm['driverMonitoringState'].awarenessStatus < 0.) or
                     (self.sm['selfdriveState'].state == State.softDisabling))
```

### User awareness

- Always remain attentive and ready to take control, even when using automated features.
- The system may not detect all instances of distraction or drowsiness.
- Regularly take breaks during long drives to maintain alertness.

Remember, while these safety features are designed to assist the driver, they do not replace the need for an attentive and responsible driver. Always follow traffic laws and prioritize safety when using openpilot.