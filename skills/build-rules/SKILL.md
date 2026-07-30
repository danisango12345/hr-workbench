---
name: "build-rules"
description: "Use when considering whether to build a tool, automate a task, or add a capability to HR and business-operations work. Eight rules covering what AI must not touch, whether something is worth building, and how anything built has to behave."
---

# Build rules

The rules any proposed tool must pass before it gets built.

Eight rules in three groups, applied in order. The first group kills ideas outright. The second
decides whether survivors are worth the effort. The third governs how anything built must behave.

---

## Group one: hands off

Four rules about what AI shouldn't touch at all. Failing any one ends the discussion, however much
time the tool would save.

### 1. Never decide about a named individual

General artifacts — policies, templates, communications, analysis in aggregate — can be drafted and
then routed for review. Anything that **selects, ranks, or scores specific people** cannot, ever.

That covers: redundancy selection, performance ranking, promotion shortlists, candidate screening,
flight-risk scoring, calibration outcomes.

**The distinction:** a policy is judged on its wording, and it can be checked against the statute. A
layoff is judged on how the list was arrived at — and "the tool helped" is indefensible there
regardless of how good the output was. It is also the most litigated area of AI in HR, and the one
where HR professionals have been named individually in lawsuits.

*Allowed:* drafting a parental leave policy, then routing to legal.
*Never:* running a layoff audit.

### 2. The encounter stays human — and so does the chain around it

When something lands hard on a person, the conversation and the work leading up to it belong to the
human. Paperwork that follows **after** the human interaction has happened can be assisted.

**Separations are handled separately.** The forms must be legally approved, so use approved wording
or nothing.

**Notice avoidance.** If someone is reaching for a tool because they don't want to have the
conversation, say so and point them to their HR partner. A tool that lets someone discharge a duty
of care by generating a document has done harm, not work.

### 3. Prepare, never produce the words

Where a task is a skill someone needs to own, help them think it through — structure, what to
consider, what usually goes wrong, what to avoid saying. Don't write their sentences.

*Passes:* a difficult-feedback **preparer**.
*Fails:* a difficult-feedback **drafter**.

A manager who never drafts their own hard feedback never learns to give it. And automating
managers' difficult conversations produces managers who can't have them — which generates more
employee relations work. Anything that relieves a symptom while worsening the cause is a net loss.

**This applies to you, not only to the people you support.** Facts can be retrieved: what someone
said, the date they said it, what happened, what a document contains. **Reasoning stays yours** —
what a delay costs, what the consequence of something is, how to frame a point, what to actually
say. A tool that hands over a ready-made argument leaves you defending reasoning you haven't done,
which collapses the moment it's questioned.

**The caveat: drafting an artifact is not the same as exercising a skill.**

Where the deliverable is a document that codifies a decision already made — a policy, a handbook
clause, a process, a standing guideline the organisation will operate within — drafting it is
production, not judgment. The judgment was deciding what the policy should say. The writing is
execution, and a draft is fine.

**The test:** does producing this exercise influence, feedback, development, or reasoning? Then
it's yours. Does it set out a rule the organisation operates within? Then draft it — subject to
rule 1, and to review where legal weight applies.

### 4. Where being written by you is the point

Some things are worth something only because a particular person wrote them. Recognition, a
reference, a personal note of thanks. Generated, they are worse than nothing — the recipient
eventually works out which they got.

---

## Group two: is it worth building

### 5. What does doing it by hand actually cost?

Not frequency. **Cost**, counted four ways:

- **Hours** per year — instances × time each
- **Dread** — work that gets avoided, where the delay costs more than the task
- **Error rate** by hand — mistakes made when tired or rushed, expensive to find later
- **What it blocks** — short tasks that stop everything behind them

A ten-hour job done twice a year clears this easily. A five-minute daily job might not, if those
five minutes aren't a problem.

---

## Group three: how anything built must behave

Not gates — requirements. Any tool passing groups one and two must satisfy all three.

### 6. Data handling

**Never pull in or write down:**

- Health, medical, disability, accommodations, sick-leave reasons
- Anything from an active grievance or investigation

**Gated — legitimate, but surfaced deliberately:**

- Individual pay
- Individual performance history

These are genuinely needed for real work: coaching preparation needs performance history, a pay
decision needs a pay rate. The rule is to **warn before surfacing them**, so handling sensitive data
is a conscious act rather than something that happens incidentally while assembling context.

Aggregate by default. Never carry an individual's details into a document that travels further than
the source did.

### 7. It must be checkable

Four requirements, all of them:

- **Every claim traceable to a source that can be opened** — document, section, date. Not "according
  to research."
- **Found and inferred kept separate** — a hard line between what a source says and what was
  concluded from it. Inference dressed as finding is the dangerous failure, because it survives
  review by reading exactly like evidence.
- **Uncertainty stated** — thin evidence, gaps and guesses flagged, not smoothed into a
  confident-sounding answer.
- **Reproducible by hand** — slowly, if a CFO or a lawyer pushed back.

Output that can't be verified is worse than no output. It converts uncertainty into apparent
confidence, and the person who presents it is the one who has to defend it.

### 8. Refuse misuse

Following from rule 2: notice when a tool is being used to avoid a human responsibility, and say so.
This applies beyond duty of care — anything where the request suggests someone wants the tool to do
the part that requires them.

---

## Applying them

| Candidate | Verdict | Rule |
| --- | --- | --- |
| Assemble context before starting work | Build | Passes all; needs rules 6 and 7 built in |
| Track who owes what, prompt the follow-up | Build | Passes all; rules 3 and 8 matter here |
| Evidenced research with graded sources | Build | Rule 7 is most of its design |
| Record decisions and reasoning | Build | Passes all |
| Review quality checker | Build | Checks a manager's work rather than replacing it |
| Bonus and raise letters | Build | A form, not a judgment. Rule 6 gating on pay data |
| Policy or handbook drafting | Build | Rule 3 caveat — codifies a decision already made. Then to legal |
| Difficult feedback **preparer** | Build | Rule 3 satisfied — prepares, doesn't produce |
| Difficult feedback **drafter** | No | Rule 3 |
| Reasoning out what a delay costs, for you | No | Rule 3 — ask the questions, don't answer them |
| Termination or disciplinary drafting | No | Rule 2 — approved forms only, separate handling |
| Layoff or redundancy audit | No | Rule 1 |
| Performance ranking or calibration | No | Rule 1 |
| Candidate screening or ranking | No | Rule 1 |
| Recognition messages | No | Rule 4 |
| PIP drafting | No | Rules 2 and 3 |
| Investigation structuring | No | Rule 1 in effect — one fixed template, never generated per case |

---

## Two patterns to watch for

**Published lists automate the wrong half.** They target the parts of the job that need a human
present — hard conversations, coaching, termination — and ignore the parts that are pure
administration, like working out who still owes three numbers before a model can be finished.
Document generation is easy to demonstrate; assembly and follow-up are unglamorous and harder to
build. When a proposed tool feels obvious and easy, check whether that's because it's automating the
human half.

**Watch for tools that create their own demand.** Anything that relieves a symptom while worsening
the underlying cause is a net loss however much time it saves in the moment.

