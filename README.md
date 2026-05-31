# Professor Nii's Principles Building Assistant

A Claude Code command that helps you design an AI system **the right way, before you build it** — grounded in H. Penny Nii's 1986 *AI Magazine* article, *"The Blackboard Model of Problem Solving and the Evolution of Blackboard Architectures."*

Most people wiring together "AI agents" skip the only questions that matter: *Should this even be several specialists? What do they share? Who decides what runs next?* Forty years ago, Nii answered those questions for a whole generation of systems. This tool turns her framework into an interactive design session — in plain language, for people who are experts in their own field but not in computer science.

---

## What's in here

| File | What it is |
|---|---|
| [`commands/decompose.md`](commands/decompose.md) | The `/decompose` command — an interactive, one-question-at-a-time design walkthrough. |
| [`nii-blackboard-summary.md`](nii-blackboard-summary.md) | A structured summary of Nii's article — the grounding the command is built on. |

---

## What `/decompose` actually does

You type `/decompose my contract review system` and it walks you through Nii's framework, one question at a time, waiting for your real answers (it never answers for you):

1. **Should you even split this up?** *(the suitability gate)* — Nii's companion paper is about *which problems suit this method at all.* Most tasks are really **one job that wants one good prompt**, not a team of specialists. The command will tell you that plainly and stop — it won't manufacture a committee for a task that doesn't want one.
2. **Name the specialists** — break the work into independent parts (and merge the "parts" that turn out to be one job).
3. **Design the shared board** — the central workspace every specialist reads and writes. *(Nii's HEARSAY-I lesson: if the board can only hold one kind of note, specialists working in any other "vocabulary" are silently locked out.)*
4. **Decide the running order** — *(Nii's jigsaw lesson: a fixed order is usually the right answer, not a downgrade. Cleverness only earns its keep when the order genuinely can't be known in advance.)*
5. **Stress-test independence** — *(Nii's koala-bear lesson: a koala-spotter needn't know botany. If specialist A needs to know *how* B works internally, they aren't really separable.)*
6. **Keep humans in the loop**, then
7. **Get a buildable blueprint** — and an honest statement that the blueprint is the *shape* of the system, not the system. The hard engineering lives one level below it.

### The Nii ideas it's built on

- **Three components** — specialists (knowledge sources), a shared board (blackboard), and control (who runs next).
- **The partitioning principle** — *"The domain knowledge needed to solve a problem is partitioned into knowledge sources, which are kept separate and independent."*
- **The jigsaw puzzle** — control appears because of *serial* access, and a fixed running order is a perfectly good answer.
- **The koala bear** — specialists share *results, not reasoning*; none needs to know how another does its job.
- **The HEARSAY-I failure** — a single-vocabulary board caps the whole system; the board must carry every level the specialists work at.
- **Model → Framework → System** — the decomposition is a *Model-level sketch*; building, representing, and testing each specialist is where the real work lives.

---

## Install

The command lives in your personal Claude Code commands folder:

```bash
mkdir -p ~/.claude/commands
curl -fsSL https://raw.githubusercontent.com/SaifAlYounan/professor-nii-principles-building-assistant/main/commands/decompose.md \
  -o ~/.claude/commands/decompose.md
```

Or just clone and copy:

```bash
git clone https://github.com/SaifAlYounan/professor-nii-principles-building-assistant.git
cp professor-nii-principles-building-assistant/commands/decompose.md ~/.claude/commands/decompose.md
```

Then, in Claude Code:

```
/decompose my contract review system
```

---

## Source

H. Penny Nii. "The Blackboard Model of Problem Solving and the Evolution of Blackboard Architectures." *AI Magazine* 7(2), Summer 1986. (See also Part 2: "Blackboard Application Systems and a Knowledge Engineering Perspective," *AI Magazine* 7(3), August 1986.)

This project summarizes and operationalizes that work for educational use; all credit for the framework is Nii's.
