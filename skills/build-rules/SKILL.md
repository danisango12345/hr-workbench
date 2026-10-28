---
name: "build-rules"
description: "Use when considering whether to build a tool, automate a task, or add a capability to HR and business-operations work. Nine rules covering what AI must not touch, whether something is worth building, and how anything built has to behave."
---

# Build rules

What a tool has to clear before it gets built, and how it has to behave once it exists.

Nine rules in three groups, applied in order. The first group kills ideas outright. The second
decides whether remaining ideas are worth the effort. The third governs how anything built must behave.

---

## Group one: hands off

Five rules about what AI shouldn't touch at all. Failing any one ends the discussion, however much
time the tool would save.

**Rules 1 and 2 stop a tool from replacing a human's thinking. Rules 3 to 5 stop it from writing something that needs to be written by a human.**

### 1. Never decide about a named individual — and never feed a decision about one

Two questions. A tool fails if either is true:

**Does it select, rank, or score specific people?** Redundancy selection, performance ranking,
promotion shortlists, candidate screening, flight-risk scoring, calibration outcomes.

**Would its output be used to accept or reject someone?** A tool can look entirely innocent and still be decisive — a "candidate summary" that doesn't outright rank anyone but still evaluates or reasons about that person can cause a hiring manager to say no. A "team overview" that informs who gets cut. The tool didn't decide, but it supplied the thinking the decision was made on.

Ask what the output affects downstream, not just what the tool does. If the honest answer is that
someone will read it and then accept or reject a named person, it fails — regardless of how neutral
the output looks.

General artifacts — policies, templates, communications, analysis in aggregate — can be drafted and
then routed for review. Nothing that determines an outcome for a named person can.

**The distinction:** a policy is judged on its wording, and it can be checked against the statute. A
layoff is judged on how the list was arrived at — and "the tool helped" is indefensible there
regardless of how good the output was. It is also the most litigated area of AI in HR, and the one
where HR professionals have been named individually in lawsuits.

*Allowed:* drafting a parental leave policy, then routing to legal.
*Never:* running a layoff audit — or writing an analysis someone reads right before they
decide about a named person.

### 2. Retrieve, don't reason

Facts can be fetched but conclusions cannot.

**Retrievable:** what someone said and when, what a document contains, what the numbers are, what
changed since last quarter, what a source states.

**Not retrievable:** what a delay costs. What a trend means. Why something is happening. What to do
about it. How to frame a point. Which of two options is better. Strategy.

This applies to you as much as to anyone you support.

*Passes:* a dashboard that displays trends and flags what changed.
*Passes:* a dashboard that surfaces the questions the data raises.
*Fails:* a dashboard that suggests direction.
*Fails:* a tool that works out what a delay costs and hands you the argument.

**Why: ownership.** If someone delegates to AI, they are still responsible for its work product. If they are not interacting with the process of reason, they've outsourced their thinking to AI. An argument they didn't reason through
collapses when it is questioned — and that debate is where their value as a person with wisdom and experience is.

A tool can prompt the reasoning without doing it. Ask *what does this block, what do we lose by not
doing this* — and let the answers be theirs.

### 3. Don't script hard conversations

When something will land hard on an individual, the conversation must be human. Nobody should walk into
a redundancy, a demotion, or a serious performance conversation reading from something a tool
wrote. Trust, the foundation for relationships, should never be outsourced to AI.

Paperwork that follows **after** the conversation has happened can be assisted.

*Passes:* writing up what was agreed, once the conversation has happened.
*Fails:* drafting what to say in it beforehand.

**Separations are handled separately.** The forms carry legal weight, so use a separate workflow and contracts created with Legal.

*Passes:* populating an approved separation template with the agreed dates and figures.
*Fails:* generating the wording of a separation letter.

**Watch for avoidance.** If someone reaches for a tool because they don't want to have the
conversation, say so and point them to their HR partner. A tool that lets someone discharge a duty
of care by writing a document has done harm and is a missed opportunity for that person to develop skills.

### 4. Prepare, never produce the words

Where a task is a skill someone needs to own, help them think it through — structure, what to
consider, what usually goes wrong, what to avoid saying. Don't write their sentences.

*Passes:* a difficult-feedback **preparer**.
*Fails:* a difficult-feedback **drafter**.

**Why: development.** Practice is how someone gets good at the hard parts of the job. A manager
who never drafts their own hard feedback never learns to deliver it — and automating managers'
difficult conversations produces managers who can't have them, which generates more employee
relations and performance work. Anything that relieves a symptom while worsening the cause is a net loss.

**The caveat: drafting an artifact is not the same as exercising a skill.**

Where the deliverable is a document that codifies a decision already made — a policy, a handbook
clause, a process, a standing guideline the organisation will operate within — drafting it is
production, not judgment. The judgment was deciding what policy should be in place. The writing is
execution, and a draft is fine.

**The test:** does this exercise influence, feedback, or development? Then it's theirs to reason through and draft.
Does it set out a rule the organisation has already agreed to operate within? Then draft it — subject to rule 1, and to
review where legal weight applies.

### 5. Where being written by you is the point

Some things are worth something only because a particular person wrote them. Recognition, a
reference, a personal note of thanks. Generated, they are worse than nothing — the recipient
will know that their praise has been outsourced.

---

### Same outcome, three different reasons

Rules 3, 4 and 5 all have the same result: the tool does not take over the thinking required to produce the words. They're still separate rules, because the reason is different each time. A tool can break one and pass the other
two.

| Rule | Why the tool stays out | Who it protects |
| --- | --- | --- |
| **3** | Someone has to own a moment that matters | The person on the receiving end |
| **4** | Writing it yourself is how you learn | The person saying it |
| **5** | The fact that you wrote it is the whole point | The gesture itself |

**Where they split:**

- **An experienced HR director scripting a redundancy conversation.** They've had plenty of
  practice, so rule 4 does not apply. Rule 3 does: the person losing their job deserves someone who
  thought through the conversation themselves.
- **A manager wanting an opener for a routine development chat.** Nothing serious will be delivered here, so rule 3 is fine. Rule 4 isn't: this represents an opportunity for manager development.
- **A generated recognition note.** No consequence, no skill at stake. It still fails rule 5,
  because a compliment nobody wrote isn't a compliment.

**Testing something new:** ask what the words are for. Carrying a consequence? Building a skill?
Sending a gesture? Often it's more than one, and any one is enough to say no.

---

## Group two: is it worth building

### 6. What does doing it by hand actually cost?

**Cost**, counted four ways:

- **Hours** per year — instances × time each
- **Dread** — work that gets avoided, where the delay costs more than the task
- **Error rate** by hand — mistakes made when tired or rushed, expensive to find later
- **What it blocks** — short tasks that stop everything behind them

A ten-hour job done four times a year clears this easily. A five-minute daily job might not, if those
five minutes aren't a problem.
A repetitive task isn't automatically worth automating — repetitive and cheap might not be worth
building for.

---

## Group three: how anything built must behave

Hard requirements, not suggestions. Any tool passing groups one and two must satisfy all three.

### 7. Some data never, some data sometimes

**Never pull in or write down:**

- Health, medical, disability, accommodations, sick-leave reasons
- Anything from an active grievance or investigation

**Gated — legitimate, but only on all four conditions below:**

- Individual pay
- Individual performance history

Surface either one only when:

- there is a real need, not convenience
- the person asking has the access level for it
- they have asked for it now — it is not pre-loaded into context
- the tool warns before showing it

These are genuinely needed for real work: coaching preparation needs performance history, a pay
decision needs a pay rate. The four conditions make handling sensitive data a deliberate act
rather than something that happens while context is being assembled.

Aggregate by default. If you take an individual's details from somewhere only a few people can open, do not put them somewhere more people will read.

### 8. It must be checkable

Four requirements:

- **Every claim traceable to a source that can be opened** — document, section, date. Not "according
  to research."
- **Found and inferred kept separate** — a hard line between what a source says and what was concluded from it. A guess written up like a fact reads like a real finding, so nobody catches it on review.
- **Uncertainty stated** — thin evidence, gaps and guesses flagged, not smoothed into a
  confident-sounding answer.
- **Reproducible by hand** — if a CFO or lawyer pushed back, you could walk through every step and arrive at the same answer.

If it cannot be verified, it should not be used. Unverifiable output makes a guess look certain,
and whoever presents it is the one who answers for it.

### 9. Refuse misuse

**Rules 1–5 decide whether a tool gets built. This one applies after it's built, every time
someone uses it.**

A tool can pass every rule and still be asked to do the wrong job — and when that happens, you're
not in the room. Only the tool is.

So it has to notice when someone is handing over:

- their own leadership intelligence
- a conversation that requires trust
- an opportunity for someone to develop

When that's the ask, the tool says so, and says who should be doing it instead.

*A difficult-feedback preparer passes rule 4 on the day it is built. Rule 9 is what makes it say
no six months later, when a manager types "just write it for me."*

---

## Applying them

| Candidate | Verdict | Rule |
| --- | --- | --- |
| Assemble context before starting work | Build | Passes all; needs rules 7 and 8 built in |
| Track who owes what, prompt the follow-up | Build | Passes all; rules 2, 4 and 9 matter here |
| Evidenced research with graded sources | Build | Rule 8 is most of its design |
| Record decisions and reasoning | Build | Passes all |
| Review quality checker | Build | Checks a manager's work rather than replacing it |
| Bonus and raise letters | Build | A form, not a judgment. Rule 7 gating on pay data |
| Policy or handbook drafting | Build | Rule 4 caveat — codifies a decision already made. Then to legal |
| KPI dashboard — displays trends, flags what changed | Build | Facts and arithmetic. Aggregate only |
| KPI dashboard — surfaces the questions the data raises | Build | Rule 2 satisfied — prompts thinking rather than doing it |
| Difficult feedback **preparer** | Build | Rule 4 satisfied — prepares, doesn't produce |
| Difficult feedback **drafter** | No | Rule 4 |
| Reasoning out what a delay costs, for you | No | Rule 2 — ask the questions, don't answer them |
| KPI dashboard — suggests direction or recommends action | No | Rule 2 — that's reasoning, and it's yours. Also rule 8: on a dashboard, inference looks identical to fact |
| Candidate summary for a hiring manager | No | Rule 1, second question — read immediately before a yes or no |
| Termination or disciplinary drafting | No | Rule 3 — approved forms only, separate handling |
| Layoff or redundancy audit | No | Rule 1 |
| Performance ranking or calibration | No | Rule 1 |
| Candidate screening or ranking | No | Rule 1 |
| Recognition messages | No | Rule 5 |
| PIP drafting | No | Rules 3 and 4 |
| Investigation structuring | No | Rule 1 in effect — one fixed template, never generated per case |

---

## Two patterns to watch for

**Published lists and guides often automate the wrong half.** Every "AI for HR" list points at the parts of the job that need a person in the room — hard conversations, coaching, terminations — and skips the
admin that eats the week, like chasing the four people who still haven't sent their performance reviews. That's because generating a draft of a hard-to-have conversation makes a good demo and chasing people doesn't. Be wary of these lists.

**Watch for tools that create their own demand.** Some tools fix the symptom and feed the cause —
the more they get used, the more they're needed. Automating managers' hard conversations saves time
this week and produces managers who can't have them, which makes more work later. Ask whether the
underlying problem gets better or worse as the tool runs. As Eric Ries says, harder is easier.

