---
name: "kpis"
description: "Set up and maintain a KPI dashboard that tracks change over time. Use when establishing what to measure for a team or organisation, when running a periodic reporting cycle, or when preparing metrics for leadership or a board. Walks you through choosing your own KPIs and displays what they do; it does not decide what to measure or tell you what the numbers mean."
---

# kpis

Establishes a set of KPIs, records them on a declared cadence, and renders a readable dashboard
showing how they have moved.

**It walks you through picking your own KPIs and displays what they do. It does not choose your
metrics, interpret movement, or tell you what to act on.** Those are yours.

## The problem this solves

Three things, in order of difficulty:

1. **Point-in-time reporting hides the thing that matters.** Attrition of 12% means nothing.
   Attrition that moved 4% to 12% across three quarters is a different conversation. Most
   reporting shows the number, not the movement.
2. **Instrumentation drifts.** A metric gets declared, reported twice, then quietly stops
   appearing because nobody collected the data. The gap disappears rather than being visible.
3. **The dashboard and the story get built separately.** Numbers go in a spreadsheet, narrative
   goes in a doc, and by the time it reaches a board the two have diverged.

---

## Architecture

Two layers, kept separate so the tool can be cloned to a different domain.

### Engine

Domain-agnostic. Owns cadence, history, thresholds, gap handling, and rendering. Knows nothing
about what is being measured.

### Domain pack

Supplies the intake questions, the sensitivity flags, and any domain-specific handling rules. The
People pack ships as the reference implementation. Cloning to finance, programme delivery, or
fundraising means writing a new domain pack, not touching the engine.

**When adding a domain pack, it must define:** the intake questions, which data classes are
sensitive and require gating, any population-size or measurement caveats that apply to the domain,
and any definitions known to be ambiguous.

---

## Modes

The command runs in one of three modes. **It determines the mode itself from the presence of a
config file and the user's request — it does not ask which mode to run in.**

### Setup

Runs once. Fires when no config exists.

**Announce at the start that this is a one-time setup, that it will take a while, and that it will
not be repeated on subsequent runs.** The user is about to answer a lot of questions and should
know why.

**Also say what the first run will and won't show.** Run one establishes the baseline — current
readings, no movement, because there is nothing yet to move against. Movement appears from run
two, a trend from run three. Say this at setup so the first dashboard reads as a starting point
rather than a tool that isn't working.

Produces a config file the user commits.

### Record

The default. Fires whenever a config exists and the user has not asked to change settings.

Collects the period's data, computes aggregates, appends to history, renders the dashboard.

**Never re-asks setup questions in this mode.** If something is missing, handle it as a gap (see
below) rather than reopening intake.

### Amend

Fires only when the user explicitly asks to change what is being tracked.

Re-opens the relevant part of intake. Everything not being changed stays as it is. **Existing
history is never discarded** — if a KPI is removed, its history is retained and marked
discontinued from that date. If a KPI's definition changes materially, mark the break in the
series rather than recomputing backwards.

---

## Setup: what to establish

The domain pack supplies the specific questions. The engine requires these regardless of domain.

### Population size

Ask for it, and record it. It is needed for judging whether percentage-based movement is
meaningful, and it changes over time, so capture it every period as part of Record.

### Cadence

Monthly, quarterly, or a stated other. Declared once. The engine uses it to detect missed periods.

### The KPIs themselves

**Walk the user through choosing. Do not propose a list.**

Prompt them toward:

- What decision this metric would inform
- Who reads it
- Whether it is already being measured somewhere, or would be new instrumentation

If a user asks what they should be tracking, ask what decisions they are trying to make better.
Do not answer the question directly.

### Per KPI, capture

| Field | Purpose |
| --- | --- |
| Name | As it will appear on the dashboard |
| Definition | How it is calculated, in enough detail to be reproduced by hand |
| Unit | Count, percentage, currency, ratio, score |
| Source | Named placeholder — see below |
| Sensitivity | Flagged by the domain pack; drives gating on ingest |
| Threshold | Optional. Set by the user, not the tool — see below |
| Direction | Whether up is good, bad, or neither |

### Definitions that are known to be ambiguous

Some metrics have standard definitions that differ materially. Where the domain pack knows of one,
prompt rather than accepting the first answer given.

**Attrition / turnover.** Ask two things before recording the definition:

- **Regretted or non-regretted** — whether the organisation would have kept the person. Track them
  as separate readings. A rising total that is entirely non-regretted is a different finding from
  the reverse, and a combined figure hides which one you have.
- **The denominator** — headcount at period start, or average headcount across the period. These
  give different numbers. State which one on the dashboard.

The regretted/non-regretted classification is the user's judgment, supplied as an input. The tool
records the split; it never classifies a departure itself.

### Source: named placeholder

Every KPI records where its data will eventually come from, even where that system does not exist
yet.

Ask what the eventual source should be — an HRIS export, an ATS, a finance system, a survey tool,
manual entry. Record the answer. **Until a resolver is wired, every source resolves to manual
entry at run time**, and the config schema does not change when one is wired later.

This exists so that adding a real integration does not require migrating configs that already
carry history.

---

## Thresholds

A threshold is **the user's stated rule, recorded at setup.** The engine compares the reading
against it and states the comparison as fact.

*Allowed:* "Threshold set at 15%. Current reading 17%."
*Never:* "Attrition is elevated and needs attention."

The first is retrieval against a rule the human set. The second is the tool reasoning about
consequence, which is the human's.

Thresholds are optional. A KPI without one simply displays its movement.

---

## Gaps

When a declared KPI has no data for a period, **output it as an explicit gap.** Never skip it
silently, and never interpolate.

- The dashboard shows the KPI with "not reported this period"
- The trend line breaks visibly rather than connecting across the gap
- The history records the period as missing, not as zero

A metric that stops being reported is a finding about instrumentation. Making it invisible removes
the only signal that would surface it.

**Missed periods.** If the declared cadence says a period should exist and no run happened, record
it as missing on the next run. A flat line and an unrun period must not look the same.

---

## Data handling

**Aggregate by default. Individual records may be ingested where a metric requires them; only
aggregates are ever stored or displayed.**

Some metrics genuinely require individual-level data to compute — pay equity and equity-grant
equity being the clearest cases. These are legitimate. The requirement is that handling them is a
conscious act.

### Gating on ingest

When a KPI's sensitivity flag is set, **warn before ingesting**, stating what class of data is
about to be handled and what will be retained.

**The warning fires whether or not the data carries names.** De-identification is not sufficient
protection at small headcount: role, location, level and a protected characteristic will often
re-identify a single person in a team of thirty. Treat de-identified individual records as
individual records.

### What is never ingested

- Health, medical, disability, accommodation, or sick-leave data
- Anything from an active grievance or investigation

If a proposed KPI would require either, say so and stop. Do not offer a workaround.

### What is stored

The history file holds computed aggregates and the period stamp. It does not hold individual
records, and it does not hold the raw input. Raw input is read, computed against, and dropped.

**The history file is intended to be committed.** Its contents must be safe to sit in a
repository, and must be checked against that standard on every write.

---

## Small populations

At small headcount, percentage-based metrics move sharply on single events. One departure from
thirty people is a three-point swing in attrition.

**State the population alongside any percentage.** Where the population is small enough that a
single event produces a large percentage movement, show the underlying count as well as the
percentage.

**Do not smooth, suppress, or annotate the movement as noise.** Whether a swing is meaningful is
the reader's judgment. The tool's job is to make sure they can see what the percentage is built
on. A three-point move on a base of thirty and a three-point move on a base of three hundred are
different facts, and the reader can only tell them apart if the base is visible.

---

## Output

A single self-contained HTML file. No build step, no server, no dependencies. It must open on a
laptop belonging to someone who has never heard of this tool, and survive being emailed.

Written fresh each run. The history file is the record; the HTML is a rendering of it.

### What appears

**Per KPI:**

- Current reading, with unit
- Movement since the previous period, and across the full recorded series
- A trend visualisation, with gaps shown as breaks
- Population base where the KPI is a percentage
- Threshold comparison, stated as fact, where one is set
- Source and last-updated stamp

**Overall:**

- Period covered and cadence
- Population size for the period
- Any missed periods
- Any KPIs discontinued or with a marked break in series

**On the first run**, no KPI has movement. State that once, at the top — "first recorded period,
movement appears from the next run" — rather than printing "no prior period" against every metric,
which reads as a series of failures rather than a baseline.

### Commentary slots

The tool renders **an empty, clearly-marked commentary slot per KPI, and one for the dashboard as
a whole.** It never fills them.

Unfilled slots render visibly as unfilled — the reader can see that commentary was expected and is
absent, rather than the section quietly not appearing.

This exists because the numbers are the tool's and the narrative is the user's. Writing the
narrative is the part that requires knowing the organisation, and a tool that drafts it produces a
user defending reasoning they have not done.

### Audience

The same file serves the user, an exec team, and a board. That means: plain language, no jargon,
no dense tables, readable at a glance, and printable. Assume it will be looked at on a phone
five minutes before a meeting.

---

## Checkability

Every number on the dashboard must be reproducible by hand.

- **The definition travels with the metric.** Someone reading the dashboard can see how the number
  was calculated without opening the config.
- **Source and date are shown per KPI**, not just for the dashboard as a whole.
- **Computed and supplied values are distinguishable.** A number entered manually and a number
  derived from a formula are not the same kind of claim.
- **No number appears without a period stamp.**

If a CFO or a lawyer asked how a figure was arrived at, the answer must be on the page.

---

## What this tool does not do

Stated plainly so the boundary does not erode as features get added:

- **It does not choose your KPIs.** It asks what decisions you are making and helps you pick.
- **It does not interpret movement.** It shows the change and the threshold you set.
- **It does not recommend action.** No "needs attention", no priority ordering, no flags beyond
  the factual threshold comparison.
- **It does not score, rank, or report on named individuals.** Aggregate only, every time.
- **It does not write your commentary.** The slots stay empty until you fill them.

---

## Applying it

| Request | Verdict |
| --- | --- |
| "Set up tracking for our People metrics" | Setup mode |
| "Run this quarter's numbers" | Record mode |
| "Add hiring velocity to what we track" | Amend mode, existing history retained |
| "What KPIs should we be tracking?" | Ask what decisions they need to improve; do not answer |
| "Which of these numbers is concerning?" | Not this tool's job — say so |
| "Show me attrition by manager" | Refuse — individual-level reporting |
| "Track sick leave rates" | Refuse — never-ingest class |
| "Rank the teams by engagement score" | Refuse — ranking, and thinly-disguised individual reporting at small team size |
