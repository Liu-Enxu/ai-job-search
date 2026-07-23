# Search Queries for Job Scraper

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos and any skill you add with `/add-portal` are included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

> **US market note:** the four shipped portal demos (Jobindex, Jobbank, Jobdanmark, Jobnet) are Danish and will return nothing useful here. The working CLI for this search is `linkedin-search`. Scaffolding Indeed, Dice, or Built In with `/add-portal` would materially improve coverage — until then, the `site:` fallbacks below carry most of the load.

## HARD FILTER: Work Authorization

**Apply before anything else.** The candidate needs an employer willing to file an **H1B transfer** (lottery-selected, petition approval pending). See `04-job-evaluation.md` section 4.

Drop from results, regardless of technical match. Match these case-insensitively and treat any hit as disqualifying:
- "must be authorized to work in the US without sponsorship"
- "no visa sponsorship" / "no sponsorship available" / "no C2C"
- "US citizen" / "U.S. citizen" / "green card" / "permanent resident" required
- **"US Person" / "U.S. Person"** — the standard euphemism, and the one most often missed
- **ITAR / EAR / export control / export-controlled / "export licensing"**
- "security clearance" / "active clearance" / "Secret" / "Top Secret" / "TS/SCI" / "DoD" / "public trust"
- "must be able to obtain a clearance" — still disqualifying; clearance eligibility requires citizenship

**Sector pre-filter.** Aerospace, defense, space, and defense-adjacent automotive suppliers are dense with these restrictions and dense with otherwise-ideal embedded roles. Postings from these employers should be treated as **restricted until proven otherwise**, not the reverse. A first test search on 2026-07-19 returned a SpaceX embedded OS/platform role in the top five - a strong technical match that Enxu cannot take. Expect this to recur constantly and filter early, before it costs drafting time.

Flag (do not drop) postings that are silent on sponsorship. Verify before drafting.

Useful positive-signal search modifiers:
```
"H1B" OR "visa sponsorship" OR "will sponsor" OR "sponsorship available"
-"no sponsorship" -"without sponsorship" -"US citizen" -ITAR -clearance
```

## Search Sites

Primary:
- **linkedin.com/jobs** — covered by the `linkedin-search` CLI. **This is the only working portal CLI for this search.** Verified 2026-07-19 returning current, relevant embedded roles. LinkedIn's own "willing to sponsor" filtering is unreliable; verify in the posting text.
- **builtin.com** — startup/tech roles by metro (Bay Area, Seattle, Austin, Boston). WebSearch fallback only.

The four Danish demo portals (`jobindex`, `jobbank`, `jobdanmark`, `jobnet`) are set `enabled: false` so `/scrape` skips them. `freehire-search` remains enabled (country-agnostic). To re-enable any, flip `enabled:` back to `true` in its `SKILL.md`.

### Portals investigated and rejected (2026-07-19) — do not retry without new information

- **Indeed: not possible.** Returns **403** to server-side requests (Cloudflare); the public jobs API was retired. Any CLI would return zero results, and circumventing the block would breach their ToS. Use WebSearch `site:indeed.com` as a fallback instead.
- **Dice: rejected as too fragile.** The site responds 200 but is entirely client-rendered (363 KB of HTML, no job data). Its data lives on `job-search-api.svc.dhigroupinc.com` behind a rotating `x-api-key`; the formerly-public key is revoked. A CLI would depend on a key scraped from turbopack-hashed bundles and would break **silently** on their next rebuild. Deliberately skipped.
- **Greenhouse / Lever / Ashby: viable, not built.** These are *officially public* ATS APIs and returned real postings in testing (`boards-api.greenhouse.io/v1/boards/<slug>/jobs`, `api.lever.co/v0/postings/<slug>?mode=json`, Ashby's `non-user-graphql`). Most embedded and robotics companies use one of them. Combined with public H1B filing records to build a target-company list, this is the highest-value option for a sponsorship-constrained search, and the natural next step if LinkedIn-only coverage proves thin.
- **myvisajobs.com** and **h1bgrader.com** — not job boards, but searchable records of which employers have actually filed H1B petitions. Use to check sponsorship history for a FLAG posting before investing drafting time.

Secondary:
- Company career pages via `site:` searches for known target employers

## Query Categories

Combine each with location terms. Priority metros: Bay Area, Seattle, Austin, Boston (weighted up), plus Ann Arbor / Detroit metro (no relocation required).

> **ACTIVE FOCUS (updated 2026-07-19): MCU firmware leads; embedded Linux stays in, filtered.**
> Priority 1 (MCU) and Priority 3 (robotics) run first and carry the most weight. Priority 2
> (Embedded Linux / BSP) is **back in the search**, but every Linux/BSP posting must clear the
> **Two-Unknowns Rule** below before being presented.
>
> **THE TWO-UNKNOWNS RULE, WEIGHTED (applies to every posting, Linux or not).** Score the
> unfamiliar technology clusters in the posting's **required** skills. **Weighted total above
> 2.0 → skip**, however well it scores.
>
> | Weight | What |
> |---|---|
> | 1.0 | A distinct engineering discipline, vendor ecosystem, or domain body of knowledge with no foothold (Jetson/L4T, embedded security, Wi-Fi 802.11 stacks, IP networking, QNX, EtherCAT, BT audio/DSP, multi-DOF kinematics) |
> | 1.0 | An unmet **portfolio gate** - "shipped a custom Linux OS", "upstream contributions", "public open-source track record", "led teams / owned large-scale deliverables". Cannot be studied, only accumulated. |
> | 0.5 | Learnable tooling, a single library/framework, or an adjacent variant (a different EDA tool, one unfamiliar build system) |
> | 0.0 | **Excluded entirely** - see below |
>
> **Never counted:** (a) regulations, certifications, and compliance standards - ISO 26262,
> IEC 62304, DO-178C, ANSI/RIA R15.06, FDA design control, FCC, MISRA, AUTOSAR-as-process;
> employers train these and nobody expects a hire to arrive certified. (b) general dev tooling -
> Docker/containers, IDEs, ticketing, CI services. (c) anything transferable: delta OTA with
> rollback → fail-safe/A-B OTA; FreeRTOS → another RTOS; STM32/ESP32 → another Cortex-M;
> MIPI-CSI → DSI.
>
> Count what takes months to acquire, not what takes an afternoon or what the employer trains.
>
> Worked examples: **Apptronik Senior Embedded Linux (4375011566) = 2.5, skipped** (Jetson 1.0 +
> security engineering 1.0 + OverlayFS/partitioning 0.5). **Apptronik Firmware Engineer
> (4377899602) = 1.0, kept** (EtherCAT 1.0; the ANSI/RIA safety cert and Docker are both
> excluded). Same raw count of three, opposite verdicts.
>
> Full rule and rationale: `04-job-evaluation.md` section 5b.

### Priority 1: Embedded Firmware / MCU Engineer

Deepest earned evidence: FreeRTOS, ESP32/ESP-IDF, STM32, BLE/OTA, drivers, board bring-up.

```
site:linkedin.com/jobs "embedded software engineer" firmware C++ "visa sponsorship"
site:linkedin.com/jobs "embedded firmware engineer" FreeRTOS OR RTOS
site:dice.com "firmware engineer" "bare metal" OR FreeRTOS -"no sponsorship"
site:indeed.com "embedded software engineer" STM32 OR ESP32
site:linkedin.com/jobs "embedded engineer" BLE OR Bluetooth firmware
site:builtin.com "firmware engineer" embedded
```

### Priority 2: Embedded Linux / BSP Engineer — active, Two-Unknowns filtered

Career direction and the target of the in-flight RV1126 work. **Every hit here must clear the
Two-Unknowns Rule above before being presented** - this category is where it bites hardest,
because BSP postings stack vendor ecosystems, security stacks, and portfolio gates that the
profile does not yet reach.

```
site:linkedin.com/jobs "embedded linux engineer" Yocto OR Buildroot
site:linkedin.com/jobs "BSP engineer" OR "board support package" linux
site:dice.com "embedded linux" "device tree" OR "kernel driver"
site:indeed.com "linux kernel engineer" embedded -"no sponsorship"
site:linkedin.com/jobs "platform software engineer" linux embedded U-Boot
site:linkedin.com/jobs "embedded software engineer" Yocto sponsorship
```

### Priority 3: Robotics / Controls Engineer

Real but thinner: much of the controls work is coursework or simulation. Weight ROS2 and sensor-fusion postings above pure controls-theory ones.

```
site:linkedin.com/jobs "robotics software engineer" ROS2 C++
site:linkedin.com/jobs "embedded robotics engineer" ROS OR ROS2
site:dice.com "sensor fusion" engineer kalman OR EKF embedded
site:linkedin.com/jobs "motion control engineer" BLDC OR FOC firmware
site:builtin.com robotics "embedded" engineer
```

### Priority 4: Adjacent and Wider Net

```
site:linkedin.com/jobs "mechatronics engineer" firmware embedded
site:linkedin.com/jobs "hardware engineer" firmware "bring-up"
site:linkedin.com/jobs "systems engineer" embedded C++ linux
site:linkedin.com/jobs "IoT engineer" embedded BLE firmware
site:dice.com "camera" OR "vision" embedded engineer V4L2 OR MIPI
site:linkedin.com/jobs "edge AI" OR "edge computing" engineer embedded NPU
```

## Location Filter

- **Weighted up:** San Francisco Bay Area, Seattle, Austin, Boston — largest embedded markets and the densest concentration of employers with established sponsorship processes
- **Also strong:** Ann Arbor, Detroit metro (no relocation required; strong automotive embedded cluster)
- **Acceptable:** San Diego, Portland, Denver, Raleigh-Durham, Chicago, NYC, Los Angeles, Phoenix
- **Remote (US):** acceptable, but flag — board bring-up and BSP roles rarely work fully remote; verify lab and hardware access
- **Exclude:** non-US locations (would disrupt the H1B timeline)

## Experience-Level Filter

**Include both entry-level and senior postings** — do not filter by title seniority. Titles are
unreliable proxies: a "Senior" title at a startup may ask for 3 years while an "Engineer II" at
a large firm asks for 6.

**The binding rule is the stated experience requirement: keep postings asking for 5 years or
fewer.** Enxu has ~2 years post-MSc (Apr 2024 - present), plus prior project depth.

- Requires 0-5 years, or states no minimum → **keep**
- Requires 6+ years → **drop**, regardless of how well the tech matches
- States a range spanning the line (e.g. "3-7 years") → **keep and flag**; the bottom of the
  range is what matters at application time
- No stated requirement → **keep**; judge from the responsibilities instead

Titles like "Principal" and "Staff" usually carry a 8+ year expectation and will mostly fail
this test, but check the stated requirement rather than dropping on the title alone.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

**Given the transfer deadline, prefer recent postings.** A role posted 30+ days ago may already be deep in its pipeline, and pipeline position matters more than usual when the timeline is tight.

## When To Stop Scraping (saturation rule, added 2026-07-19)

Earlier runs in this repo stopped at an arbitrary budget - about 6 queries capped at 8-10
results each, roughly 40-50 raw hits per run. That is too small for a US-wide search and it
stopped for no principled reason. Use the following instead.

### Run in waves, measure saturation

A **wave** is one pass over a coverage slice (see the matrix below), each query capped at
~20 results. After each wave compute:

```
new-unique rate = (results not already keyed in seen_jobs.json) / (total results returned)
```

**Stop when two consecutive waves return under 20% new-unique.** That means the query set is
re-finding what it already found, and more of the same queries will not help - the fix at that
point is new query *phrasings* or a new portal, not more pages.

### Floors and ceilings

- **Do not stop before the coverage matrix is complete** (below), even if saturation looks
  early. A high dedup rate inside one metro says nothing about an unsearched one.
- **Do not continue past ~15 banked viable roles** in one sitting. Beyond that the bottleneck
  is application effort, not lead volume, and `/rank` plus `/apply` are the better spend.
- **Expect roughly a 50% kill rate.** This repo's first three runs tracked 36 postings and
  eliminated 18 on work authorization, experience ceiling, or the Two-Unknowns Rule. So to bank
  N viable roles, plan on surfacing about 2N.

### Coverage matrix (a wave is complete when every cell is hit once)

| | Priority 1 (MCU) | Priority 2 (Linux/BSP) | Priority 3 (Robotics) |
|---|---|---|---|
| Austin | | | |
| Bay Area | | | |
| Seattle | | | |
| Boston | | | |
| Ann Arbor / Detroit | | | |
| US-wide / remote | | | |

18 cells. At ~20 results per cell that is a genuine wave, roughly 4x what the early runs did.

### Query construction (learned the hard way)

**Keep queries to 2-4 terms.** LinkedIn matches literally, so stacking terms silently returns
nothing. Both of these returned **zero results**:

```
"motor control firmware engineer BLDC"          -> 0 results
"embedded software engineer microcontroller C"  -> 0 results   (Austin)
```

while `"firmware engineer"` and `"embedded linux engineer"` returned full pages. A zero-result
query is not evidence the market is empty - it is usually evidence the query was over-specified.
Re-run any empty query with fewer terms before concluding anything.

### What saturation does *not* fix

Hitting saturation on LinkedIn means LinkedIn is exhausted, not the market. With only one
working portal for this search, saturation is the signal to **add a source** (see the
Greenhouse/Lever/Ashby note above), not to conclude the search is done.

## Posting Age: the displayed date lies (added 2026-07-19)

**LinkedIn's displayed date is a refresh date, not a creation date.** Employers repost stale
requisitions and the listing shows as days old. Always estimate true age before deciding how
to approach a role.

**Method.** LinkedIn job IDs increment roughly with creation time. Within one scrape batch,
sort by numeric job ID and compare against displayed dates: for genuinely new postings the two
rise together. Any posting whose ID sits well *below* that band while showing a *recent* date
is a repost. Calibrate the rate from the postings that do correlate (measured 2026-07-19:
**~520,000 IDs/day**, from five clearly-new postings spanning 6-18 July), then estimate
`created = base_date + (id - base_id) / rate` and report the lag.

**Limits, state them when reporting.** Five-point linear calibration; IDs are not strictly
globally sequential and the rate drifts over time. The *ordering* is reliable, the exact day
counts are not. Treat output as "fresh / recent / long-running", never as a precise date. A
repost can also mean a reopened or re-approved req, or several openings on one listing.

**What to do with it.**
- **Fresh (lag < 8 days):** apply quickly; the pool is still shallow and speed is real advantage.
- **Long-running repost (lag > 25 days):** the application alone is weak leverage - months of
  applicants have already gone into that pipeline and been passed over. Prioritise a referral or
  a direct message to the hiring manager or recruiter. Read the age as ambiguous, not bad: a req
  open for months is harder to fill, which can favour a candidate slightly below spec, but it can
  equally be an evergreen pipeline posting with low intent to close.

**Applicant counts are NOT available.** The `linkedin-search` CLI uses the public `jobs-guest`
endpoints, which do not return applicant numbers - that figure exists only on the logged-in
LinkedIn job page. Do not guess it, and do not imply it is known. If the user wants it, they
must check the posting while signed in.

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- `/scrape bsp` -> Priority 2 queries plus custom device-tree/driver-specific queries
- `/scrape sponsorship` -> re-run all priorities with the positive-signal sponsorship modifiers appended
