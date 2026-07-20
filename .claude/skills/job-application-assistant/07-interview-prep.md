---
framework_version: 1.0.0
---

# Interview Preparation Guide

## STAR Format

Structure answers as: **Situation** (context), **Task** (your responsibility), **Action**
(what you did), **Result** (outcome).

Keep spoken answers to ~90 seconds. The detail below is so you can go deep when an
interviewer drills in, not so you can recite it. End with what you learned or would do
differently.

**Source:** `documents/cv/STAR_stories.md`. Stories 1-7 are already-earned and usable today.
Stories 8-9 are forward-looking and carry usage restrictions - see that section.

Role-family tags: **FW** = firmware/MCU, **BSP** = embedded Linux/BSP.

## Ready-Made STAR Examples

### 1. Power-rail undervolt bring-up (FW - board bring-up and debug)
**S:** During Haptic Pillow bring-up, the ESP32 reset intermittently, only when the vibration
motor engaged.
**T:** Find the root cause before the board could ship. "Works on the bench" was not enough.
**A:** Probed the motor supply rail with an oscilloscope while triggering motor load via
J-Link; saw the rail sag below the brown-out threshold on current spikes. Correlated with
DRV8833 inrush. Diagnosed insufficient bulk capacitance plus a marginal supply trace.
**R:** Added reservoir caps and rerouted the trace; the rail held under load and resets were
eliminated. Shipped.
**Lesson:** Intermittent digital faults are often analog or power problems.
**Use for:** "Tell me about a hard bug", "Describe your debugging process", "A time you had to
ship under pressure"

### 2. Delta OTA over BLE (FW - BLE/OTA)
**S:** The product needed field firmware updates over BLE, but full-image OTA was slow and
data-heavy over a constrained link.
**T:** Make updates fast and small enough to be practical for end users.
**A:** Designed a delta-OTA pipeline: diff against the running image, transfer only the patch
over BLE telemetry, apply with rollback protection.
**R:** Patch size down to **1.2 KB**, upgrade completing in **under 10 s**. Built bash
automation to regression-test the OTA path in CI.
**Use for:** "Deep firmware/RTOS", "A time you optimized something", "How do you handle
field updates". The absolute numbers are the strength here - keep them.

### 3. pvPortRealloc on 128 KB SRAM (FW - RTOS heap internals)
**S:** On the STM32F446RE handheld, LVGL, FatFS, and FreeRTOS all needed dynamic memory, but
FreeRTOS heap_4 has no `realloc`, and the part has only 128 KB SRAM / 512 KB flash.
**T:** Run all three middlewares off a single managed heap (`ucHeap`) without fragmentation
blowing the budget.
**A:** Implemented a custom `pvPortRealloc` over the heap_4 allocator, handling adjacent
free-block merging and the split/coalesce edge cases the upstream API omits; routed LVGL's
allocator hooks through it.
**R:** Booted the full LVGL + FatFS + FreeRTOS stack within 128 KB.
**Use for:** "Deep firmware/RTOS", "Memory-constrained design", "Tell me about something you
built below the API level". Shows allocator-level understanding, not just API usage.

### 4. ADC resistive-touch ISR flag (FW - ISR safety and bring-up)
**S:** The ILI9486 resistive touch on the STM32 handheld returned stale or garbage coordinates.
**T:** Get reliable touch sampling from the ADC+DMA path.
**A:** Captured the ADC ISR timing on a logic analyzer; found the conversion-complete flag was
not auto-clearing on ISR entry, so reads raced the next conversion. Added
`__HAL_ADC_CLEAR_FLAG` at entry and re-synced the DMA trigger.
**R:** Stable touch input.
**Use for:** "Tell me about a hard bug", "Race conditions", "Interrupt handling". Demonstrates
ISR/race debugging with the right instrument.

### 5. ROS2 DAQ decomposition and Yocto (BSP - ROS2 + Yocto/BSP)
**S:** An automotive OEM seat upgrade and actuation program needed a DAQ collecting IMU (I2C),
video (ffmpeg), and GNSS on a Raspberry Pi 5 with a reproducible, deployable OS image.
**T:** Build it maintainable and reproducible, not a one-off SD card.
**A:** Decomposed the DAQ into ROS2 nodes (I2C/IMU, BLE, TCP, ffmpeg/video, GPS/serial) with
custom IPC via Unix-domain sockets and multiprocessing. Built a custom **Yocto scarthgap**
image; authored a bbappend adding `KERNEL_MODULE_AUTOLOAD:rpi += "i2c-dev"`; validated
I2C/UART/BLE/TCP on hardware.
**R:** Reproducible image plus a clean node graph; data streams packaged as ROS2 topics.
**Use for:** "Embedded Linux/BSP", "Build systems", "System architecture". Bridges embedded
Linux and robotics middleware.

> **Framing note:** the work spans research through OEM delivery but is primarily automotive
> OEM. Lead with the OEM framing; the research dimension is available as honest color if an
> interviewer probes how exploratory the work was.

### 6. CAN-FD ADAS controller on S32K144 (FW - CAN-FD and safety)
**S:** A course/HIL ADAS project required steering-angle and torque control over CAN-FD on an
NXP S32K144.
**T:** Deliver real-time, safe setpoint control with mode arbitration.
**A:** Defined CAN-FD message IDs for steering-angle and torque setpoints; implemented a
Stateflow state machine for mode arbitration; tuned the PID loop to <10% overshoot;
HIL-verified the control path.
**R:** Real-time control validated on CAN-FD.
**Use for:** "Systems/bus design", "Automotive protocols", "Real-time control". Message-ID
design plus arbitration is bus-level thinking, not just sending frames.
**Say it was coursework.** It is strong coursework, and volunteering that framing costs
nothing while being caught not volunteering it costs a lot.

### 7. GNSS/INS loose-coupling EKF (BSP - sensor fusion and state estimation)
**S:** In the openpilot port to Orange Pi 5, GNSS alone was too noisy for robust position
estimation and IMU alone drifts, but the vehicle-CAN dependency had to be removed.
**T:** Fuse them for robustness against noise and dropout.
**A:** Built an Innovation-Adaptive-Estimation Kalman Filter (IAE-KF) with IGGIII robust
weighting, loosely coupling IMU (BMI160) and GNSS (NEO-M9N) into locationd, feeding a Kalman
GPS/IMU speed estimate into vehicle state.
**R:** Increased robustness against measurement noise versus a vanilla KF; removed the
vehicle-CAN dependency.
**Use for:** "Sensor fusion", "State estimation", "Tell me about a math-heavy problem"

## Current-Work Answers (forward-looking - NOT yet earned)

> **Usage rule.** These are in-progress builds with target outcomes, not achievements. Use
> them **only** to answer "what are you working on now". Never state a target result as a
> delivered one, and never put them on a CV until the milestone actually lands. Once a
> milestone lands, move the story up into the earned section above and update
> `01-candidate-profile.md`.

### 8. (Partly earned) BLDC robotic joint (FW + robotics - FOC, ROS2, EKF on hardware)

> **Corrected 2026-07-19.** The original framing ("motor control and EKF in simulation but no
> integrated robot on real hardware") understated what is already done. The Motion Sync ROS2
> DAQ ran **BLDC-FOC motor control over CAN-FD on real hardware**, with the candidate both
> integrating it over CAN and writing portions of the FOC control loop. Hardware FOC is
> **earned** - cite the DAQ for it. What remains forward-looking is a *complete* bare-metal
> FOC implementation (full Clarke/Park + SVPWM + current-sense loop authored end to end) and
> the integrated ROS2 joint with live EKF pose.

**S:** Hardware BLDC-FOC over CAN-FD is already delivered on the DAQ, but not as a complete
self-authored control loop, and not as an integrated ROS2 joint with state estimation.
**T:** Build a ROS2-integrated BLDC joint with state estimation, carrying full firmware depth.
**A (in progress):** SimpleFOC torque/velocity control on the bench, CAN node publishing joint
state, ROS2 URDF and joint-state node, EKF (robot_localization) fusing encoder and IMU.
Firmware-depth phase: bare-metal Clarke/Park plus SVPWM and current-sense loop, and a
bare-metal CAN driver.
**R (target):** Demo video of the joint following a ROS2 trajectory with live EKF pose;
FOC-internals and CAN-driver talking points.

### 9. (Forward) RV1126 edge camera and driver (BSP - device tree, V4L2, RKNN)
**S:** Has Yocto/ROS2 Linux experience but no authored kernel driver or deployed NPU model
with real numbers.
**T:** Produce the headline embedded-Linux/BSP deliverable.
**A (in progress):** Board bring-up to shell (U-Boot/UART), V4L2 capture and ISP tuning, RKNN
INT8-quantized YOLO on the NPU with measured FPS/latency, custom char driver or device-tree
node for an I2C sensor or GPIO interrupt, Buildroot/Yocto layer.
**R (target):** Inference-overlay demo video, an authored driver, and quantified NPU numbers.
**Note:** This is the strongest single proof point for BSP roles once it lands.

## Quick-Pick by Interview Type
- **"Tell me about a hard bug"** -> 1 (power rail) or 4 (ISR flag)
- **"Deep firmware / RTOS"** -> 3 (pvPortRealloc) or 2 (delta OTA)
- **"Embedded Linux / BSP"** -> 5 (ROS2 + Yocto), or 9 as current work
- **"Systems / bus design"** -> 6 (CAN-FD message ID and arbitration)
- **"Sensor fusion / estimation"** -> 7 (IAE-KF)
- **"What are you working on now"** -> 8 (BLDC) or 9 (RV1126)

## Common Tough Questions

### "Why are you looking to leave Motion Sync?"
> _TODO - prepare this. Be honest, forward-looking, no negativity about the startup._

### "You have not authored a production kernel driver / shipped an NPU model."
> Acknowledge directly, then bridge: device-tree modification and a custom GPIO kernel module
> are already done (Lichee Pi Zero, RV1126), and the RV1126 driver work is in progress with a
> defined deliverable. Concrete in-flight work beats a claim of interest.

### "Your automotive and ADAS work is coursework or personal, not production."
> True for the S32K144 and self-driving projects. Bridge to the Motion Sync DAQ and openpilot
> port, which are real hardware with real integration problems, and to shipping the Haptic
> Pillow end-to-end through FCC compliance to sale.

### "Where do you see yourself in 5 years?"
> _TODO - prepare, aligned to the role's growth path._

### "What's your biggest weakness?"
> _TODO - genuine weakness with a concrete mitigation strategy._

### "Why this company specifically?"
> Customize per company. Must reference specific projects, values, market position, or team
> structure. Never generic.

## Questions You Should Ask Interviewers

### About the Role
- "What does a typical week look like in this role?"
- "What would success look like in the first 6 months?"
- "What's the biggest challenge the team is facing right now?"
- "How much of the role is board bring-up versus maintaining existing firmware?"

### About the Team
- "How big is the team, and how do you divide work?"
- "What does the development lifecycle look like, from schematic to shipped firmware?"
- "How do you onboard new team members?"
- "Who owns the boundary between hardware and firmware here?"

### About Tech & Growth
- "What's your current toolchain and build system for target images?"
- "How do you handle field updates and rollback?"
- "Is there room to grow into architectural decisions?"

### About Culture
- "How would you describe the team culture?"
- "What's the balance between new development and maintenance work?"
- "How would you describe the leadership style in this team?"
- "What do people who thrive here have in common?"
- "Is there lab access and hardware availability per engineer?"

## Phone/Video Interview Tips
- Have STAR examples written out (use this file)
- Keep a glass of water nearby
- Smile when speaking (it changes your tone)
- Ask for clarification if a question is vague
- It's OK to take 5 seconds to think before answering
- End with: "Is there anything else you'd like to know about my background?"

## After the Application (Best Practice)

### Follow-Up Etiquette
- **Don't call to "stand out"** or to learn more about the role post-submission
- If the employer specified a timeline, respect it and wait
- If no timeline was given and 2+ weeks have passed, a brief status call is acceptable
- If you have genuinely new, relevant information, a short follow-up is fine

### Thank-You Notes
- Send a brief thank-you on any update (interview invitation, rejection, status)
- Express appreciation for their time and the process
- Keep it short (2-3 sentences)

## Roleplay Guidelines
When the user asks for interview practice:
1. Ask which role/company to simulate
2. Start with easy warm-up questions ("Tell me about yourself")
3. Progress to role-specific technical questions
4. Include 1-2 behavioral questions using the competencies from the job posting
5. End with a tough question or curveball
6. After each answer, give brief feedback: what worked, what to sharpen
7. Suggest which STAR example would work best for each question
