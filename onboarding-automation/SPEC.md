# Onboarding Automation Layer: v1 Spec

> Automate the admin. Protect the human moments.

**Status:** Draft v1
**Type:** Portfolio tool (hr-tools), built generic, working integrations
**Scope:** Administrative onboarding only

---

## TL;DR

A new hire record lands in the HRIS. The layer builds that person's onboarding plan from a pre-approved profile, the People team approves it, and the layer runs the admin: accounts, tool invites, device order, calendar holds, paperwork tracking, and compliance clocks.

It never touches sensitive data. It never writes words a new hire reads. It only sends what the People team uploaded and approved.

---

## 1. Why this exists

Most onboarding pain is not strategy. It is the laptop that shipped late, the I-9 nobody chased, the invite sent to an inbox the new hire can't open yet.

That work is repetitive, rules-based, and easy to drop. It is also exactly the work that steals time from the parts of onboarding that actually drive retention: connection, clarity, culture.

This layer takes the admin off the People team's plate without taking the humanity out of a new hire's first week.

---

## 2. Scope

### In scope for v1
- Paperwork **status** tracking (sent / done) pulled from the HRIS or PEO
- Compliance clocks: I-9, E-Verify, benefits enrollment window
- Work account creation and activation checks
- Tool invites and group memberships per profile
- Recurring meeting invites per profile
- Device ordering or device reminders
- Day-one calendar invites (People call, hiring manager call)
- Templated emails to new hires, using People-uploaded templates only
- Escalations and reminders for stalled tasks
- Reacting to HRIS changes (start date, location, rescinded offer)
- A dashboard for People to approve, track, and act

### Out of scope for v1
- Company layer (values, mission, org chart, customers, ways of working)
- Role layer (30/60/90 plans, role tools training, intro meeting series)
- Offboarding
- Any AI-generated content reaching a new hire

---

## 3. Hard rules (not configurable)

These are baked into the product. The setup flow cannot turn them off.

1. **No sensitive data, ever.** SSNs, bank details, tax forms, I-9 documents, and passwords live in the HRIS, PEO, or identity system. The layer only stores status flags (`sent`, `in_progress`, `done`, `overdue`).
2. **No AI-written words reach a new hire.** Every new-hire-facing message comes from a template the People team uploaded. The layer only fills merge fields.
3. **The People team owns every approval.** Hiring managers and IT receive notifications and requests. They do not approve actions.
4. **Compliance clocks use business days, not hours.** Federal holidays are excluded.
5. **Nothing is sent to an account that doesn't exist yet.** The layer confirms the work account is created and active before sending anything to it.
6. **The layer never handles credentials.** Temporary passwords are delivered by the identity system itself (Google Workspace, Okta, etc.). The layer only tracks that delivery happened.

---

## 4. How it works

### 4.1 Trigger
**New hire record created in the HRIS/PEO.** The HRIS stays the single source of truth.

### 4.2 Profiles
A profile is a pre-approved bundle of actions for a type of hire.

- Examples: `Full-time employee`, `Contractor`, `Engineer`, `Sales`, `Remote, Oregon`
- Each profile defines: which templates, which tools, which groups, which recurring meetings, which device path, which paperwork rules
- **Default path:** People picks a profile, reviews the plan, approves the whole bundle in one click
- **Custom profile:** every action is approved individually. Use for the oddballs.

### 4.3 Approval flow
1. HRIS creates the new hire record
2. Layer drafts an onboarding plan (suggested profile based on HRIS fields like employment type and department, People confirms)
3. People reviews in the dashboard
4. Profile hire: one approval releases all actions
5. Custom hire: each action waits for its own approval
6. Actions run in dependency order (see 4.4)

### 4.4 Dependency order
Some actions can't fire until another one is confirmed.

```
Work account created
  └─ Work account active (verified)
       ├─ Tool invites
       ├─ Group memberships
       ├─ Recurring meeting invites
       └─ Day-one calendar invites
```

Device ordering and the pre-start welcome email do **not** depend on the work account. They fire as soon as the plan is approved.

### 4.5 Per-hire states
`Pending approval` → `Pre-start` → `Day one` → `Week one` → `Complete`
Plus flags: `At risk`, `Escalated`, `On hold` (HRIS change pending)

---

## 5. Timeline

All timing is relative to the **start date** (Day 1), except where noted.

| When | Action | Recipient | Notes |
|---|---|---|---|
| Plan approved | Order device (or remind People to order) | IT / vendor / People | Fires immediately, regardless of lead time |
| Plan approved | Create work account | Identity system | Credentials delivered by identity system, not the layer |
| Plan approved | Flag `At risk` if start date is closer than device shipping lead time | People | So People can plan a backup |
| Account active | Tool invites, groups, recurring meetings | Work account | Per profile |
| Account active | Day-one calendar invites | Work account | People call first, then hiring manager call |
| Pre-start (configurable) | Welcome email | **Personal** email | From People's template. Includes work email address, sign-in link, day-one schedule, and "watch for a separate email with your temporary password" |
| Pre-start | Paperwork reminders per stalled-paperwork rules | Personal email / People | Per form |
| Day 1, before first call | First-login check | People | If no first login X minutes before the People call, escalate |
| End of Day 1 | I-9 Section 1 check | People + new hire | Hard rule |
| End of business day 2 | I-9 Section 2 + E-Verify check | People + new hire | Hard rule. One business day of runway before the deadline |
| Configurable | Benefits enrollment window checks | People + new hire | Deadline set in setup flow, typically 30 days |

---

## 6. Compliance clocks

### 6.1 Form I-9 (hard rule)
- **Section 1** (new hire): due by end of first day of work. Not complete by end of Day 1 → escalate to People **and** the new hire.
- **Section 2** (employer): due within 3 business days of the first day of work. Not complete by end of business day 2 → escalate to People **and** the new hire (who may need to present documents).
- **Clock starts at the start date**, not the hire date.
- **Setup check:** does the HRIS report Section 1 and Section 2 separately? If not, the layer falls back to a single I-9 status and escalates on the Section 1 deadline. The setup flow warns the user about this.

### 6.2 E-Verify (if enrolled)
- Cases must be created no later than the third business day after the employee starts work for pay.
- Tracked alongside I-9 Section 2. Not complete by end of business day 2 → escalate to People.
- Setup flow asks: enrolled in E-Verify? (Also required to use the DHS remote I-9 alternative procedure.)

### 6.3 Benefits enrollment
- New hire windows are commonly 30 days from start or eligibility, and a missed window is generally not fixable.
- Setup flow collects the window length and reminder cadence.
- Layer tracks status only (`not started`, `in progress`, `elected`, `waived`). Elections stay in the HRIS/PEO.

### 6.4 State requirements (configurable)
- **State new hire reporting:** usually handled by the payroll provider or PEO. Layer tracks status if the HRIS exposes it.
- **Required state notices:** e.g. Paid Leave Oregon notice to new hires and remote Oregon employees. Setup flow lets the user add required notices per state as templates with delivery tracking.

---

## 7. Stalled paperwork and escalations

Configured per form in the setup flow. I-9 and E-Verify rules are fixed (section 6).

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

## 8. Handling changes

The layer watches the HRIS record for updates after the plan is approved.

| Change | Layer response |
|---|---|
| Start date moves | Hold pending actions, recalculate every date, re-request approval for anything already sent with the old date (calendar invites, welcome email) |
| Location changes | Hold device shipment if not yet shipped, flag state-specific notices and requirements, alert People |
| Offer rescinded / no-start | Cancel all pending actions, flag sent items for People to clean up (accounts, invites, device return) |
| Profile-relevant field changes (department, employment type) | Suggest a profile change, People confirms |

---

## 9. The setup flow (onboarding the tool itself)

Run once when a company adopts the tool. Every answer is editable later.

1. **HRIS / PEO.** Which one? Connect. Check: does it report I-9 sections separately? Does it expose benefits and state reporting status?
2. **Identity and email.** Google Workspace, Microsoft 365, Okta, other. Confirm "force password change at first login" is on. Confirm credential delivery method (identity system's own sign-in email to personal address).
3. **Chat and other tools.** Slack, Teams, project tools, etc. Connect what should receive invites.
4. **Calendar.** Which calendar system sends day-one invites?
5. **Dashboard home.** Where should the dashboard live? (e.g. Google Sheets, standalone web view). The layer does the setup lift for the chosen option at this step.
6. **Templates.** Upload every new-hire-facing message: welcome email, reminders, day-one invite descriptions, escalation messages. Preview with merge fields.
7. **Day-one calls.** Default time, length, and host for the People call and the hiring manager call. Time zone follows the new hire.
8. **Devices.** Choose a path:
   - Fire a device request to IT or a vendor (ticket, email, or form) and track status, **or**
   - Remind People to order by X days before start
   - Set expected shipping lead time (used for the `At risk` flag)
9. **Compliance.** E-Verify enrolled? Benefits window length? States you hire in and their required notices?
10. **Stalled-paperwork rules.** Per form, per section 7.
11. **First-login check.** How many minutes before the first day-one call?
12. **Profiles.** Build profiles from the actions above. Mark which is default.
13. **Approvers.** Who on the People team can approve.

---

## 10. Dashboard

One place for People to see everything.

- **Approvals queue:** plans and individual actions waiting on a decision
- **New hires:** every active hire, their profile, start date, and state
- **Per-hire view:** every action with status, timestamp, and who approved it
- **Escalations:** overdue compliance items first, then stalled paperwork, then `At risk` flags
- **Audit log:** what was sent, when, to whom, from which template, approved by whom

**Never shown in the dashboard:** any sensitive data. Status flags only.

---

## 11. What the layer stores vs. never stores

| Stores | Never stores |
|---|---|
| Name, work email, personal email | SSN, tax IDs, bank details |
| Start date, location (state level), department, employment type | I-9 or ID documents |
| Manager and People contact | Passwords or temporary credentials |
| Profile and action list | Benefits elections |
| Status flags and timestamps | Shipping address (lives in HRIS or vendor form) |
| Template IDs and approval records | Anything in the HRIS beyond what's listed here |

---

## 12. Build notes

- **This needs a backend.** It listens for HRIS events, holds integration credentials, and runs timers when nobody is looking. A browser-only tool can't do that.
- **Integrations are adapters.** Each category (HRIS, identity, chat, calendar, ticketing) gets a common interface so new providers can be added without touching core logic.
- **Business-day math** needs a holiday calendar.
- **Idempotency matters.** If the HRIS sends the same event twice, the layer must not create two accounts or send two welcome emails.
- **Least privilege.** Each integration gets only the permissions its actions need.
- **Data quality is the real risk.** Most provisioning failures trace back to inconsistent fields between HRIS and identity systems (misspelled department, missing manager). The setup flow should validate field mappings before go-live.

---

## 13. Research context

Key findings that shaped this spec:

- **HRIS as trigger** is the standard provisioning pattern. Identity systems (Okta, Rippling, JumpCloud) automate account creation off HRIS events.
- **Zero-touch devices** only work if hardware is bought through business channels (Apple Business Manager, Windows Autopilot). Retail purchases can't be enrolled after the fact.
- **Remote I-9** via the DHS alternative procedure requires E-Verify enrollment, used consistently for all hires at that location.
- **Form I-9 edition:** forms must show the 05/31/2027 expiration date as of August 1, 2026.
- **Off-the-shelf options** exist (Gusto, Justworks, Rippling, Donut, n8n and Zapier templates), but none combine status-only data handling, People-authored messaging, and profile-based approvals in one layer.
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
- [Gusto: Hiring and onboarding](https://gusto.com/product/hr/hiring-onboarding)
- [Donut: Onboarding journeys](https://help.donut.ai/articles/657523-donut-for-onboarding)
- [n8n: Onboarding workflow template](https://n8n.io/workflows/3860-automate-employee-onboarding-with-slack-jira-and-google-workspace-integration/)
- [StrongDM: Onboarding statistics (Brandon Hall / Glassdoor)](https://www.strongdm.com/blog/employee-onboarding-statistics)

---

## 14. Open questions for v2

- Company and role layers: what gets delivered, when, and by whom
- Offboarding as the mirror image (deprovisioning, device return, final paperwork)
- Hiring manager visibility: read-only dashboard view?
- Onboarding feedback survey at day 30 / 90
