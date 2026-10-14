# Onboarding Automation Layer: v1 Spec

> Automate the admin. Protect the human moments.

**Status:** Draft v1.1
**Type:** Portfolio tool (hr-workbench), built generic, working integrations
**Scope:** Administrative onboarding only

---

## TL;DR

A new hire record lands in the HRIS. The layer suggests a stack of pre-built profile layers, the People team approves it, and the layer runs the admin: accounts, tool access, device order, day-one calls, paperwork tracking, compliance clocks, and state registration checks.

It never touches sensitive data. Nothing it drafts reaches a new hire without People's approval. It never acts on a change without People's approval.

**Using an all-in-one platform** (e.g. Rippling, Deel, Gusto, Justworks)? This workflow hands off what the platform does natively, and the layer only fills in the gaps.

---

## 1. Why this exists

Most onboarding pain is not strategy. It is the laptop that shipped late, the I-9 nobody chased, the invite sent to an inbox the new hire can't open yet, the first hire in a new state that nobody registered for.

That work is repetitive, rules-based, and easy to drop. It is also exactly the work that steals time from the parts of onboarding that actually drive retention: connection, clarity, culture.

This layer takes the admin off the People team's plate without taking the humanity out of a new hire's first week.

---

## 2. Scope

### In scope for v1

- Paperwork **status** tracking (sent / done) pulled from the HRIS or PEO
- Compliance clocks: I-9, E-Verify, benefits enrollment window
- State compliance checks for the first hire in a new state
- Work account creation (direct or by request) and activation checks
- Tool access and group memberships per profile stack (direct or by request)
- Recurring meeting invites per profile stack
- Device ordering or device reminders
- Day-one calendar invites (People call, hiring manager call)
- A drafted pre-start to-do list for the welcome email, approved by People (section 5.7)
- Escalations and reminders for stalled tasks
- Reacting to HRIS changes (start date, location, rescinded offer), always with People's approval
- A dashboard for People to approve, track, and act

### Out of scope for v1

- Company layer (values, mission, org chart, customers, ways of working)
- Role layer (30/60/90 plans, role tools training, intro meeting series)
- Offboarding
- Dedicated connectors for separate benefits platforms
- AI-drafted welcome emails or reminders (only the to-do list is drafted)

---

## 3. Hard rules (not configurable)

These are baked into the product. The setup flow cannot turn them off.

1. **No sensitive data, ever.** SSNs, bank details, tax forms, I-9 documents, and passwords live in the HRIS, PEO, or identity system. The layer only stores status flags (`sent`, `in_progress`, `done`, `overdue`).
2. **New-hire-facing words are human, with one exception.** Every new-hire-facing message is written by People or comes from a template People uploaded. The only exception is the **pre-start to-do list** in the welcome email: the layer drafts it from the hire's plan, and People reviews, edits, and approves it before it's included. Nothing AI-drafted reaches a new hire without that approval.
3. **The People team owns every approval.** Hiring managers, IT, and tool owners receive notifications and requests. They do not approve actions.
4. **Changes are never automatic.** The layer never reschedules, cancels, or sends anything in response to an HRIS change without People's approval. Its only automatic response to a change is to pause.
5. **Compliance clocks use business days, not hours.** Federal holidays are excluded.
6. **Nothing is sent to an account that doesn't exist yet.** The layer confirms the work account is created and active before sending anything to it.
7. **The layer never handles credentials.** Temporary passwords are delivered by the identity system itself (Google Workspace, Okta, etc.). The layer only tracks that delivery happened.
8. **Research is not legal advice.** Any compliance research the layer produces is cited, graded by source agreement, and carries a reminder to confirm obligations with counsel.

---

## 4. Profiles: built once, stacked per hire

### 4.1 Building profiles happens in setup mode only

Profiles are built in the setup flow (section 12) and revisited when the company's needs change. **People never builds or edits profiles while onboarding a specific hire.**

Editing a profile changes future hires only. Plans already in flight stay as approved.

### 4.2 Stacking

A hire's plan is one **base layer** plus any number of **add-on layers**.

- **Base layer** (one per employment type: e.g. Full-time, Part-time, Contractor, Intern). Paperwork rules, benefits eligibility, work email, company-wide tools, company-wide pre-reading, device path, day-one calls.
- **Add-on layers** (department, role, location, or anything the team needs). Mostly tools, access levels, and team-specific pre-reading. Location add-ons can carry state-specific notices (e.g. Paid Leave Oregon).

**Pre-reading** is a list of links on each layer (handbook, benefits guide, team docs). It stacks like tools do and feeds the to-do list.

Each layer defines when it applies, using HRIS fields (e.g. employment type = full-time, department = Finance, work state = OR). That's how the layer suggests a stack automatically.

### 4.3 Starter layers

The tool ships with starter base layers (Full-time, Part-time, Contractor, Intern) with sensible defaults, so nobody starts from a blank page. Any layer can be duplicated and edited.

### 4.4 Access conflicts

When two layers in a stack set the same tool to different access levels, the layer **flags People to choose** and spells out the difference:

> **Access conflict: Google Drive**
>
> - *Full-time (base):* **Member**. Can view and edit shared files.
> - *Finance (add-on):* **Admin**. Can manage users, sharing settings, and all files.
>
> Choose one before this action runs.

The action waits until People picks. The choice is logged.

### 4.5 The Custom profile

A Custom profile always exists for hires who don't fit a stack. Every action in a Custom plan is approved individually. **Off-pattern hires go to Custom.** There is no per-hire editing of a stacked plan.

---

## 5. How a hire runs

### 5.1 Trigger

**New hire record created in the HRIS/PEO.** The HRIS stays the single source of truth.

### 5.2 Approval flow

1. HRIS creates the new hire record
2. Layer suggests a stack (base + add-ons) from HRIS fields
3. People confirms the stack or swaps layers. No building, no editing.
4. Layer resolves the stack into a plan and surfaces any access conflicts
5. People resolves conflicts and approves the plan in one click (Custom plans: per action)
6. Actions run in dependency order (5.5)

### 5.3 Provisioning modes

Every tool, including the work email account, uses one of two modes, set per tool in the tool-owner directory (section 12):

- **Direct:** the layer creates the account through the tool's admin API. Requires the company to grant admin access in setup.
- **Request:** the layer emails the tool owner and waits for confirmation.

Most companies will run a mix. Many tools only support direct provisioning on paid plans or through an identity system like Okta, and some can't be automated at all.

### 5.4 The request flow

1. Layer drafts a short, plain-text request to the tool owner: who, which tool, which access level, needed by when. (AI-drafted is fine here: it goes to an internal owner, not the new hire.)
2. People verifies the access level and approves the send
3. Tool owner receives the request, which includes a one-click **Mark done** link
4. Owner confirms:
   - **Clear yes (reply or link)** → item checked off, timestamped, logged
   - **Unclear reply, or a different access level than requested** → flagged for People to confirm before it's checked off
   - **No reply by the configured deadline** → reminder to the owner, then escalation to People

### 5.5 Dependency order

Some actions can't fire until another one is confirmed. In request mode, "confirmed" means the owner's affirmative reply.

```
Work account created (direct, or confirmed by owner)
  └─ Work account active (verified)
       ├─ Tool access (direct or request)
       ├─ Group memberships
       ├─ Recurring meeting invites
       └─ Day-one calendar invites
```

Device ordering, the state compliance check, and the to-do list draft do **not** depend on the work account. They start as soon as the plan is approved.

### 5.6 Per-hire states

`Pending approval` → `Pre-start` → `Day one` → `Week one` → `Complete`
Plus flags: `At risk`, `Escalated`, `State compliance incomplete`, `Access conflict`, `On hold: HRIS change pending`


### 5.7 Welcome email to-do list

People writes the welcome email (from scratch or from their own template). The layer drafts **only the to-do list block** inside it.

**How the draft is built:** the layer reads the hire's resolved plan and lists what the new hire needs to do before or on Day 1, for example:
- Complete paperwork in the HRIS/PEO (I-9 Section 1 by end of Day 1, W-4, direct deposit, emergency contact)
- Enroll in benefits by the deadline
- Watch for the separate email with a temporary password, then sign in to the work account at the sign-in link
- Accept tool invites (from the stack's tools and access levels)
- Read the pre-reading (from the stack's pre-reading links)
- Watch for the laptop (tracking link, if shipped)
- Day-one schedule: People call and hiring manager call, in the new hire's local time

**Rules:**
- The draft only lists tasks and facts from the plan. No greetings, no tone, no filler.
- People reviews, edits, and approves the list before it's included in the email
- Nothing in the list reaches the new hire without that approval
- If the plan changes before the email is sent, the draft is regenerated and needs approval again
---

## 6. Timeline

All timing is relative to the **start date** (Day 1), except where noted.

| When | Action | Recipient | Notes |
| --- | --- | --- | --- |
| Plan approved | Order device (or remind People to order) | IT / vendor / People | Fires immediately, regardless of lead time |
| Plan approved | Flag `At risk` if start date is closer than device shipping lead time | People | So People can plan a backup |
| Plan approved | Create work account (direct) or send request (request mode) | Identity system / account owner | Credentials delivered by identity system, not the layer |
| Plan approved | State compliance check, if new state | People | See section 8. Flags, never blocks. |
| Plan approved | Draft the pre-start to-do list | People | People reviews, edits, and approves (section 5.7) |
| Account active | Tool access, groups, recurring meetings | Work account / tool owners | Per stack, direct or request |
| Account active | Day-one calendar invites | Work account | Scheduled per section 7 |
| Pre-start (configurable) | People sends their welcome email | **Personal** email | People writes the email; the approved to-do list block is included (section 5.7) |
| Pre-start | Paperwork reminders per stalled-paperwork rules | Personal email / People | Per form |
| Day 1, before first call | First-login check | People | No first login X minutes before the People call → escalate |
| End of Day 1 | I-9 Section 1 check | People + new hire | Hard rule |
| End of business day 2 | I-9 Section 2 + E-Verify check | People + new hire | Hard rule. One business day of runway before the deadline |
| Benefits window | Enrollment checks and reminders | People + new hire | See section 9.3 |

---

## 7. Day-one calls

Two calls on Day 1: the **People call** first thing, then the **hiring manager call** right after.

### 7.1 Setup flow asks

- The company's standard workday start time
- Default length of each call
- Who on People hosts by default
- For each host and manager: their time zone, and **"If you're onboarding someone in an earlier time zone (e.g. you're on the West Coast and they're on the East Coast), what's the earliest you'd start a day-one call in your time zone?"**

### 7.2 Scheduling rule (shown upfront in setup, with examples)

1. Aim for the **new hire's local workday start**
2. If that's earlier than the host's earliest acceptable time, schedule at the **host's earliest acceptable time**
3. The hiring manager call follows the People call, using the same logic with the manager's hours
4. Flag People only if the result falls outside the new hire's workday

**Example** (host in Bend sets 7:00am Pacific as their earliest):

- New hire in New York → 7:00 Pacific / **10:00 Eastern**
- New hire in Denver → 8:00 Pacific / **9:00 Mountain**
- New hire in Seattle → **9:00 Pacific**, no compromise needed

The new hire's time zone comes from their HRIS work location. Call times go into the to-do list draft in the new hire's local time.

---

## 8. State compliance

### 8.1 Trigger

A new hire's work location is a state the company isn't yet registered in. This is a **company-level** event and fires once per state, not once per hire.

### 8.2 What the layer researches

- **Business registration:** foreign qualification with the Secretary of State, plus any state or local business license
- **Tax accounts:** state income tax withholding, and local taxes where they apply
- **Insurance:** state unemployment insurance, workers' compensation requirements, state disability and paid leave programs (e.g. Paid Leave Oregon, CA SDI, NY DBL)
- **Employment notices and posters** required for new hires in that state
- **New hire reporting:** agency and deadline

### 8.3 How the research works

- At least **two independent sources** per obligation, with official state agency sites ranked first
- Each item is cited and marked **confirmed** (sources agree) or **check this** (sources conflict, or only one source found)
- A standing banner on every checklist: *"This is a research checklist, not legal advice. Confirm these obligations with counsel before acting."*
- AI-written is fine here. The checklist goes to People, not the new hire.

### 8.4 What People gets

- A state compliance checklist on the dashboard, with a **confirmed with counsel** checkbox per item
- The hire is flagged `State compliance incomplete` until every item is checked
- `At risk` flag if the start date is too close to realistically finish registration before first payroll

**Incomplete state compliance flags the hire. It never blocks onboarding.** Everything else keeps running.

### 8.5 Setup flow asks

- Which states the company is already registered in
- Whether the PEO or payroll provider handles state registrations (many cover unemployment insurance and withholding under their own accounts, which shrinks the list)

---

## 9. Compliance clocks

### 9.1 Form I-9 (hard rule)

- **Section 1** (new hire): due by end of first day of work. Not complete by end of Day 1 → escalate to People **and** the new hire.
- **Section 2** (employer): due within 3 business days of the first day of work. Not complete by end of business day 2 → escalate to People **and** the new hire (who may need to present documents).
- **Clock starts at the start date**, not the hire date.
- **Setup check:** does the HRIS report Section 1 and Section 2 separately? If not, the layer falls back to a single I-9 status and escalates on the Section 1 deadline. The setup flow warns the user about this.

### 9.2 E-Verify (if enrolled)

- Cases must be created no later than the third business day after the employee starts work for pay.
- Tracked alongside I-9 Section 2. Not complete by end of business day 2 → escalate to People.
- Setup flow asks: enrolled in E-Verify? (Also required to use the DHS remote I-9 alternative procedure.)

### 9.3 Benefits enrollment

The layer never sends the enrollment invite, shows plans, or sees elections. That stays in the HRIS, PEO, or benefits platform. It watches the clock, tracks status, and nudges.

**Setup flow asks:**

- **Where does benefits enrollment happen?** HRIS/PEO, a separate benefits platform, or a broker portal
- **When the window opens:** start date, or an eligibility date after a waiting period (e.g. "1st of the month after 30 days")
- **Window length** (commonly 30 days)
- **Reminder rule:** reminder template to the new hire plus a People alert, or People only
- **Escalation point:** e.g. 5 business days before close
- **Eligibility per base layer:** e.g. Full-time eligible, Contractor not

**Per hire:**

1. Window opens → layer confirms the enrollment invite was sent
2. Tracks status: `not started` → `in progress` → `elected` or `waived`. **Waived counts as done.**
3. Reminders fire on the configured cadence
4. Escalation to People before close
5. Window closes incomplete → red flag on the dashboard ("missed window, talk to your broker") and an audit entry. A missed window is generally not fixable.

**Status source:** if enrollment happens in the HRIS/PEO, status is pulled automatically. If it happens in a separate platform or broker portal, v1 uses a **manual checkbox** for People, and the clock, reminders, and escalations still run. Dedicated benefits platform connectors are a v2 item.

The enrollment deadline goes into the to-do list draft in plain terms ("Enroll in benefits by Nov 14").

### 9.4 State requirements per hire

- **State new hire reporting:** usually handled by the payroll provider or PEO. Layer tracks status if the HRIS exposes it.
- **Required state notices:** carried by location add-on layers as templates, with delivery tracking.

---

## 10. Stalled paperwork and escalations

Configured per form in the setup flow. I-9 and E-Verify rules are fixed (section 9).

For every other form, the user picks:

- **Option A:** Alert People only
- **Option B:** Send a pre-approved reminder template to the new hire, plus alert People

And sets:

- Days before/after start to first check
- Reminder cadence and max reminders
- When it escalates from "reminder" to "People needs to step in"

**Default list of forms to configure** (commonly stalled):

- Benefits enrollment
- Tax and banking setup (status only)
- Emergency contact
- IT security training
- Policy and handbook acknowledgments

Repeated non-completion is usually a blocker (access issue, unclear instructions), not negligence. Escalations to People should say so.

---

## 11. Handling changes

The layer watches the HRIS record for updates after the plan is approved. **Every change follows the same four steps:**

1. **Auto-pause.** The layer immediately pauses every pending action for that hire. Pausing is internal: nothing is sent, cancelled, or contacted. This is the only thing it does on its own.
2. **Propose.** It drafts a change plan and shows a before/after.
3. **People approves** the change plan as a whole, or item by item.
4. **Then it acts.** Outbound messages, cancellations, and requests only go out after approval.

The hire shows `On hold: HRIS change pending` until People clears it.

| Change | What the layer proposes (nothing happens until People approves) |
| --- | --- |
| Start date moves | New dates for every pending action; updated calendar invites, and a corrected to-do list for People to approve and resend if the welcome already went out |
| Location changes | Hold or redirect the device shipment if not yet shipped; new state compliance check if it's a new state; updated location add-on layers; new day-one call times |
| Offer rescinded / no-start | Cancel pending actions; revoke created accounts; request device return; notify tool owners |
| Profile-relevant field changes (department, employment type) | A revised stack |

---

## 12. The setup flow (onboarding the tool itself)

Run once when a company adopts the tool. Every answer is editable later. **This is where all profile building happens.**

1. **HRIS / PEO.** Which one? Connect. Check: does it report I-9 sections separately? Does it expose benefits and state reporting status?
2. **Identity and email.** Google Workspace, Microsoft 365, Okta, other. Direct or request mode for the work account. Confirm "force password change at first login" is on. Confirm credential delivery method (identity system's own sign-in email to personal address).
3. **Tool-owner directory.** For each tool: who provisions it, direct or request mode, available access levels, and a **one-line plain-English description of each level** (used in access conflict flags). Request-mode reply deadline and reminder cadence.
4. **Calendar.** Which calendar system sends day-one invites?
5. **Dashboard home.** Where should the dashboard live? (e.g. Google Sheets, standalone web view). The layer does the setup lift for the chosen option at this step.
6. **Templates.** Upload every templated new-hire-facing message: reminders, required state notices, day-one invite descriptions. Optionally, a welcome email template People wrote. The to-do list block is drafted per hire (section 5.7).
7. **Day-one calls.** Standard workday start, call lengths, default hosts, and each host's and manager's earliest acceptable time (section 7). The scheduling rule is shown with examples.
8. **Devices.** Choose a path:
   - Fire a device request to IT or a vendor (ticket, email, or form) and track status, **or**
   - Remind People to order by X days before start
   - Set expected shipping lead time (used for the `At risk` flag)
9. **Compliance.** E-Verify enrolled? Benefits setup (section 9.3)? Which states is the company already registered in? Does the PEO or payroll provider handle state registrations?
10. **Stalled-paperwork rules.** Per form, per section 10.
11. **First-login check.** How many minutes before the first day-one call?
12. **Profile builder.** Start from the starter base layers or from scratch. For each layer:
    1. Name it and set when it applies (HRIS fields)
    2. Pick paperwork rules and benefits eligibility (base layers)
    3. Pick tools and access levels from the tool-owner directory
    4. Pick groups, recurring meetings, and pre-reading links
    5. Pick the device path (base layers)
    6. Use default day-one call settings or override them
    7. Preview the full plan for a sample hire, then save
13. **Approvers.** Who on the People team can approve.

---

## 13. Dashboard

One place for People to see everything.

- **Approvals queue:** plans, Custom actions, access conflicts, outgoing tool requests, and change plans waiting on a decision
- **New hires:** every active hire, their stack, start date, and state
- **Per-hire view:** every action with status, mode (direct or request), timestamp, and who approved it
- **Escalations:** overdue compliance items first, then stalled paperwork, unanswered tool requests, and `At risk` flags
- **State compliance:** open checklists per state, with sources and counsel checkboxes
- **Audit log:** what was sent, when, to whom, from which template, approved by whom

**Never shown in the dashboard:** any sensitive data. Status flags only.

---

## 14. What the layer stores vs. never stores

| Stores | Never stores |
| --- | --- |
| Name, work email, personal email | SSN, tax IDs, bank details |
| Start date, location (state level), time zone, department, employment type | I-9 or ID documents |
| Manager and People contact | Passwords or temporary credentials |
| Profile stack and action list | Benefits elections |
| Tool-owner directory and access level descriptions | Shipping address (lives in HRIS or vendor form) |
| Status flags, timestamps, approval records | Anything in the HRIS beyond what's listed here |
| State compliance checklists with sources | |

---

## 15. Build notes

- **This needs a backend.** It listens for HRIS events, holds integration credentials, and runs timers when nobody is looking. A browser-only tool can't do that.
- **Integrations are adapters.** Each category (HRIS, identity, chat, calendar, ticketing, benefits) gets a common interface so new providers can be added without touching core logic. Direct and request mode are two implementations of the same provisioning interface.
- **Reply parsing must be conservative.** In request mode, only a clear yes or the Mark done link checks an item off. Anything ambiguous goes to People.
- **Business-day math** needs a holiday calendar.
- **Time zone math** needs the new hire's work location and every host's and manager's time zone.
- **Idempotency matters.** If the HRIS sends the same event twice, the layer must not create two accounts or send two requests.
- **Least privilege.** Each integration gets only the permissions its actions need.
- **Data quality is the real risk.** Most provisioning failures trace back to inconsistent fields between HRIS and identity systems (misspelled department, missing manager). The setup flow should validate field mappings before go-live.

---

## 16. Research context

Key findings that shaped this spec:

- **HRIS as trigger** is the standard provisioning pattern. Identity systems (Okta, Rippling, JumpCloud) automate account creation off HRIS events.
- **Layered templates** are a proven pattern: GitLab stacks country, entity, department, and role-specific tasks onto a base onboarding template.
- **Zero-touch devices** only work if hardware is bought through business channels (Apple Business Manager, Windows Autopilot). Retail purchases can't be enrolled after the fact.
- **Remote I-9** via the DHS alternative procedure requires E-Verify enrollment, used consistently for all hires at that location.
- **Form I-9 edition:** forms must show the 05/31/2027 expiration date as of August 1, 2026.
- **Off-the-shelf options** exist (Gusto, Justworks, Rippling, Donut, n8n and Zapier templates), but none combine status-only data handling, People-authored messaging, stacked profiles, and approval-gated changes in one layer.
- **Stat hygiene:** the widely cited "82% retention, 70% productivity" figures come from a Brandon Hall Group study for Glassdoor. Recent vendor content misattributes them to a 2026 Gartner study. Cite the original.

### Sources

- [Interlaced: SCIM provisioning for new hires](https://interlaced.io/blog/scim-provisioning-new-hire-automation/)
- [Okta: HRIS integrations](https://www.okta.com/human-resources-information-systems/)
- [Macktez: Zero-touch provisioning](https://macktez.com/glossary/zero-touch)
- [Netchex: Remote I-9 verification 2026](https://netchex.com/blog/form-i-9-remote-verification-what-employers-need-to-know-in-2026/)
- [Engage PEO: Form I-9 expiration date](https://www.engagepeo.com/blog/form-i-9-expiration-date-what-employers-need-to-know-before-august-1-2026/)
- [E-Verify: Create a case](https://www.e-verify.gov/e-verify-user-manual-20-initial-verification/22-create-a-case)
- [Paychex: New hire enrollment period](https://paychex.com/articles/employee-benefits/new-hire-open-enrollment)
- [FirstHR: Benefits enrollment](https://firsthr.app/blog/benefits/benefits-enrollment)
- [Oregon Admin Code 137-055-4040: New hire reporting](https://www.law.cornell.edu/regulations/oregon/Or-Admin-Code-SS-137-055-4040)
- [The Larkin Company: Oregon notice requirements](https://thelarkincompany.com/wp-content/uploads/2025/01/Oregon-Employer-Notice-Requirements-250127.pdf)
- [HubSpot: Pre-arrival tasks](https://employeeonboarding.hubspot.com/americas-pre-arrival-tasks-0?hsLang=en)
- [HR Cloud: New hire checklist](https://www.hrcloud.com/hubfs/NewHireChecklist.pdf)
- [Hyring: Onboarding reminder email](https://hyring.com/free-hr-toolkit/hr-email-templates/onboarding-checklist-reminder-email)
- [GitLab Handbook: Onboarding](https://handbook.gitlab.com/handbook/people-group/general-onboarding/)
- [GitLab Handbook: Employment issue templates](https://handbook.gitlab.com/handbook/people-group/engineering/employment-issues)
- [Gusto: Hiring and onboarding](https://gusto.com/product/hr/hiring-onboarding)
- [Donut: Onboarding journeys](https://help.donut.ai/articles/657523-donut-for-onboarding)
- [n8n: Onboarding workflow template](https://n8n.io/workflows/3860-automate-employee-onboarding-with-slack-jira-and-google-workspace-integration/)
- [StrongDM: Onboarding statistics (Brandon Hall / Glassdoor)](https://www.strongdm.com/blog/employee-onboarding-statistics)

---

## 17. Open questions for v2

- Company and role layers: what gets delivered, when, and by whom
- Offboarding as the mirror image (deprovisioning, device return, final paperwork)
- Dedicated connectors for separate benefits platforms and broker portals
- Hiring manager visibility: read-only dashboard view?
- Onboarding feedback survey at day 30 / 90
