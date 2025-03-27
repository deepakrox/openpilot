# Data Privacy Policy

## Introduction

This document outlines the data collection and privacy practices of the openpilot system. It explains what data is collected, how it's used, and the options available to users regarding data sharing.

## Data Collection

The openpilot system collects various types of data to improve its functionality, safety features, and overall performance. This includes:

1. Vehicle Data
   - Speed
   - Acceleration
   - Steering angle
   - Pedal positions
   - Other CAN bus data

2. Sensor Data
   - Camera feeds
   - GPS location
   - IMU (Inertial Measurement Unit) data

3. User Interactions
   - System engagement/disengagement
   - User inputs and preferences

4. Performance Metrics
   - Model predictions
   - Control outputs
   - System diagnostics

## Data Usage

The collected data is used for the following purposes:

1. Improving the openpilot system
   - Enhancing driving models and algorithms
   - Fixing bugs and addressing issues
   - Developing new features

2. Safety analysis and improvements
   - Identifying potential safety risks
   - Developing and refining safety features

3. Research and development
   - Advancing autonomous driving technology
   - Contributing to academic and industry research

4. Compliance with legal requirements
   - Responding to law enforcement requests
   - Meeting regulatory obligations

## Data Storage and Transmission

Data collected by the openpilot system is stored locally on the device and may be transmitted to secure servers for analysis and processing. The transmission of data is done using encrypted channels to ensure privacy and security.

## User Options for Data Sharing

Users have control over their data sharing preferences:

1. Opt-out: Users can choose to opt-out of data sharing by modifying the appropriate settings in the openpilot system.

2. Data access: Users can request access to their personal data collected by the system.

3. Data deletion: Users can request the deletion of their personal data, subject to legal and regulatory requirements.

## Third-Party Sharing

openpilot may share anonymized and aggregated data with third parties for research, development, or other purposes that align with improving autonomous driving technology. Personal identifiable information is not shared without explicit user consent, except when required by law.

## Changes to Privacy Policy

This privacy policy may be updated from time to time. Users will be notified of any significant changes to the data collection and usage practices.

## Contact Information

For any questions or concerns regarding data privacy, please contact:

[Insert contact information here]

## Code Example: Data Collection in modeld

The following code snippet from `selfdrive/modeld/modeld.py` demonstrates how some data is collected and processed within the openpilot system:

```python
def run(self, buf: VisionBuf, wbuf: VisionBuf, transform: np.ndarray, transform_wide: np.ndarray,
              inputs: dict[str, np.ndarray], prepare_only: bool) -> dict[str, np.ndarray] | None:
    # Model decides when action is completed, so desire input is just a pulse triggered on rising edge
    inputs['desire'][0] = 0
    new_desire = np.where(inputs['desire'] - self.prev_desire > .99, inputs['desire'], 0)
    self.prev_desire[:] = inputs['desire']

    self.full_desire[0,:-1] = self.full_desire[0,1:]
    self.full_desire[0,-1] = new_desire
    self.numpy_inputs['desire'][:] = self.full_desire.reshape((1,ModelConstants.INPUT_HISTORY_BUFFER_LEN,ModelConstants.TEMPORAL_SKIP,-1)).max(axis=2)

    self.numpy_inputs['traffic_convention'][:] = inputs['traffic_convention']
    self.numpy_inputs['lateral_control_params'][:] = inputs['lateral_control_params']
    imgs_cl = {'input_imgs': self.frames['input_imgs'].prepare(buf, transform.flatten()),
               'big_input_imgs': self.frames['big_input_imgs'].prepare(wbuf, transform_wide.flatten())}

    # ... (rest of the method)
```

This code shows how various inputs, including desire, traffic convention, and lateral control parameters, are collected and processed before being fed into the model. It also demonstrates how image data is prepared for analysis.