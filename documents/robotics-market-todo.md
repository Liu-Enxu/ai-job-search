# TODO: Filling the Gap to the Robotics Market

**Created:** 2026-07-24
**Purpose:** One-page action list to become a competitive robotics-engineer candidate.
Consolidates the three source docs — keep those for detail:
- Skill gaps + resources: `upskill/report-2026-07-24-robotics-engineer-role-family.md`
- Real-project work plan: `documents/in-flight-robotics-slam-plan.md`
- Search strategy + live leads: `job_scraper/seen_jobs.json`

**The one-line strategy:** your embedded/firmware/motor-control/estimation depth already fits a
*class* of robotics roles TODAY (the intersection titles). Apply to those now. In parallel, close
the ROS2 + SLAM gaps on real Motion Sync work so the broader "Robotics Software Engineer" pool
opens up in 1–3 months. Do NOT wait for the study plan to finish before applying.

---

## Track 1 — Build real experience (highest leverage)

These land in Professional Experience because they're employed work. In-flight → describe as
current work, never as a delivered result, never on a CV until the milestone runs.

- [ ] **ROS2 track (RPi5 seat/DAQ):** migrate the hand-rolled GNSS/IMU EKF to the standard
      `robot_localization` package (`ekf_node`, fusing GNSS + IMU + visual odometry). Closes the
      #1 hiring gap (ROS2 ecosystem) with a low-risk refactor of working code.
- [ ] **SLAM track (Orange Pi / openpilot prediction unit):** add VIO into the `locationd`
      localizer + a rolling local map for lane/road-geometry lookahead. Closes the SLAM/perception
      gap and directly improves the prediction unit (anticipatory > reactive).
      - [ ] First prove the compute budget: Mali-G610 is already at ~10 Hz on supercombo; run VO
            on CPU/NPU or accept lower rate before promising any throughput/accuracy number.
- [ ] **Portfolio proof (if the above slip):** one buildable demo — simulated mobile robot in
      Gazebo that maps a room with `slam_toolbox` and navigates with Nav2. Answers "have you used
      ROS2 in a real system?" even before the work projects land.

## Track 2 — Study plan (from the upskill report)

Do it as the one buildable project above, not as isolated courses. Order matters (dependencies).

- [ ] 1. **URDF/xacro + tf2** — ~10h — foundation for everything
- [ ] 2. **Gazebo simulation** — ~15h — do alongside #1 (transfers from your MuJoCo)
- [ ] 3. **ros2_control** — ~12h — closest to your firmware strength, fast win
- [ ] 4. **Nav2** — ~18h — the headline ROS2 skill on JDs
- [ ] 5. **SLAM (slam_toolbox)** — ~30h — your EKF/particle-filter background makes this faster
- [ ] 6. **MoveIt/OMPL + manipulator kinematics** — ~35h — CONDITIONAL, only if targeting arms/humanoids

**Core competency (steps 1–5): ~85h.** Skip step 6 unless a manipulation role is live.
Resources + "what to skip" notes: see the upskill report.

## Track 3 — Search & apply (do now, in parallel)

- [ ] **Search the intersection titles, not "robotics":** Robotics Firmware Engineer, Embedded
      Robotics Engineer, Motor Control Engineer, Sensor Fusion Engineer, Controls Engineer
      (robotics). These reward what you have today.
- [ ] **Expect an ITAR minefield.** Pure-robotics search is dense with defense/export-controlled
      employers (auto-skip): MORSE, Firestorm, Anduril, Shield AI, Overland AI, Applied
      Intuition, Airbus S&D, etc. Filter early; don't spend drafting time on them.
- [ ] **Ignore "Controls Engineer" industrial-PLC results** — automation/SCADA roles are not your
      embedded/firmware profile (a common false positive of that title).

### Live leads from the 2026-07-24 scrape (act on these)

- [ ] **Metamorphic** (Palo Alto) — TOP. Explicit visa sponsorship; a min-qual mirrors your DAQ
      time-sync work. → evaluate + draft.
- [ ] **SERES / SF Motors** (Milpitas) — exact firmware stack for robotics actuators. → send a
      two-question sponsorship check FIRST (JD is silent), then draft only if confirmed.
- [ ] Weave Robotics, Summer Robotics, Path Robotics — medium, sponsorship silent → verify auth
      before drafting.
- [ ] Detail-fetch batch: VenHub, Home Depot, Qualcomm Sensors, Aptiv (Troy MI, low relo),
      Treeswift (verify drone export).

---

## If you only do three things

1. **Apply to Metamorphic** this week (explicit sponsor + near-perfect req match).
2. **Start the `robot_localization` migration** on the ROS2 project — biggest gap, lowest risk.
3. **Keep searching intersection titles**, not the word "robotics".
