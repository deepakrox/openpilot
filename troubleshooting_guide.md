# Troubleshooting Guide

This guide provides solutions for common issues you might encounter while using or developing with openpilot. Follow the steps below to diagnose and resolve problems.

## Table of Contents

1. [Model Performance Issues](#model-performance-issues)
2. [Camera and Sensor Problems](#camera-and-sensor-problems)
3. [Vehicle Control Issues](#vehicle-control-issues)
4. [System Startup Problems](#system-startup-problems)
5. [Networking and Connectivity Issues](#networking-and-connectivity-issues)

## Model Performance Issues

If you're experiencing issues with model performance, such as poor lane detection or unexpected behavior, try the following:

1. Check model inputs:
   - Ensure `modelV2` messages are being received properly.
   - Verify that camera inputs (`buf_main` and `buf_extra`) are correctly formatted.

2. Inspect model outputs:
   - Enable raw prediction output by setting the `SEND_RAW_PRED` environment variable.
   - Analyze the `raw_pred` field in the model outputs for unexpected values.

3. Verify model loading:
   - Check if the vision and policy models are loaded correctly from their respective `.pkl` files.
   - Ensure the model metadata files are present and contain correct information.

Example of enabling raw prediction output:

```python
import os
os.environ['SEND_RAW_PRED'] = '1'
```

## Camera and Sensor Problems

For issues related to cameras or other sensors:

1. Verify camera connections:
   - Check if `vipc_client_main` and `vipc_client_extra` (if applicable) are connected.
   - Ensure the correct camera streams are being used (main and wide).

2. Calibrate cameras:
   - Run the camera calibration process if you suspect misalignment.
   - Check `liveCalibration` messages for calibration status.

3. Inspect sensor data:
   - Monitor `wideRoadCameraState` for exposure and light sensor information.
   - Verify `pandaStates` messages for ignition and other vehicle states.

## Vehicle Control Issues

If the vehicle is not controlling properly:

1. Check control status:
   - Verify `controlsState` messages for the current control state.
   - Ensure `carControl` messages are being sent and received.

2. Inspect vehicle parameters:
   - Confirm that the correct `CarParams` are loaded for your vehicle.
   - Check `lateralControlParams` and other vehicle-specific parameters.

3. Analyze control outputs:
   - Examine `actuators` fields in the `carControl` messages.
   - Verify that `steer`, `accel`, and other control commands are within expected ranges.

Example of inspecting control outputs:

```python
def inspect_control_outputs(CC):
    print(f"Steering command: {CC.actuators.steer}")
    print(f"Acceleration command: {CC.actuators.accel}")
    print(f"Desired curvature: {CC.actuators.curvature}")
```

## System Startup Problems

If the system fails to start or crashes during initialization:

1. Check process initialization:
   - Verify that all required processes (e.g., `modeld`, `controlsd`) are starting correctly.
   - Look for error messages in the system logs.

2. Inspect hardware status:
   - Check `deviceState` messages for any hardware issues.
   - Verify that all required sensors and cameras are detected.

3. Review configuration:
   - Ensure all necessary configuration files are present and correctly formatted.
   - Check `Params` for any misconfigured settings.

## Networking and Connectivity Issues

For problems related to networking or connectivity:

1. Verify network status:
   - Check the device's internet connection.
   - Ensure all required services are reachable.

2. Inspect messaging system:
   - Verify that `SubMaster` and `PubMaster` are initialized with the correct topics.
   - Check for any message publication or subscription errors.

3. Analyze data flow:
   - Monitor message frequencies and ensure all required messages are being received.
   - Look for any gaps or delays in message reception.

Example of monitoring message frequencies:

```python
def monitor_message_frequencies(sm):
    for socket, last_updated in sm.updated.items():
        if last_updated:
            print(f"{socket}: {sm.logMonoTime[socket] * 1e-9:.2f} seconds ago")
```

By following this troubleshooting guide, you should be able to diagnose and resolve most common issues encountered while working with openpilot. If you continue to experience problems after trying these steps, please reach out to the community forums or file an issue on the project's GitHub repository.