---
framework_version: 1.0.0
---

# Behavioral Profile

> **Status: partially populated.** No formal assessment (PI/DISC/MBTI/StrengthsFinder), no
> LinkedIn About text, and no reference letters were available at setup. The observations
> below are inferred from CV bullets and `documents/cv/STAR_stories.md`, which show *how*
> Enxu works through problems and are stronger evidence than CV bullets alone, but they are
> still self-authored. Review before relying on any of it in an interview.
>
> To strengthen this file: drop a LinkedIn export in `documents/linkedin/` or reference
> letters in `documents/references/`, then re-run `/setup`.

## Overview
No formal profile type on file. The working picture from the source material is an engineer
who takes end-to-end ownership of hardware-plus-firmware scope and debugs by instrumenting
rather than guessing.

## Strongest Behaviors

- **End-to-end ownership.** *[Inferred from CV + STAR stories - review before relying on this]*
  The language across sources is consistently first-position: "fully owned", "led", "architected".
  The Haptic Pillow ran from firmware architecture through PCB design, bring-up, FCC compliance,
  and shipping to sale. Evidence: STAR 1, 2.
- **Instrument-first debugging.** *[Inferred from STAR stories - review before relying on this]*
  The recurring pattern in every debugging story is reaching for the right measurement tool
  before forming a theory: oscilloscope on the supply rail (STAR 1), logic analyzer on ISR
  timing (STAR 4). The stated lesson from STAR 1, "intermittent digital faults are often
  analog/power problems", is a systems-level generalization, not a one-off fix.
- **Depth over API-level sufficiency.** *[Inferred from STAR stories - review before relying on this]*
  Repeatedly goes below the abstraction that would have been good enough: wrote a custom
  `pvPortRealloc` handling coalesce edge cases the FreeRTOS API omits (STAR 3); designed
  CAN-FD message IDs and arbitration rather than just transmitting frames (STAR 6).
- **Self-directed capability building.** *[Inferred from CV - review before relying on this]*
  Three unpaid hardware projects (RV1126, Lichee Pi Zero, STM32 handheld) run concurrently
  with full-time work, each targeting a named gap rather than being hobby-shaped.
- **Explicit gap awareness.** *[Inferred from STAR_stories.md structure - review before relying on this]*
  The STAR file tags each story with the gap it closes and separates already-earned from
  forward-looking work. Candidly self-assessing where the resume is thin, and building
  deliberately against it, is a genuine strength and worth surfacing in interviews.

## How You Work Best

**Thrives at the intersection of three areas** (candidate-stated):
- **Secure firmware** - safety and security properties as first-class design constraints
- **Robotics ecosystem** - ROS2 and hardware-integrated robotic systems
- **Real-time RTOS** - hard timing guarantees, determinism, scheduling

**Core motivation (candidate's own words):** exploring the limits of real-time systems under
*safety* constraints (secure firmware, error handling) and *timing* constraints.

This is the sharpest positioning signal in the profile, and it is a genuinely coherent one:
it connects work already delivered (FreeRTOS timer accuracy, core-pinned jitter isolation,
rollback-protected OTA) to where the candidate wants to go. Use it in cover letters as the
"why this work" thread. Note the distinction from the growth areas below: **secure firmware
motivates him but is not yet a demonstrated skill.** Both statements are true and must both
be respected - write about it as what draws him, never as what he has already done.

_Still unrecorded - ask if a posting turns on any of it:_
- Preferred team size and structure
- Degree of autonomy vs direction
- Tolerance for maintenance work vs greenfield development

## Growth Areas (frame positively in applications)

Candidate-stated development targets. These are **directions of travel, not current skills** -
never draft them as existing competencies.

- **Security firmware.** Actively developing. Secure boot, signed images, key storage, and
  attack-surface reduction are natural extensions of the bootloader and OTA work already done
  (delta OTA with rollback protection is adjacent to signed-update work, and honest to cite as
  the foundation being built on). The candidate plans to update the CV as this develops - check
  `01-candidate-profile.md` for current status before claiming anything here.
- **ROS2 / robotics ecosystem.** Deepening beyond the node architecture and DAQ integration
  already delivered, toward the wider ecosystem (navigation stack, robot_localization,
  simulation, hardware integration). The in-flight BLDC joint project is the vehicle for this.

Framed positively: both are deliberate extensions of demonstrated work, not gaps being papered
over. A posting asking for firmware security or deeper ROS2 is a *motivating* fit even where
the current match is partial, and is worth flagging to the candidate as such.

Candidate-identified technical gaps from `STAR_stories.md` are recorded in
`04-job-evaluation.md` rather than here.

## Mapping to Job Posting Language

**Likely strong behavioral fit** *[inferred - validate]*:
- "end-to-end ownership", "own the full stack from schematic to firmware"
- "small team", "startup environment", "wear many hats"
- "debug complex hardware/software interactions", "root cause analysis"
- "greenfield", "bring-up", "0-to-1 product"

**Flag as potential friction** *[inferred - validate]*:
- Roles that are predominantly maintenance of a mature codebase with no hardware access
- Large-organization roles with narrow, single-layer scope (firmware only, no board contact)
- Heavy process/documentation roles with little hands-on lab time

## Management Style Preferences
_TODO - needs candidate input._

## Using This in Applications
- **Cover letters:** Lead with end-to-end ownership and a concrete debugging narrative. The
  power-rail story (STAR 1) is the most broadly legible.
- **CV:** Emphasize breadth across the stack (firmware, board, Linux) plus the quantified
  outcomes (1.2 KB patch, <10 s update, 30% cost reduction, ~1.94 mW).
- **Interviews:** See `07-interview-prep.md`. Stories 1 and 4 for "hard bug", 3 for RTOS depth.
- **Don't overstate:** No formal behavioral assessment exists. Do not claim leadership of
  people; "led firmware design" is scope leadership on a startup team, not people management.
