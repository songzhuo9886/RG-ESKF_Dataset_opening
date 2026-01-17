# Public Dataset (Vision–IMU Time Series)


This repository provides time-synchronized vision-based relative attitude measurements and camera-platform IMU recordings collected from our semi-physical hardware-in-the-loop setup, together with the corresponding ground-truth target angular velocity and angular-momentum (H-axis) direction for each test scenario described in the manuscript.


## 1. Directory Structure

Each **top-level folder** corresponds to one **test scenario** (working condition):

```text
dataset_root/
├─ w0.3/        # nominal low-rate case
├─ w3/          # nominal medium-rate case
├─ w15/         # nominal high-rate case
├─ w_jump/      # observation jump disturbance case
├─ w_loss/      # loss-of-observation case
└─ README.md
````

Inside **each scenario folder**, the file layout is:

```text
<scenario>/
├─ Cb2c
├─ wib_Qb2i
├─ w_gt
└─ H_gt
```

wib_Qb2i is currently being prepared, and will be updated in future releases.

## 2. Frames and Notation

We use the following coordinate frames:

* (\Sigma_i): inertial frame
* (\Sigma_b): chaser/camera platform body frame (the camera frame is assumed aligned with (\Sigma_b))
* (\Sigma_c): target **centroid** frame (vision-labeling frame)
* (\Sigma_t): target **principal-axes body** frame (center-of-mass / principal inertia axes)

Rotation notation:

* (\mathbf{C}_{b}^{c}(k)): rotation from (\Sigma_b \rightarrow \Sigma_c)

Angular-rate notation:

* (\boldsymbol{\omega}_{ib}^{b}(k)): angular velocity of (\Sigma_b) w.r.t. (\Sigma_i), expressed in (\Sigma_b)
* (\boldsymbol{\omega}_{it}^{t}(k)): angular velocity of (\Sigma_t) w.r.t. (\Sigma_i), expressed in (\Sigma_t)



## 3. File Definitions (Per Scenario)

### 3.1 `Cb2c` — Vision attitude measurements

* **Content:** vision-based relative attitude measurement
  [
  \mathbf{C}_{b}^{c}(k)
  ]
  i.e., rotation from (\Sigma_b) to (\Sigma_c).
* **Usage:** visual observation input for downstream state estimation / fusion.

### 3.2 `wib_Qb2i` — IMU measurements and platform attitude

This file contains inertial information recorded on the camera platform:

* **`wib`**: IMU gyroscope measurement
  [
  \boldsymbol{\omega}_{ib}^{b}(k)
  ]
* **`Qb2i`**: platform attitude quaternion describing the body-to-inertial orientation (equivalent to (\mathbf{C}_{b}^{i}) in rotation-matrix form).
  **Note:** quaternion convention/order (e.g., Hamilton vs. JPL; ([w,x,y,z]) vs. ([x,y,z,w])) should be interpreted consistently with your implementation.

### 3.3 `w_gt` — Ground-truth target angular velocity

* **Content:** ground-truth angular velocity of the target
  [
  \boldsymbol{\omega}_{it}^{t}(k)
  ]
* **Usage:** quantitative evaluation of angular-velocity estimation errors.

### 3.4 `H_gt` — Ground-truth angular momentum axis (“H-axis”)

* **Content:** ground-truth direction of the target angular-momentum axis (often unit-normalized).
* **Usage:** evaluation of H-axis direction error (angular misalignment between estimated and ground-truth directions).

---

## 4. Scenario Meanings (Top-Level Folders)

* `w0.3`, `w3`, `w15`: nominal **low / medium / high** target angular-rate cases (scenario naming follows the approximate rate level).
* `w_jump`: **jump disturbance** case, where the vision attitude measurements contain abrupt changes.
* `w_loss`: **loss-of-observation** case, where the vision stream experiences observation loss / dropout behavior.

---

## 5. Time Alignment and Sampling

* Data within each scenario is **time-aligned by frame index** (k).
* The sequences are typically generated at a fixed sampling interval (commonly (\Delta t = 0.2~\mathrm{s}), unless otherwise specified by the experiment setting).


