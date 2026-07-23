---
framework_version: 1.0.0
---

# Job Evaluation Framework

<!-- SETUP: Skill match areas and career goals are personalized by running /setup -->

## Scoring Dimensions

Evaluate each job posting against these five dimensions:

### 1. Technical Skills Match (0-100)
How well do the required/preferred skills align with the candidate's capabilities?

| Score | Meaning |
|-------|---------|
| 80-100 | Core requirements are primary skills |
| 60-79 | Most requirements match, 1-2 gaps that are learnable |
| 40-59 | Partial match, significant upskilling needed |
| 0-39 | Fundamental mismatch |

**Strong match areas:** Embedded C/C++; **firmware security (hardware root of trust, applied cryptographic signatures, verified/secure boot chain, secure debug lockout, rollback-protected OTA)**; FreeRTOS and bare-metal firmware (including heap/allocator internals); MCU peripheral and driver work (I2C, SPI, UART, ADC, PWM, DMA); ESP32/ESP-IDF and STM32/HAL; BLE and OTA/delta update with rollback; board bring-up and hardware debug (oscilloscope, logic analyzer, JTAG/SWD); embedded Linux BSP (Yocto, Buildroot, U-Boot, kernel build, device tree, custom kernel modules)

**Moderate match areas:** ROS2 and robotics middleware; **BLDC-FOC motor control on real hardware over CAN-FD** (system integration plus partial control-loop implementation - hardware-validated, not simulation); controls (PID, LQR/LQE, MPC, Kalman/EKF, sensor fusion); PCB design (KiCAD) and analog bring-up; Qt (C++) GUI; Python data processing; CAN/CAN-FD; V4L2 and camera/ISP pipelines; NPU inference deployment (RKNN); MATLAB/Simulink

**Weak match areas (current state):** *Organisational/platform* security tier only - TPM 2.0, HSM, PKI/CA operations, fleet-scale key management, LUKS, SELinux/AppArmor, security certification (Common Criteria, FIPS). **Device-level firmware security is a STRONG area as of 2026-07-23** (hardware root of trust, signed images, secure boot, secure debug lockout) and must not be scored as a gap; web and cloud backend development; large-scale ML training (deployment-only exposure); functional-safety certification workflows (ISO 26262, DO-178C, IEC 61508) beyond MISRA C familiarity; formal people management; production-grade upstream kernel driver authorship (in progress, not yet earned); RF design; high-volume manufacturing test engineering

### Candidate-identified gaps (from `documents/cv/STAR_stories.md`)

The candidate has explicitly named these as targets being closed by in-flight projects. Treat
them as honest gaps, not hidden ones, and do not let `/apply` draft around them:
- Authored kernel driver and deployed NPU model with measured numbers (RV1126 project, in progress)
- ~~FOC internals and hardware-validated EKF on a real robot~~ **partly closed as of 2026-07-19.**
  Hardware BLDC-FOC over CAN-FD is **earned** via the Motion Sync ROS2 DAQ (integrated over CAN
  *and* wrote portions of the control loop). What remains open is a *complete* self-authored
  bare-metal FOC loop (Clarke/Park + SVPWM + current-sense) and an integrated ROS2 joint with
  live EKF pose. Score motor-control postings on the earned hardware experience, not the gap.

A posting demanding either as a hard requirement should score lower on experience match, but
the in-flight work is legitimate cover-letter material framed as current, not complete.

### 2. Experience Match (0-100)
Does work history align with what they're looking for?

| Score | Meaning |
|-------|---------|
| 80-100 | Direct experience in the same domain and role type |
| 60-79 | Related experience, transferable skills clear |
| 40-59 | Adjacent experience, would need to make the case |
| 0-39 | Unrelated experience |

**Strong:** Consumer-electronics firmware owned end to end (architecture through PCB, bring-up, FCC compliance, shipped to sale); startup/small-team environments; embedded Linux system integration on SBCs

**Moderate:** Automotive and ADAS contexts (real work on the DAQ and openpilot port, but the S32K144/self-driving items are coursework); robotics; data acquisition and instrumentation

**Entry-level:** Roles requiring 5+ years post-degree industry experience (approximately 2 years since the MSc, Apr 2024 to present, at a single employer); team lead or engineering management roles; safety-certified product development

**Seniority note:** One employer since Apr 2024, overlapping the MSc that ended Dec 2024.
Postings asking for senior/staff embedded engineers are a stretch on tenure regardless of
technical depth. Score experience match accordingly and flag it rather than reframing.

### 3. Behavioral/Culture Fit (0-100)
Does the role and company culture match the behavioral profile?

| Score | Meaning |
|-------|---------|
| 80-100 | Culture strongly matches behavioral preferences |
| 60-79 | Mixed signals but mostly compatible |
| 40-59 | Some friction areas |
| 0-39 | Significant culture mismatch |

**Red flags to research:** Department disorganization, work dominated by maintenance over development, poor chemistry with leadership, culture mismatches. Check reviews, media coverage, LinkedIn connections, and network contacts for insider perspective.

### 4. Work Authorization (Pass/Fail - HARD FILTER, evaluate FIRST)

**This gate outranks every other dimension. Check it before scoring anything else. A posting
that fails here scores nothing and is not drafted for, regardless of technical fit.**

The candidate holds an H1B selected in the lottery with petition approval pending, and needs
a **new employer willing to file an H1B transfer** (see the timeline in `CLAUDE.md`).

| Posting signal | Verdict |
|---|---|
| "Will sponsor" / "visa sponsorship available" / "open to H1B transfer" | PASS |
| Silent on sponsorship | FLAG - verify before investing drafting effort |
| "Must be authorized to work in the US without sponsorship, now or in the future" | **FAIL** |
| "No sponsorship available" / "no C2C, no sponsorship" | **FAIL** |
| US citizen / green card / permanent resident required | **FAIL** |
| ITAR / EAR / export-control restricted; "US Person" required | **FAIL** |
| Federal, defense, or cleared work requiring citizenship | **FAIL** |

## PRE-DRAFT GATE (both checks, every time, before any drafting)

Two cheap checks that must both pass before `/apply` Step 2. Each was learned by wasting a complete
tailored CV and cover letter on 2026-07-21.

**Gate 1 - Is the employer still accepting applications?**

> **A successful LinkedIn detail fetch does NOT satisfy this gate.** Job-board listings outlive the
> open requisition. Verification must happen on the **employer's own careers page or ATS**
> (Greenhouse, Lever, Ashby, Workday), where the role must appear AND have a working apply path.

Two applications were built and lost to this in one day:
- *Bevi Embedded Engineer (75/100)* - full CV and cover letter produced, export-control verified clear, then found closed.
- *Smith+Nephew Embedded SW Engineer II (re-scored 71/100)* - LinkedIn detail fetched fine, so it was called live; the employer had stopped accepting. **The LinkedIn-only check is what failed.**

If the employer's own listing cannot be reached, report the role as **unverified** and say the
liveness check could not be completed. Do not draft on a board listing alone.

**Gate 2 - Does the application page carry control restrictions?** See below.

Report both results before drafting. If either cannot be checked, say so and treat the role as
**unverified**, never as clear.

**CHECK THE EMPLOYER'S OWN APPLICATION PAGE BEFORE DRAFTING (learned the hard way 2026-07-21).**
A clean LinkedIn posting body does **not** mean the role is clear. Export-control and work-authorization
terms frequently appear only in the employer's application form or careers-site legal block, which the
job-board text never shows.

> **Worked example - Apptronik Firmware Engineer.** The LinkedIn posting contained no citizenship,
> ITAR, or export-control language whatsoever. It was evaluated at 80/100 Strong Fit, ranked #1, and a
> full tailored CV and cover letter were produced. Only when the candidate opened the actual application
> page did this appear: *"Technology Control Restrictions: As a person in this position will have access
> to technical data and/or computer software maintained by the Company, which includes export-controlled
> technical data and/or computer software, successful applicants must be eligible under U.S. export
> control regulations to access such information."* That is a US Person requirement. All the drafting
> effort was wasted.

**Required step:** before Step 2 of `/apply` (drafting), open the employer's real application page or
careers listing and search it for: `export`, `ITAR`, `EAR`, `control`, `citizen`, `U.S. Person`,
`sponsor`, `clearance`. Report what was found. If the page cannot be reached without submitting an
application, say so explicitly and treat the role as **unverified**, not as clear.

**ITAR is a common trap in this candidate's target space.** Automotive defense suppliers,
aerospace embedded, and anything space- or defense-adjacent frequently require US Person
status even when sponsorship is otherwise offered. Check explicitly on any
aerospace/defense/space posting.

**Sponsorship likelihood by employer type** (heuristic for the FLAG case, not a rule):
- Large tech and established semiconductor/automotive firms: usually have counsel and a
  process. Best odds.
- Mid-size firms with existing international staff: workable, often slower.
- Small startups (<50 people) with no prior sponsorship: frequently unwilling or unable, and
  the cost and timeline may be genuine blockers rather than reluctance. Ask early.

**Position this as an advantage, not an apology.** A transfer for an already-selected,
approved petition is not subject to the lottery, so the hiring employer takes on none of the
lottery uncertainty that makes companies hesitate on cap-subject candidates. This is a real
and material difference from a typical sponsorship request and is worth stating plainly in
early conversations with recruiters.

> The immigration specifics here (grace-period length, transfer timing, premium-processing
> eligibility, portability under AC21) are the candidate's to confirm with an immigration
> attorney. This file records them only to filter postings, and nothing in it is legal advice.

### 5. Location & Logistics (Pass/Fail + Notes)
- Bay Area, Seattle, Austin, Boston: PASS, **weight up** (larger embedded markets, denser
  sponsorship experience)
- Ann Arbor / Detroit metro: PASS (no relocation needed, strong automotive embedded cluster)
- Other US metros: PASS (open to relocation)
- Remote: PASS, but note that board bring-up and BSP roles are rarely genuinely remote; verify
  hardware/lab access
- Requires relocation outside the US: FLAG (would disrupt the H1B timeline)
- Frequent international travel: FLAG (international travel while a transfer is pending
  carries re-entry risk; confirm with counsel)

### 5b. The Two-Unknowns Rule (hard skip, set 2026-07-19; weighted 2026-07-19)

**Score the unfamiliar technology clusters named in a posting's REQUIRED skills. If the
weighted total exceeds 2.0, skip the posting** - regardless of how high career alignment scores.

How to apply it:
- Count **required** sections only. "Preferred", "nice to have", "bonus", "ways to stand out",
  and "desired" items do not count. List them separately so they stay visible, but they never
  contribute to the total. **Misreading a preferred item as required is the single most common
  error here** - check which section every item sits in.
- Count **clusters**, not individual words. `LUKS + TPM 2.0 + PKI` is one cluster (embedded
  security engineering). `Yocto + Buildroot` is one cluster (distro build systems).
  `DNS + DHCP + VPN + routing` is one cluster (IP networking).

#### Weights

| Weight | What it covers |
|---|---|
| **1.0** | A distinct engineering discipline, vendor ecosystem, or domain body of knowledge with no foothold in the profile. Examples: Nvidia Jetson/L4T ecosystem, embedded security engineering, Bluetooth Classic audio/DSP, Wi-Fi 802.11 stacks, IP networking architecture, QNX, kinematics/dynamics for multi-DOF manipulation, EtherCAT. |
| **1.0** | An unmet **portfolio gate** - "demonstrated experience shipping a custom Linux OS", "upstream kernel contributions", "public open-source track record", "experience leading teams and owning large-scale deliverables". These cannot be studied, only accumulated, so they never discount. |
| **0.5** | Learnable tooling, a single library or framework, or an adjacent variant of something known. Examples: a specific EDA tool (Altium vs KiCAD), one unfamiliar build system, a particular test framework. |
| **0.0** | **Never counted** (see exclusions below). |

#### Never counted as unknowns (set 2026-07-19 at the candidate's direction)

- **Regulations, certifications, and compliance standards.** ISO 26262, IEC 62304, DO-178C,
  ANSI/RIA R15.06, FDA design control, FCC, AUTOSAR-as-process, MISRA. These are learned on the
  job, usually with the employer's own training and QMS, and no employer expects a hire to
  arrive certified. Treating them as barriers wrongly eliminates whole regulated industries
  (medical, automotive, industrial robotics) where the underlying firmware work is a strong match.
- **General development tooling and infrastructure.** Docker and containerization, specific
  IDEs, ticketing systems, particular CI services, VCS hosting. A candidate with CI/CD, bash,
  Git, and cross-compilation experience picks these up in days.

The distinction that matters: **count what takes months of practice to acquire, not what takes
an afternoon or what the employer will train.**

- **Transferable** (never counted) means the candidate has done something adjacent enough to
  claim honestly and pick up fast. Delta OTA with rollback transfers to fail-safe/A-B OTA.
  FreeRTOS transfers to another RTOS. ESP32/STM32 transfers to another Cortex-M part such as
  TI C2000 or NXP. MIPI-CSI transfers to DSI.

#### Worked example A - Apptronik **Senior Embedded Linux** (4375011566): SKIPPED at 2.5

| Required cluster | Weight |
|---|---|
| Nvidia Jetson ecosystem (L4T, Jetpack SDK, flashing utilities) | 1.0 - vendor ecosystem, no foothold |
| Security engineering (LUKS, TPM 2.0, PKI for embedded devices) | 1.0 - distinct discipline |
| Resiliency: OverlayFS + partition management | 0.5 - Linux tooling, adjacent to existing BSP work |
| *(fail-safe OTA)* | 0.0 - transferable from delta OTA with rollback |
| **Total** | **2.5 → skip** |

Skipped even though it scored career alignment 85, the highest of any job in the batch.

#### Worked example B - Apptronik **Firmware Engineer** (4377899602): 1.0, REINSTATED

Originally miscounted as 3 unknowns and wrongly skipped. Under the weights:

| Required cluster | Weight |
|---|---|
| EtherCAT (industrial fieldbus) | 1.0 - distinct protocol discipline |
| ANSI/RIA R15.06 robot safety certification | **0.0** - a certification standard, excluded |
| Docker / containerization | **0.0** - general dev tooling, excluded |
| *(TI C2000 MCU)* | 0.0 - transferable from STM32/ESP32 Cortex-M |
| **Total** | **1.0 → keep** |

The contrast between A and B is the point of the weights: three raw "unknowns" can mean a
genuine wall or a single real gap plus two things you learn on arrival.

**Why this rule exists.** Career alignment measures *pull*, not *qualification*. A posting can
describe exactly the work the candidate wants and still be unreachable. The weighted average
blends those into one number that reads more encouraging than the JD warrants. This rule is
the correction: it gates on what the posting demands, before the score gets computed.

**Related failure mode this catches.** Embedded Linux / BSP postings frequently gate on
*portfolio* rather than tenure - "demonstrated experience shipping a custom Linux OS",
"upstream kernel contributions", "public open-source track record". Those artifacts are
inherently senior and cannot be satisfied by strong recent years. Treat an unmet portfolio gate
as one unfamiliar cluster, and note it explicitly to the candidate rather than burying it.

### 6. Career Alignment & Motivation (0-100)
Does this role advance career goals and contain tasks that energize?

| Score | Meaning |
|-------|---------|
| 80-100 | Strongly aligned with career direction, clear growth path |
| 60-79 | Good role but only partially aligned with long-term goals |
| 40-59 | Decent job but doesn't build toward career goals |
| 0-39 | Dead end or backwards step |

**Career goals:**
- Deepen embedded Linux / BSP capability toward authored drivers and production board bring-up
  (the explicit target of the in-flight RV1126 work)
- Retain firmware depth rather than drifting to pure application or integration work
- Land at an employer with the stability and process to support a long-term immigration path,
  which a small spinoff startup may not be able to offer

**Growth directions (score career alignment UP when a posting offers these):**
- **Security at platform/organisational scale** - TPM, HSM, PKI/CA operations, fleet key
  management, MAC hardening. The device-level secure boot chain is earned (2026-07-23); the
  tier above it is the growth direction. A posting offering that is a strong career-alignment
  signal, and he now has a credible foundation to argue from rather than only interest.
- **ROS2 / robotics ecosystem depth** - navigation, localization, simulation, hardware
  integration beyond the node architecture already delivered.

**Motivation filter:** Evaluate not just whether you *can* do the tasks, but whether the tasks will *energize* you. Consider:
- **Tasks that energize:** exploring the limits of real-time systems under safety constraints
  (secure firmware, error handling and fault tolerance) and timing constraints. Concretely:
  hard real-time scheduling and determinism, RTOS internals, secure boot and signed images,
  failure-mode design, jitter and latency work, robotics/ROS2 systems with real timing budgets.
  **Score career alignment 80+ when a posting genuinely centers on any of these.**
- **Tasks that drain:** _not yet recorded_
- Non-task factors: leadership style, department culture, company values, degree of autonomy

**Life situation alignment:** Consider personal constraints:
- **Security**: [YOUR_FINANCIAL_SITUATION_CONTEXT]
- **Flexibility**: [YOUR_SCHEDULE_CONSTRAINTS]
- **Professional development**: [YOUR_GROWTH_PRIORITIES]

### 7. Salary Benchmark

**Target: 1.1x to 1.2x the local market average for an embedded systems engineer**, benchmarked
against the **metro area of the role**, not Ann Arbor. This matters a lot given relocation is on
the table: the average for Bay Area or Seattle is far above Ann Arbor or Detroit, so a number
that looks strong in Michigan can be a real-terms cut in California. Always anchor to the
posting's location and note the cost-of-living difference explicitly when comparing offers
across metros.

**How to produce the benchmark for a posting:**
1. Establish the local average for "embedded systems engineer" (or the closest match to the
   posting's actual title and seniority) in that metro, via WebSearch against Levels.fyi,
   Glassdoor, Built In salary reports, or BLS data. Cite which source and date.
2. Compute the 1.1x-1.2x band.
3. Compare against the posting's stated range if there is one. Many US postings now include a
   range by law in CA, CO, NY, WA, and others - use it when present rather than estimating.
4. Report as: local average, target band, posting range, and verdict (below / within / above).

**Seniority caveat:** with roughly two years post-MSc, "average" for the market may skew toward
more experienced engineers. If a posting is aimed at senior levels, note that the 1.1-1.2x
target is being measured against a bar the experience does not yet fully meet, rather than
silently treating the number as achievable.

**H1B note:** employers filing an H1B must pay at least the prevailing wage for the role and
location. This sets a legal floor, and it is a useful cross-check that a sponsoring employer's
offer is not being suppressed. It is a floor, not the target - the 1.1-1.2x band sits above it.

If the salary lookup tool is configured (`salary_data.json` exists), look up the company:
```
python salary_lookup.py "<Company Name>" --json
```

If a city is known from the posting, add `--city "<City>"` to narrow results.

Present findings as:
```
### Salary Benchmark
| Metric | Value |
|--------|-------|
| [Category] index | XX.X (+/-X.X% vs baseline) |
| Overall index | XX.X (+/-X.X% vs baseline) |
```

Interpret results relative to the baseline defined in the data file's metadata. For index-based data, higher typically means above-market compensation.

If the salary tool is not configured, skip this section.

## Output Format

Present the evaluation as:

```
## Job Fit Evaluation: [Role] at [Company]

| Dimension | Score | Notes |
|-----------|-------|-------|
| **Work Authorization** | **PASS/FLAG/FAIL** | **[state the posting's exact sponsorship wording]** |
| Technical Skills | XX/100 | [brief note] |
| Experience Match | XX/100 | [brief note] |
| Behavioral Fit | XX/100 | [brief note] |
| Location | PASS/FAIL | [brief note] |
| Career Alignment | XX/100 | [brief note] |

**Overall Score: XX/100** (weighted average of scored dimensions)

### Verdict: [Strong Fit / Good Fit / Moderate Fit / Weak Fit / Poor Fit]

### Key Strengths for This Role
- [bullet points]

### Gaps to Address
- [bullet points]

### Recommendation
[1-2 sentences: apply/skip/apply with caveats]

### Company Research Checklist
- [ ] Checked company website (mission, values, recent news)
- [ ] Checked review sites (Glassdoor, Jobindex, etc.)
- [ ] Checked LinkedIn for team size, recent hires, connections
- [ ] Checked media for restructuring, growth, or workplace issues
- [ ] Identified network contacts who may know the team/manager
```

## Weighting
- Technical Skills: 30%
- Experience Match: 25%
- Behavioral Fit: 15%
- Career Alignment: 30%

(Work Authorization and Location are pass/fail, not weighted. A Work Authorization FAIL
overrides the weighted total entirely: report the posting as skipped and do not compute a
score, however strong the technical fit looks.)

## Timeline Pressure (active as of July 2026)

The candidate is working against a transfer deadline of roughly end of 2026. This changes
triage economics in two ways:

- **Sponsorship confirmation comes before drafting.** A FLAG posting is worth one clarifying
  email or recruiter question before any CV tailoring effort. Time spent drafting for an
  employer that will not sponsor is time that cannot be recovered.
- **Prefer employers who can move fast.** Companies with in-house immigration counsel and
  premium-processing willingness are worth more than a marginally better technical fit at a
  company that will take months to decide. Factor stated interview-process length into
  ranking.

## Thresholds
- **Strong Fit** (75+): Definitely apply, tailor everything
- **Good Fit** (60-74): Apply, address gaps in cover letter
- **Moderate Fit** (45-59): Consider carefully, discuss with user
- **Weak Fit** (30-44): Probably skip unless strategic reasons
- **Poor Fit** (<30): Skip

## Pre-Application: Call the Employer (Best Practice)

Before writing the application, consider whether the candidate should call the contact person listed in the posting. **Only call if there are substantive questions** - never call just to "be remembered."

### When to Suggest Calling
- The posting has unclear or ambiguous requirements
- It's unclear which competencies are essential vs. nice-to-have
- The role description is vague about day-to-day tasks
- There's a named contact person who invites questions

### Good Questions to Ask
- "What are the primary challenges in this role?"
- "How is time typically divided across the listed responsibilities?"
- "Which competencies are most critical for success in this position?"
- "What does success look like in the first 6-12 months?"

### Rules for the Call
- Prepare a 30-second "elevator pitch" about your background in case they ask
- The call's purpose is **gathering information**, not delivering a pitch
- Take notes - use what you learn to tailor the application
- Reference the conversation naturally in the cover letter ("After speaking with [name], I was especially drawn to...")
