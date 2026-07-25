# In-Flight Work: SLAM / Localization on the Motion Sync Projects

**Status:** IN-FLIGHT — being actively developed, not yet delivered.
**Created:** 2026-07-24
**Purpose:** Records robotics work Enxu is adding to real Motion Sync projects to close the
SLAM/localization and ROS2-ecosystem gaps identified in
`upskill/report-2026-07-24-robotics-engineer-role-family.md`. Because this is real employed work
(not coursework or a personal project), it becomes claimable in **Professional Experience** once a
milestone lands — but NOT before. See the honesty boundary at the bottom.

---

## Two separate tracks, two different gaps, two different boards

The two Motion Sync platforms close different gaps. Be deliberate about which one you spend time on.

### Track A — SLAM / VIO on the openpilot prediction unit (Orange Pi 5)

**Hardware:** Orange Pi 5, RK3588S, Mali-G610 GPU. This is the **prediction hardware** — runs the
openpilot supercombo driving model at ~10 Hz; the vehicle-CAN ego-state dependency was already
replaced with a GNSS/IMU EKF.

**What to add:**
- **VIO into the localizer** — fuse the existing camera (visual odometry) into openpilot's
  Kalman localizer (`locationd`) alongside the GNSS/IMU EKF. Gives ego-motion that survives GNSS
  dropout and tightens the state estimate feeding the driving model. *(This is the localization half.)*
- **Local map / lane-geometry lookahead** — a rolling local map gives the prediction model a
  geometric road-structure prior where the neural output is uncertain. **This is the piece that
  most directly improves the prediction unit** — anticipatory beats reactive. *(This is the mapping half.)*

**Gap it closes:** perception / SLAM / sensor-fusion / edge-AI.
**Gap it does NOT close:** ROS2 ecosystem — openpilot runs its own `cereal`/`msgq` messaging and
tinygrad on the GPU, not ROS2.

**Right-sizing:** VIO + local map, together, is honestly "SLAM." VIO alone → call it
visual-inertial odometry, not SLAM. Full global SLAM with loop closure is overkill for a moving
vehicle and not the goal.

**Compute constraint (do not promise a result before proving this):** the Mali-G610 is already
loaded running supercombo at ~10 Hz. A visual-odometry frontend competes for that same GPU —
plan to run the VO on CPU or the RK3588S NPU, or accept a lower rate. Budget must be proven on
hardware before any throughput/accuracy claim.

### Track B — `robot_localization` migration on the ROS2 seat/DAQ unit (Raspberry Pi 5)

**Hardware:** Raspberry Pi 5, custom Yocto image, ROS2 DAQ system, time-synced IMU/video/GNSS,
core-pinned for real-time jitter isolation.

**What to add:** migrate the hand-rolled GNSS/IMU EKF to the standard **`robot_localization`**
package (`ekf_node`, fusing GNSS + IMU + visual odometry). A refactor of something that already
works — low risk, high resume value.

**Gap it closes:** ROS2 production ecosystem — the single biggest robotics-hiring gap. Makes the
estimator ROS2-native and gives a real-work `robot_localization`/`tf2` story.

---

## Honesty boundary (per CLAUDE.md guardrails — do not relax)

- **In-flight → current work only.** Describe as "integrating visual-inertial localization and
  local mapping into the vehicle prediction system," never as a delivered result.
- **No target results stated as achieved** (e.g. "improved prediction accuracy by X%") until
  measured on hardware.
- **Not on a CV until it actually runs.** Once a milestone lands, it is fully claimable in
  Professional Experience (real employed work at Motion Sync).
- **Label precisely.** "SLAM" only if mapping + localization ship together; otherwise "VIO".
