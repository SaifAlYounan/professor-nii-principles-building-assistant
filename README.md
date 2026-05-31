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

## Is this real, or just a clever wrapper?

Fair question to ask of anything called an "AI design assistant." Here's the honest answer.

**What's real about it:**

- **It actually reads your answers and reasons from them.** When you describe your job, it's genuinely listening for whether the work breaks into different *kinds* of expertise or is really one judgment repeated. Your answers change what it proposes — it is not a script that prints the same blueprint every time.
- **The first gate has teeth.** A large share of problems people bring *don't* need to be split into a team of specialists — they need one good prompt. The command is built to tell you exactly that and **stop**. A gimmick would invent a council of seven specialists for everything to look impressive. This does the opposite.
- **The output is a genuine shape:** who the specialists are, what each reads and writes on the shared board, the order they run in, and where a human stays in the loop. That's a real blueprint when your problem actually has structure.

**What it is *not* (the limitations, stated plainly):**

- **It produces a blueprint, not a system.** It tells you *what* to build, not *how*. The hard engineering — making each specialist reliable, prompting it, testing it — is untouched when the session ends. The command says this out loud rather than pretending the design is the finish line.
- **It's grounded in one framework** (Nii's blackboard model). That model fits problems shaped like "several semi-independent experts contributing to an evolving shared picture." If your problem isn't shaped that way, the framework adds nothing — and the command's first gate is built to send you back to something simpler rather than force a team you don't need.
- **It's deliberately slow.** One question at a time, waiting for your real answer each turn. If you already know your design, that will feel like overhead. The pace is the point: it's a thinking tool, not a code generator.

The blunt test: feed it a real problem. Either it tells you something non-obvious about whether to split it up, or it says "this is one prompt, go" in about three questions and saves you from over-engineering. Both are wins. It only feels gimmicky if you feed it nothing.

---

## How it works

- **Where it lives:** it's a Claude Code *slash command* — a single Markdown file named `decompose.md` that sits in your Claude Code commands folder. The filename *is* the command name, so `decompose.md` becomes `/decompose`.
- **How to trigger it:** inside Claude Code, type `/decompose` followed by a one-line description of what you want to build:

  ```
  /decompose a system that reviews commercial contracts and flags risky clauses
  ```

  You can also type `/decompose` on its own — it will ask you what you want to build first.
- **What happens then:** it runs an interactive session, **one question at a time**, and waits for your real answer before moving on. It never answers its own questions or batches them. The session can end early (at the very first gate) if your problem doesn't need splitting up — that's a success, not a failure.

---

## Install

A Claude Code command is just a Markdown file in a commands folder. You have two scopes:

- **Personal (available in every project):** `~/.claude/commands/`
- **Project (shared with anyone who clones that project's repo):** `<your-project>/.claude/commands/`

**Option A — one-line download into your personal folder:**

```bash
mkdir -p ~/.claude/commands
curl -fsSL https://raw.githubusercontent.com/SaifAlYounan/professor-nii-principles-building-assistant/main/commands/decompose.md \
  -o ~/.claude/commands/decompose.md
```

**Option B — clone and copy:**

```bash
git clone https://github.com/SaifAlYounan/professor-nii-principles-building-assistant.git
cp professor-nii-principles-building-assistant/commands/decompose.md ~/.claude/commands/decompose.md
```

Then restart Claude Code (or start a new session) and run:

```
/decompose a tool that triages incoming support tickets
```

### "Do people just point Claude Code at the repo?"

**Not for a single command, no.** Claude Code doesn't watch a remote repo and auto-load commands from it. A slash command is loaded from a file on *your* machine — so you copy the one `decompose.md` file into a commands folder (Option A or B above). That's the whole install.

Two things worth knowing:

- **You only need that one file.** The `nii-blackboard-summary.md` and this README are background reading — the command is fully self-contained.
- **The "point at a repo" experience does exist — for *plugins*, not loose commands.** If this were packaged as a Claude Code *plugin* (with a `.claude-plugin/marketplace.json` describing the repo as a marketplace), people could run `/plugin marketplace add SaifAlYounan/professor-nii-principles-building-assistant` and then `/plugin install …` to pull it straight from GitHub. This repo ships as a plain command to keep install to a single copy step; packaging it as a plugin is a possible future upgrade, not a requirement.

---

## What `/decompose` actually does, step by step

You type `/decompose my contract review system` and it walks you through Nii's framework, one question at a time, waiting for your real answers (it never answers for you):

1. **Should you even split this up?** *(the suitability gate)* — Nii's companion paper is about *which problems suit this method at all.* Most tasks are really **one job that wants one good prompt**, not a team of specialists. The command will tell you that plainly and stop — it won't manufacture a committee for a task that doesn't want one.
2. **Name the specialists** — break the work into independent parts (and merge the "parts" that turn out to be one job).
3. **Design the shared board** — the central workspace every specialist reads and writes. *(Nii's HEARSAY-I lesson: if the board can only hold one kind of note, specialists working in any other "vocabulary" are silently locked out.)*
4. **Decide the running order** — *(Nii's jigsaw lesson: a fixed order is usually the right answer, not a downgrade. Cleverness only earns its keep when the order genuinely can't be known in advance.)*
5. **Stress-test independence** — *(Nii's koala-bear lesson: a koala-spotter needn't know botany. If specialist A needs to know how B works internally, they aren't really separable.)*
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

## Source

H. Penny Nii. "The Blackboard Model of Problem Solving and the Evolution of Blackboard Architectures." *AI Magazine* 7(2), Summer 1986. (See also Part 2: "Blackboard Application Systems and a Knowledge Engineering Perspective," *AI Magazine* 7(3), August 1986.)

This project summarizes and operationalizes that work for educational use; all credit for the framework is Nii's.

---

## License

Released under the **MIT License** — free to use, copy, modify, and share. See [`LICENSE`](LICENSE).

Built by **Alexios van der Slikke-Kirillov**, Member of LegalQuants. The Nii framework it operationalizes is the work of H. Penny Nii; this project claims no ownership over it.
