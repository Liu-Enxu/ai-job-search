---
framework_version: 1.0.0
---

# Candidate Profile

## Identity
- **Name:** Enxu Liu
- **Location:** Ann Arbor, Michigan, USA
- **Phone:** (734) 489-3536
- **Email:** enxu@umich.edu
- **GitHub:** https://github.com/Liu-Enxu
- **LinkedIn:** _TODO - not present in source documents_
- **Languages:** _TODO - not stated in source documents_
- **Status:** Employed, Mechatronics Engineer at Motion Sync (Apr 2024 - present)
- **Constraints:** _TODO - commute/relocation preferences not yet captured_

## Education

| Degree | Period | Institution | Key Topics |
|--------|--------|-------------|------------|
| MSc, Electrical & Computer Engineering (GPA 3.97) | Aug 2023 - Dec 2024 | University of Michigan, Ann Arbor | Embedded systems, controls, robotics |
| BEng, Electronic & Electrical Engineering, First-Class Honours (GPA 4.00) | Sep 2020 - Jun 2023 | University College London | Electronics, signals, control |

Note: employment at Motion Sync (from Apr 2024) overlaps the final two terms of the MSc.

## Professional Experience

### Mechatronics Engineer - Motion Sync (spinoff startup) (Apr 2024 - present)
Ann Arbor, MI

**Haptic Pillow - consumer product, owned firmware and hardware end to end**
- Led FreeRTOS firmware design for ESP32 (ESP-IDF SDK) with fully decoupled peripheral drivers; architecture proved migratable across hardware; built with CMake
- Object-oriented BSP: BMI160 IMU (I2C), W25Q NOR flash (SPI), vibration motor (PWM) at millisecond accuracy via FreeRTOS timers
- **BLE and OTA architecture:** iOS app communication with slave-latency and throughput optimization; delta OTA with an **A/B dual-bank boot partition**, fail-safe rollback **and anti-rollback (downgrade) protection**; patch size reduced to 1.2 KB, update completing under 10 s
- **Secure firmware (earned; expanded from the candidate's own summary 2026-07-23):**
  - **eFuse hardware root of trust** anchoring the chain in silicon
  - **RSA-signed firmware images** (applied cryptography for image signing and verification)
  - **Verified boot chain with a secure bootloader**, so only authenticated images execute
  - **Anti-rollback protection** preventing downgrade to an older, vulnerable image
  - **BLE application-level handshake** for authenticated app-to-device communication
  - **Resolvable Private Address (RPA)** for BLE privacy, preventing device tracking
  - **Secure debug lockout** closing the debug port on production units
- **Shipped 200 units.**

<!-- CLAIM BOUNDARY - shipping scale (settled 2026-07-23 at the candidate's direction):
     Write ONLY "shipped 200 units". Nothing more.
     The units were in fact FCC certified under a pre-certification containment plan, and the
     product did go to sale - but the candidate does not want FCC regulation on the page,
     because he is not confident defending regulatory detail in an interview and does not want
     to invite the question. This is a deliberate choice to keep the CV to ground he can hold.
     Do NOT re-add "FCC certification", "pre-cert containment", or certification process detail
     to this bullet, even though it is all true and would read impressively.
     Do not inflate 200 into a larger production volume. -->

<!-- CLAIM BOUNDARY - secure firmware (set 2026-07-23, expanded same day):
     EARNED and claimable: eFuse hardware root of trust; RSA-signed firmware images; verified boot
     chain with secure bootloader; anti-rollback/downgrade protection; BLE application-level
     handshake; Resolvable Private Address (RPA) for BLE privacy; secure debug lockout;
     A/B dual-bank boot partition with fail-safe rollback.
     This is now a STRONG match area, not a growth area. "Firmware security" and "secure boot"
     may be used as competency headings.
     STILL NOT EARNED - never claim: TPM 2.0, HSM, PKI/certificate authority operations or
     fleet-scale key-management infrastructure, LUKS/full-disk encryption, SELinux/AppArmor
     mandatory access control, secure elements as separate hardware, formal threat modelling
     or security certification (Common Criteria, FIPS).
     Distinction that matters: he has built a device-level secure boot chain end to end. He has
     not run the organisational key-management or platform-hardening tier around it. -->

- Battery management: single-cell Li-ion monitoring (ADC), power-path/MOSFET control, battery-sag debugging; low-power configuration cutting BLE TX power by ~1.94 mW
- PCB design and bring-up: integrated ESP32-C3, IMU, DRV8833, TP4056 from datasheet and schematic; LTspice circuit simulation; root-caused a power-rail bug with oscilloscope and logic analyzer; JTAG/SWD debug on J-Link; bash automation for OTA regression testing
- Project management: Agile/Scrum, shipped end-to-end to sale, FCC compliance, DFM/DFA (30% cost reduction), Jira, Git/GitHub CI/CD

**Motion-sickness research to product (the underlying programme, from the industry portfolio)**
- The DAQ and active-seat work sits inside a motion-sickness reduction programme: an active seat (AISO) evaluated against a passive baseline (PISO) on a test vehicle.
- **Result: ~19-20% reduction in motion-sickness accumulation rate across N=27 subjects**, supported by a linear mixed-model fit (MS-score slope difference p = 1.8e-5) and corroborated by EDA physiological data showing reduced mean and variance.

<!-- USAGE RULE - research outcome (set by the candidate 2026-07-23):
     DEFAULT: leave the motion-sickness study result OFF the CV. It is a research finding, and
     on a firmware or embedded-systems CV it reads as off-target and costs space that should go
     to engineering evidence.
     INCLUDE IT ONLY when the posting mentions research, R&D, a university spinoff, clinical or
     human-subjects work, or otherwise signals that a measured study outcome is relevant.
     When included, it is strong: a controlled result with statistical support. When not, cut it. -->
- Productized into both the consumer Haptic Pillow and the Tier 1 automotive OEM seat programme. Product/system name: **PREACT**.

**ROS2 Data Acquisition System (DAQ) - embedded Linux, seat actuation and control**
- Delivered for a **local Tier 1 automotive OEM**, deploying the active seat onto an OEM seat product and vehicle platform, with tunable timing and tunable roll/pitch motion profiles.
- Tapped the **OEM vehicle CAN bus** for additional vehicle sensing, and upgraded triggering to run on **computer-vision-based prediction** rather than reactive input.
- Ran an **IAEKF (Innovation-Adaptive-Estimation Kalman Filter) plus a custom prediction neural network** on the Raspberry Pi 5 platform; PID controllers for seat motion driven by an FOC motor controller.
- Integrated **Empatica Embrace Plus** wearables for subject physiological data collection.
- Architected a ROS2 DAQ on Raspberry Pi 5 with a custom Yocto (scarthgap) image, gcc cross-compiled and Bitbake-flashed; one sensor per node, core-pinned for real-time jitter isolation; DAQ GUI in Qt (C++)
- Authored a bbappend adding `KERNEL_MODULE_AUTOLOAD:rpi += "i2c-dev"`; validated I2C/UART/BLE/TCP on hardware
- Custom IPC via Unix-domain sockets and multiprocessing across the node graph
- Time-synchronized IMU (I2C), video (ffmpeg/V4L2), dual GNSS (on-board u-blox plus TCP-networked), and BLE into ROS2 topics with ns-precision timestamps and auto-reconnect
- **BLDC motor control with field-oriented control (FOC) on real hardware, commanded over CAN-FD.** Both system-level integration (CAN-FD protocol, setpoint command path, control loop at the system level) **and** hands-on implementation of portions of the FOC control loop itself. This is hardware-validated motor control, not simulation - distinguish it from the MATLAB/Simulink coursework projects.

<!-- CLAIM BOUNDARY (confirmed by candidate 2026-07-19): he integrated FOC over CAN-FD AND
     wrote parts of the control loop. He did NOT author a complete FOC implementation from
     scratch. Accurate phrasing: "implemented portions of a FOC control loop and integrated
     BLDC-FOC motor control over CAN-FD on real hardware." Do not escalate this to "built a
     FOC controller" or "implemented field-oriented control" unqualified. -->

- Deployed a Kalman filter for lane-keep-assist ADAS (adaptive gain over a time window) and a PID controller for seat actuation
- Data analysis and processing in Python; datasets managed on AWS; produced technical documentation

<!-- FRAMING RESOLVED (confirmed by candidate): the work spans research through OEM
     delivery, but is primarily automotive OEM seat upgrade and actuation control. The
     automotive-OEM framing used in the source CVs is accurate and may be used. When the
     research dimension is more relevant to a posting, describe it as spanning both rather
     than substituting one framing for the other. -->

**Automotive vision prediction - openpilot port to Orange Pi 5 (RK3588S)**
- Ported sunnypilot/openpilot to Orange Pi 5 (RK3588S) on Ubuntu-rockchip; built with scons, resolving Git-LFS model-weight and Mali OpenCL toolchain issues
- Ran the supercombo driving model in fp32 on the Mali-G610 GPU (OpenCL/tinygrad) at ~10 Hz for live path, lane, and lead-vehicle prediction on edge hardware
- Brought up USB camera capture (V4L2, MJPG) into a VisionIPC shared-memory video pipeline feeding the model and a Qt UI overlay
- Camera intrinsic calibration with OpenCV, integrated into model and UI transforms for correct path geometry
- Replaced the vehicle-CAN dependency by fusing GNSS (NEO-M9N) and IMU (BMI160) in an EKF (locationd) with a Kalman GPS/IMU speed estimate into vehicle state. The filter is an Innovation-Adaptive-Estimation Kalman Filter (IAE-KF) with IGGIII robust weighting, loosely coupling IMU and GNSS for robustness against measurement noise and GNSS dropout, measurably more robust than a vanilla KF

## Independent Projects

- **RV1126 Edge Camera - MIPI-CSI camera and embedded Linux BSP** (Jun 2026 - present): Brought up the ALIENTEK RV1126 (Rockchip) board (UART console, U-Boot to Linux 4.19 kernel to shell); built the full Buildroot vendor SDK image and flashed via rkdeveloptool. MIPI-CSI IMX335 camera bring-up through the rkcif/rkisp/rkispp ISP pipeline, capturing H.264 2K video with live LCD preview. Modified the vendor SDK device tree (GPIO/LED node), rebuilt kernel/DTB, verified in `/sys/class/leds` and `/proc/device-tree`. Loaded the NPU (galcore) for on-device inference; V4L2 capture plus RKNN YOLO detection overlay in progress.
- **Embedded Linux bring-up - Lichee Pi Zero (V3s)** (Jun 2026), personal: Cross-compiled mainline U-Boot (SPL to DDR2 init) and the Linux kernel from source; flashed SPL to the sunxi 8 KB offset; booted to a shell over UART. Headless bring-up: serial console, SSH over USB-gadget/Ethernet, expanded ext4 rootfs, deployed zImage and DTB. Drove a GPIO LED with a custom kernel module (gpiolib plus kernel timer, module params), insmod/rmmod verified in dmesg. Resolved a device-tree pin-mux conflict (disabled SDIO mmc1 to reclaim LED GPIOs); enabled DWMAC_SUN8I Ethernet; configured DDR2/LCD via Kconfig.
- **Secure firmware design** (in progress as of 2026-07-21), personal: A secure-firmware design effort targeting the candidate's stated growth area. **NOT YET FINISHED - do not present as delivered, and do not list on a CV until a concrete milestone lands.** It may be described as current work when a posting centres on firmware security. Once complete, promote it here with specifics (what was implemented, on what hardware, verified how) and re-run `/rank --all`, since firmware security currently scores as a weak area and several strong postings gate on it (e.g. Apptronik Senior Embedded Linux at career-alignment 85, Anthropic OpenBMC). Ask the candidate for details before using it in any application.

- **STM32 Linux-inspired handheld system** (Jan 2026 - present), personal: HAL firmware on STM32F446RE (ARM Cortex-M4) integrating FreeRTOS, LVGL, and FatFS through a single ucHeap; gcc and Keil toolchains; ST-Link and gdb debug. Custom `pvPortRealloc` over the FreeRTOS heap_4 allocator, booting the full stack within 128 KB SRAM / 512 KB flash. Bare-metal BSP and LVGL object-oriented GUI on ILI9486 LCD, ADC+DMA touch sampling, parallel 8080 interface.
- **Motor embedded motion control** (Jan 2024 - May 2024), coursework: PWM, ADC, and interrupt routines in C on NXP S32K144 for BLDC torque control. PID controller for torque and angle designed and simulated in MATLAB/Simulink with a Stateflow state machine for mode arbitration. Defined CAN-FD message IDs for steering-angle and torque setpoints; HIL-verified real-time control at <10% overshoot.
- **Self-driving cars - path planning and motion control** (Sep 2024 - Dec 2024), coursework: Reinforcement-learning motion planning (Markov Decision Process) with MPC motion control, simulated and verified in ROS Rviz.
- **MIMO feedback controller design** (Jan 2024 - May 2024), coursework: PID controllers via loop shaping for a simulated wafer-etching system in MATLAB/Simulink. MIMO feedback controller integrating LQR with integrators, LQE, and loop transfer recovery (LTR); robustness verified by margin analysis.

<!-- The three coursework projects are labeled as such deliberately. They are legitimate CV
     content but must never be drafted as industry experience. -->

## Technical Skills

### MCU / SoC platforms
- ESP32 / ESP32-C3 (ESP-IDF), STM32 (CubeMX, HAL), NXP S32K144, Raspberry Pi 4/5, Orange Pi 5 (RK3588S), Rockchip RV1126, Allwinner V3s, ARM Cortex-M

### Embedded Linux / BSP
- Yocto (scarthgap, bbappend authoring, BitBake), Buildroot, mainline U-Boot (SPL, DDR init), Linux kernel build and cross-compilation, custom kernel modules (gpiolib, kernel timers), device tree (authoring, pin-mux conflict resolution), V4L2, MIPI-CSI and Rockchip ISP (rkcif/rkisp/rkispp), rkdeveloptool, RKNN NPU inference

### RTOS & bare-metal
- FreeRTOS (tasks, timers, heap_4 internals, custom allocator work), bare-metal BSP design, LVGL, FatFS, bootloaders and OTA/delta update with rollback

### Buses & peripherals
- I2C, SPI, UART, CAN / CAN-FD, BLE, TCP/IP, USB gadget, ADC, DAC, PWM, DMA, parallel 8080

### Languages & tooling
- Embedded C/C++ (primary), Python, Qt (C++), bash; Git/GitHub, CMake, scons, GDB, Keil, VS Code; MISRA C, Jira, Scrum, CI/CD

### Controls & robotics
- ROS2 (node architecture, topics, custom IPC), ROS Rviz, PID, MPC, LQR/LQE/LTR, Kalman filtering and EKF (including IAE-KF with IGGIII robust weighting), GNSS/INS sensor fusion, BLDC-FOC motor control, MATLAB/Simulink/Stateflow, MuJoCo

### Hardware & lab
- PCB design (KiCAD), LTspice simulation, oscilloscope, logic analyzer, multimeter, JTAG/SWD (J-Link, ST-Link), board bring-up and root-cause analysis, DFM/DFA, FCC compliance

## Publications
None.

## Patents
- Co-inventor, three University of Michigan provisional patent applications, UM Ref. Nos. 2026-235-01, 2026-234-01, 2026-233-01 (2026). Titles (from the industry portfolio):
  - *Systems and Methods Implementing Control Logic Software Architecture for Preemptive Control of Active Devices and Systems Within a Vehicle*
  - *Systems and Methods for Preemptive Control of Active Notification Devices and Systems Within a Vehicle*
  - *Systems and Methods Implementing Perception and Prediction Multithreaded Architecture for Preemptive Control of Active Devices and Systems Within a Vehicle*

## Awards
_None recorded in source documents._

## References
_None on file._

More references available upon request.
