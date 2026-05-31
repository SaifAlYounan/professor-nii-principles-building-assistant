# The Blackboard Model of Problem Solving — A Structured Summary

*Working summary of H. Penny Nii, "The Blackboard Model of Problem Solving and the Evolution of Blackboard Architectures," AI Magazine, Summer 1986 (Part 1), with reference to "Blackboard Application Systems and a Knowledge Engineering Perspective," AI Magazine, August 1986 (Part 2).*

This document is the grounding for the `/decompose` command. Everything the command does is meant to trace back to a specific idea below.

---

## 1. The core thesis — what the Blackboard Model is, and why it matters

The Blackboard Model is a way to **organize many different kinds of knowledge so they can cooperate on a problem that none of them could solve alone.**

Nii frames it through a metaphor: a group of human experts standing around a large blackboard, working together to solve a problem. The blackboard holds the emerging solution. Each expert watches it, and *whenever they personally see a place where their own expertise can contribute, they step up and write on the board.* No expert is "in charge" of the others; no expert talks directly to another. They cooperate entirely through the shared board, and the solution gets built up **incrementally and opportunistically** — a piece at a time, by whoever can contribute next.

Why it matters: the Model is aimed at **ill-structured problems** — problems with noisy or incomplete data, large search spaces, many competing kinds of knowledge, and no single algorithm that solves them end to end. Speech understanding, signal interpretation, and scientific-data interpretation are Nii's canonical examples. For those problems, the win is the *integration of diverse, independent expertise* against one evolving solution. (Part 2 is largely about the flip side of this: deciding *which problems are even suitable* for this method, and which are better served by something simpler.)

Crucially, Nii is careful about what the Model is *not*. It is a **conceptual description** — a sketch of how to organize a solution — **not a recipe for building a program.** That distinction drives the rest of the article (see §7).

---

## 2. The three components and how they work together

Nii decomposes the Model into exactly three parts:

1. **Knowledge sources (the specialists).** The total domain knowledge is split into separate, independent modules. Each one is a specialist responsible for one kind of contribution, and each knows the conditions under which *it* can usefully act.

2. **The blackboard (the shared workspace).** A single global database holding the current state of the solution. It is typically **organized into levels of abstraction** (a hierarchy of vocabularies). Knowledge sources make changes to the blackboard that move the solution forward incrementally. **All communication between specialists happens through the blackboard — never directly between specialists.**

3. **Control (the decision about what happens next).** A mechanism that watches the blackboard for changes and decides which knowledge source gets to act next. Because the specialists are triggered by what appears on the board, the overall behavior is *opportunistic*: the system does whatever is most promising given the current state, rather than following a rigid script baked into the specialists themselves.

**How they interact:** a change appears on the blackboard → one or more specialists notice that the conditions for their contribution are now met → control decides which of those specialists actually runs → that specialist writes its contribution back to the blackboard → which is itself a new change that may trigger other specialists. The loop repeats until the solution is complete. The specialists supply the *knowledge*; the blackboard supplies the *shared state and the channel*; control supplies the *sequencing*.

---

## 3. The partitioning principle (with quotes)

The single idea the whole Model rests on is that **domain knowledge is broken into separate, independent modules.** Nii states this in several places rather than one canonical sentence; these are the clearest statements of it:

> "The domain knowledge needed to solve a problem is partitioned into knowledge sources, which are kept separate and independent."

and, on what that independence means in practice:

> "Communication and interaction among the knowledge sources take place solely through the blackboard."

A third framing reinforces it: each knowledge source is *responsible for knowing the conditions under which it can contribute* — i.e., a specialist owns its own trigger and its own contribution, and nothing else owns it for it.

Read together, these say two distinct things, and both matter: (a) the knowledge is **partitioned** — really divided, not lumped — and (b) the partitions are **independent** — they don't reach into each other; they only meet on the shared board. The `/decompose` command's "real partitioning" and "specialist independence" tests come straight from these two halves.

*(Note: these are the clearest expressions of the principle in the article, not a claim that Nii anointed one definitive sentence. The idea is restated in the metaphor, in the three-component definition, and in the formal model section.)*

---

## 4. The jigsaw puzzle analogy — both versions

**Version 1 — everyone at the board at once (the pure Model).**
A group of people sit around a room with a large blackboard; each holds some oversized jigsaw pieces. Volunteers put their most promising pieces up. Then everyone looks at what's on the board and at their own pieces; anyone who sees where a piece fits goes up and adds it. They don't need to talk, hand pieces around, or coordinate — they just watch the developing picture and contribute when they can. Pieces don't have to be added in any fixed order or even in connected clusters.

*What it teaches:* cooperative, **data-directed, opportunistic** problem solving with communication happening *only* through the shared picture. This is the Model in its idealized, parallel form.

**Version 2 — one aisle / one piece of chalk, with a monitor (control appears).**
Now change one thing: the people can't all reach the board at once — say there's a single aisle, or only one piece of chalk, so **only one person can be at the blackboard at a time.** Suddenly you need a **monitor** to decide who goes up next. That monitor *is* the control component.

*What it teaches — and this is the subtle, important part:* control is introduced **by the constraint of serial access, not because cleverness is inherently better.** And the monitor has a choice. It can run people in a **fixed, predetermined order** (just go down the row), or it can decide each time who is most likely to contribute most. Both are legitimate. The fixed order is not a degraded version — it's often exactly right. Opportunistic scheduling only earns its keep when you genuinely *can't* know the useful order in advance.

The deeper lesson: **the moment you implement the Model serially (which is what almost every real system does), you are forced to make a control decision** — even if that decision is just "run them in this order." Control is a consequence of serial implementation, and "fixed order" is a perfectly good answer to it.

---

## 5. The koala bear example — specialist independence

Nii uses the example of finding koalas in photographs (camouflaged among eucalyptus leaves, with rustling leaves and reflected light making it hard). The point is how *different* the contributing knowledge is: someone who can recognize the shape of a koala, someone who knows koalas live in eucalyptus trees, someone who knows about the lighting or the foliage — each contributes a different piece toward the interpretation.

*What it illustrates:* **a specialist does not need to know how the other specialists do their jobs.** The koala-spotter need not understand botany; the person who recognizes eucalyptus need not understand koala anatomy. Each applies its own knowledge and posts its own conclusion; nobody reaches into anyone else's method. They share **results, not reasoning.**

This is the operational meaning of "independent" from §3. It gives the `/decompose` command its sharpest test: *if specialist A can only do its job by knowing how specialist B does B's job internally, then A and B are not actually separable* — they should be merged, or the split is wrong.

---

## 6. The historical evolution: HEARSAY-I → HEARSAY-II → HASP

**HEARSAY-I** was an early speech-understanding system and an early blackboard system. Its limiting flaw: its **blackboard held information at essentially one level — the word level.** Because the shared workspace only spoke one vocabulary, the system was effectively forced into a single level of representation. Knowledge that naturally lives at *other* levels (the acoustic/phonetic level below words, the syntactic/phrase level above them) had no place on the board to be expressed or shared. Specialists that worked in those other vocabularies couldn't really contribute or talk to each other, because there was nowhere on the board for their kind of information to go. **A single-vocabulary blackboard quietly caps what the whole system can do.**

**HEARSAY-II** fixed exactly this by making the blackboard **multi-leveled** — a hierarchy of levels of abstraction (parametric, segmental, syllabic, word, word-sequence, phrasal). Each level is its own vocabulary, and knowledge sources connect adjacent levels (reading from one, writing to another). Now a specialist working at *any* level had a place to read its inputs and post its outputs, and specialists at different levels could finally cooperate — through the board — without speaking the same vocabulary directly. The multi-level blackboard is the innovation that made the Model genuinely general.

**HASP/SIAP** (ocean-surveillance signal interpretation) carried the architecture further. It applied the multi-level blackboard to a *continuous, ongoing* interpretation task (understanding a situation as it evolves over time), and it pushed hard on **control** — introducing more explicit control knowledge, including the use of *expected events* to drive the system's attention. HASP demonstrated that the approach generalized well beyond speech and that control deserved to be treated as a first-class concern, not an afterthought.

The arc of the story: **the blackboard had to grow up.** It started as a single-vocabulary scratchpad (HEARSAY-I), became a multi-vocabulary hierarchy so diverse knowledge could meet (HEARSAY-II), and then control became an explicit, designed subsystem (HASP).

---

## 7. Mapping to modern multi-agent / multi-specialist AI — and where it breaks down

The mapping is direct and useful:

| Blackboard Model | Modern multi-agent / LLM system |
|---|---|
| Knowledge sources (specialists) | Agents, tools, or specialized prompts |
| Blackboard | Shared memory / context / scratchpad / state object the agents read and write |
| Control | Orchestrator, router, or the code that sequences the agents |
| Opportunistic activation | Dynamic routing / agents triggered by intermediate results |
| Levels of abstraction | The structured fields/sections of shared state, each at its own "vocabulary" |

The Model is a genuinely good lens for designing these systems: it forces you to ask *what the specialists are, what they share, and who decides what runs next* — which are exactly the questions people skip when they wire agents together ad hoc.

**But Nii is explicit about where the analogy stops, and the command must be honest about it too.** The Blackboard *Model* is a **conceptual specification, not a computational one.** Nii's own layering is:

- **Model** — the conceptual organization (these specialists, this shared board, this control). A sketch.
- **Framework** — the engineering structure that makes the Model runnable: how the blackboard is actually represented, how a knowledge source is encoded, how control is implemented and scheduled.
- **System** — the actual running program for a specific problem.

**The gap between Model and System is where almost all the real work lives.** Finishing a clean decomposition tells you the *shape* of the system; it tells you almost nothing about how each specialist is actually built, how its knowledge is represented, how it's tested, how reliable it is, or how the shared state is implemented and kept consistent. The danger of the modern analogy is that drawing the diagram *feels* like having designed the system. It isn't. A decomposition is the beginning of the engineering, not the end of it.

That honesty is the note the `/decompose` command must end on: a Model-level blueprint is a real deliverable, but it sits *above* the layer where the hard part happens.
