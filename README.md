# kalman-filter-simulink
1D Kalman Filter for Roll Estimation in Simulink


 1D Kalman Filter for Roll Angle Estimation in Simulink

This project implements a 1D Kalman Filter for estimating the **roll angle** using **gyroscope** and **accelerometer** data. The entire system is modeled in **Simulink using only block-level components** — no MATLAB Function blocks are used.

It’s designed to be both educational and technically accurate, showcasing how real-time sensor fusion can be done in a clean, modular way.

---

 Overview

The Kalman Filter fuses:
- Angular velocity from the **gyroscope**
- Tilt estimate from the **accelerometer**

to produce a smooth and accurate roll angle estimate, even in the presence of noisy measurements and gyro drift.

---

 System Structure

The project is divided into **three major subsystems**:

 1. `True Motion Generator`
- Simulates a known ground-truth roll angle using a sine wave.
- Provides reference for comparison.

 2. `IMU Sensor Model`
- Simulates a noisy gyroscope using the derivative of the roll angle.
- Simulates accelerometer outputs (accY, accZ) using gravity projection based on roll.
- Adds Gaussian noise to both gyro and accel.
- Computes an accelerometer-based roll angle using the `atan2(accY, accZ)` method.

 3. `Kalman Filter`
- Takes in `gyro_measured`, `accY_noisy`, and `accZ_noisy`.
- Computes the accel-based measurement `z` using `atan2`.
- Implements prediction and update steps using:
  - Kalman Gain `K = P / (P + R)`
  - State estimate update: `x = x_pred + K(z - x_pred)`
  - Covariance update: `P = (1 - K) * P_pred`

All blocks are standard Simulink components (`Sum`, `Gain`, `Unit Delay`, etc.)

---

 Results

The system is visualized using a multi-input Scope that compares:

- True roll (ground truth)
- Gyro-only integration (shows drift over time)
- Accelerometer-only angle (noisy but drift-free)
- Final Kalman estimate (smooth, accurate)

The Kalman estimate closely follows the true roll, correcting gyro drift using the accelerometer's long-term stability, while filtering out accelerometer noise using gyro data.

---

 Files Included

- `kalman_filter_1d.slx` – Main Simulink project file
- `model_structure.png` – Screenshot of block diagram and subsystems
- `scope_output.png` – Example scope output showing signal comparison

---

 Notes

- The simulation is built for a fixed-step system (e.g., 0.01s)
- Real IMU data can be imported via `From Workspace` blocks using resampled time series.
- Project is organized with modular subsystems to maintain clarity and scalability.

---

 Future Directions

This project can be extended to:
- Use real-time IMU data from a phone or microcontroller
- Expand to 2D or 3D orientation using Extended Kalman Filter (EKF)
- Integrate with a control system (e.g., inverted pendulum, drone, robot)
