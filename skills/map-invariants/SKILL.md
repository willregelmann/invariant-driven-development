---
name: map-invariants
description: Maps a new product before anything is planned or built, by interviewing the user and writing an invariants/ directory (INTENT.md, then primitives, capabilities and interfaces, each a short file of statements that must always hold) — use when the user says "let's start a new project", "I want to build X", "new project", "map this out", or "write the invariants", or wants to add a primitive, capability or interface to an existing invariants/ directory. For greenfield work only. Stops at the map and does not plan or implement.
tags:
  - invariants
  - intent
  - greenfield
  - design
---

# Map Invariants

Turn an idea into a map: an `invariants/` directory that says what the product is for, what it's made of, what it does and who touches it, as statements that must always hold.

The directory has one job. It must be **enough, on its own, for a capable agent to build a working product of the right shape**, while leaving how it's built open. Two builders given only this directory should produce recognizably the same product, built differently.

It lives with the product for its whole life, not with a feature. This skill stops at the map. Choosing a stack, writing a plan and writing code are separate work, and none of them happen here.

## Quick Reference

| Step | Job | Output |
|------|-----|--------|
| 1 | Listen, and check this is greenfield | A shared picture of the idea |
| 2 | Intent | `invariants/INTENT.md` |
| 3 | Primitives | `invariants/primitives/<NOUN>.md` |
| 4 | Capabilities | `invariants/capabilities/<VERB>.md` |
| 5 | Interfaces | `invariants/interfaces/<NAME>.md` |
| 6 | Walk the map | Gaps closed, links resolve, nothing built-in |
| 7 | Hand off | A short summary, then stop |

Filenames are uppercase with a `.md` extension: a singular noun for a primitive, a verb for a capability, the other side's name for an interface.

Work one layer at a time, in this order. Each layer uses the words the one before it defined. Get the user's agreement on a layer before writing its files, and before starting the next.

## The Layers

| Layer | Answers | Named with | Examples |
|-------|---------|-----------|----------|
| Intent | What is this for, and what would make it pointless? | The product's name | `INTENT.md` |
| Primitives | What is it made of? | A singular noun | `MEMORY.md`, `DECK.md`, `QUESTION.md` |
| Capabilities | What does it do with those things? | A verb | `RECALL.md`, `FORGET.md`, `ANSWER.md` |
| Interfaces | Who or what touches it from outside, and what passes between them? | The name of the other side | `USER.md`, `AGENT.md`, `HOST.md`, `SCHEDULE.md` |

## What Counts as an Invariant

An invariant is a statement about the product that is true at every moment, in every build of it. It passes four tests:

1. **Always.** It holds all the time, not at one step. If it describes a sequence, it's a procedure.
2. **Breakable.** A build could visibly violate it, and someone could point at the violation.
3. **Open.** Two quite different implementations could both satisfy it.
4. **Earned.** A capable builder working from the rest of the map could plausibly get it wrong without being told. What any sensible builder would do anyway is left out.

| Not an invariant | Why | Invariant |
|------------------|-----|-----------|
| "Memories are rows in a SQLite table" | Implementation | "A memory's content is never edited. When it turns out to be wrong, a new memory supersedes it." |
| "Listeners can ask questions" | Feature | "No question is dropped. Every question is answered, put off out loud, or declined out loud." |
| "Recall should be accurate" | Can't be broken visibly | "A memory that is only loosely related is labelled that way. It's never passed off as an answer." |
| "At most three questions a week" | A tuning number | "The user is not nagged. Questions to the user are strictly limited per conversation and per week." |
| "Load the slide, then start the audio" | Procedure | "What's seen and what's heard stay together." |
| "Errors are handled gracefully" | Any builder would say this | "Recall that fails says it failed. An agent is never shown 'nothing relevant' when the mind wasn't searched." |

**Naming a technology.** Name one only when the product must *meet* it: a runtime it has to live inside, a system it has to talk to, a format the user insists on. Never name one the product is merely *built with*. The test: swap it out. If what's left is the same product built differently, it doesn't belong here.

**Numbers.** State the limit, not its value. "Strictly limited per conversation" survives every build. A number is a tuning decision that a build makes and measures.

## Interviewing

The user has the idea. You do the drafting. Interview where guidance is needed, and nowhere else.

- **Propose, don't quiz.** Put a concrete candidate in front of the user and let them correct it. They should be able to confirm rather than compose.
- **Ask what only the user knows:** who it's for, what would make it pointless, what it must work with, what's deliberately out or deferred, and which of two reasonable shapes they want.
- **Ask when two capable builders would choose differently and the user would mind.** If they wouldn't mind, it's implementation. Leave it open and don't ask.
- **Every question carries your best answer as the default.** Use a structured-question tool when one is available, and a short numbered list when not.
- **Batch questions per layer.** One round is the aim.
- **Disagree out loud.** When the user's framing has a gap or two of their ideas collide, say so with the specific case, and offer a way through.
- **A default is not a guess.** A default is something the user sees and confirms. A guess is something written into the map that they never saw. If the user doesn't answer, the statement stays out.
- **Unresolved is a legitimate outcome.** Leave the statement out and carry the question to the hand-off.

## Process

### 1. Listen

Let the user describe what they want. Restate it in two or three sentences so they can correct the picture early.

Check the ground:

- **Source code already here?** This skill maps products that don't exist yet, before any architectural decision. Say so, and ask how to proceed.
- **`invariants/` already here?** Go to [Changing the Map](#changing-the-map).
- **A prototype or earlier attempt?** Read it for what turned out to matter, not for how it worked. Keep the consequence and drop the cause: "empty audio clips deadlock the player" becomes "silence is never ambiguous". A later build is expected to rediscover the details, and that is intended.

Don't research stacks, libraries or architectures. Research into the problem itself is fine when the user asks for it.

### 2. Intent

Draft `invariants/INTENT.md` from what the user said, and ask only for what's missing. It's an elevator pitch: about ten lines of prose with no sections, no lists and no mechanism.

```markdown
# <Name>

<What it is, in a few words>: <the one thing that sets it apart>.

<When it matters>, <Name> <answers | keeps open> one question: **<the question>**

<Why the obvious version of this product is only half the job. Concrete cases in
everyday words.>

<The one idea it's built on, and how the product behaves because of it. Behaviour
in broad strokes, never mechanism.>

If <the specific thing that would make it pointless>, <Name> has failed at the one
thing it exists to do.
```

- The **question** is the one the product exists to keep answering: "how much should I trust this right now?", "did that land?". If you can't find one, the idea isn't sharp yet. Keep talking.
- The **failure sentence** names an outcome someone could observe: "If a lecture would have gone exactly the same with nobody listening". It's the most useful line in the directory, so ask for it directly if the user hasn't given it.
- If the product has no name, propose one.
- `INTENT.md` links to nothing. It's written before the other layers exist and should read on its own.

Show the draft and get agreement before going on.

### 3. Primitives

A primitive is a thing the product is made of, and that would exist in any implementation of it. You'd need the word to explain the product to someone.

Propose the list before writing files: each name, one line on what it is, and why it's its own thing. Also list what you considered and folded in. Expect three to six.

- **Collapse hard.** For every pair, ask if they're different kinds of thing, or the same kind in different circumstances. If what separates them is how they came to be or how they're connected, they're one primitive, and its description says so. An observation, a belief and a concept are all memories. A prepared slide and one sketched ten seconds ago are both slides.
- **Not primitives:** a field of something else, a storage structure, a screen, or anyone who uses the product. A person is an interface. What the product keeps about them is a primitive: the user is an interface, and their mind is a primitive.

Each file:

```markdown
# <Noun>

<What it is, in everyday terms. What it gathers or separates. Which things that look
different are really this same kind of thing.>

## Invariants

- **<One of the few statements the product would be different without.>** <What it means in practice.>
- <Most statements are plain, one per bullet.>
```

Look for: what it belongs to, how one comes to exist and how it ends, what about it never changes, what is only ever added, what is kept and what deleting means, what it never claims, and what it does when it can't do its job.

### 4. Capabilities

A capability is something the product does with its primitives, named with a verb. Propose the list the same way: name, one line, why it's separate.

```markdown
# <Verb>

<What it does, to which [primitives](../primitives/NOUN.md).>

<Where judgment sits and what is fixed: "Deciding X takes judgment, so <who> makes
that call. That Y happens is fixed.">

## Invariants

- ...
```

- **Say where judgment lives.** When a person or a model decides something, name who decides, and then state what is guaranteed whatever they decide. This is what lets a build leave room for judgment without leaving room for silence.
- Look for: what it never does, what stays true if it runs twice, what happens when it fails, what it leaves untouched, when it runs and when it mustn't.
- **A capability can be deferred.** Write `**Deferred.**` under its description and record only the guardrails that are already settled.

### 5. Interfaces

An interface is anyone or anything that touches the product from outside: a person, an agent or model, a runtime it lives in, another system, the clock. A model the product relies on for its own judgment counts: it's on the far side of an interface, and the map says what it may decide and what holds whatever it says. Propose the list, then check it against the capabilities: **something must start each capability, and something must see its result.** A capability nothing reaches means an interface is missing.

```markdown
# <Name>

<Who or what this is, and what it's to the product.>

## <What passes>

| <Tool, moment, state or signal> | <What it returns or does> |
|---|---|

## Invariants

- ...
```

- The table names the surface: the tools an agent gets, the moments a runtime provides, the states a person sees. It gives names and effects. Exact inputs and outputs belong to a later contract, and the file can say so.
- Use as many tables as the surface has parts, or none when prose says it. Group rows by what they're allowed to do, such as reading against changing.
- A row doesn't need a capability behind it. Small acts like listing or opening something live only in the table. When a row needs rules of its own, it has become a capability and gets a file.
- Interfaces are where naming an outside system is right, because the product has to meet it.
- Look for: what this side can and can't reach, what it's always able to ask for, what it's never asked to do or learn, how failure looks from this side, and what stays the same across every instance of it.

### 6. Walk the Map

Read the whole directory once, as a builder who has nothing else.

- **Links.** When a file means another file's subject specifically, its first mention is a relative link, and every link resolves. An everyday use of the same word needs no link. Files written early can point forward now that the later ones exist.
- **Primitives are used.** Each one is read or changed by at least one capability. If not, it isn't a primitive or a capability is missing.
- **Capabilities are reachable.** Each one is started and observed through at least one interface.
- **Intent is carried.** Each promise in `INTENT.md` is held up by at least one invariant, and you can name the invariants that stop the failure sentence from coming true.
- **Nothing collides.** No two invariants contradict. The same rule appearing in two files says the same thing.
- **Nothing is built in.** Search for storage, libraries, algorithms, protocols and file layouts. Apply the swap test to each.
- **The guess test.** Ask: "If this directory were all I had, what would I have to guess that the user would mind me guessing wrong?" Each answer is a missing invariant or a question for the user.

Fix what you can settle and ask about the rest in one round. Adding or removing a file changes a layer's list, so it goes to the user with the questions.

### 7. Hand Off

Report the product in one sentence, the names in each layer, anything deferred, any question left open, and any default you took that the user didn't explicitly confirm. Don't paste the files back.

Then stop. Don't write a plan, pick a stack, scaffold a repository or start building, and don't offer to as part of this skill.

## Changing the Map

When `invariants/` exists, the map is the product's, so a new feature changes the map in place. There is no per-feature file.

1. Read the whole directory first.
2. Say which layer the change lands in. Most features add or change a primitive or a capability. A change to `INTENT.md` means it's becoming a different product, so confirm that explicitly.
3. Propose the change as a list of files added, edited and removed, and get agreement.
4. Edit in place. Files say what is true now, with no history, dates or "previously". When a primitive is replaced, delete its file and fix every link to it.
5. Walk the map again (Step 6). A change to a primitive usually touches the capabilities and interfaces that mention it.

## Writing Style

- Plain, everyday words. Describe the product the way you'd explain it to someone who will never see the code.
- Short declarative sentences in the present tense. "Never", "always", "only", "exactly one".
- One idea per bullet. Bold the few statements in each file the product would be a different product without, and no others.
- Concrete examples in quotes beat abstractions: "is your favorite color still pink?", "wait, why?".
- Use colons and commas for asides, not dashes.
- Files are short: a description and four to ten invariants. A file that keeps growing is usually two things.
- No status, no dates, no rationale for how it will be built, no references to this conversation.

## Common Mistakes

- **Writing a specification.** User stories, acceptance criteria, task lists and phases describe work to do. The map describes what stays true once the work is done.
- **Letting implementation in.** A database, a framework or a wire format in an invariant closes a decision the map exists to leave open.
- **Listing features.** "Supports search" says what's there. An invariant says what must hold about it.
- **Too many primitives.** Separate primitives for things that differ only in origin or circumstance produce a build with needless types. Collapse them.
- **Capabilities with nobody to call them.** Writing interfaces last makes it easy to skip the check that each capability is reachable.
- **Leaving judgment unbounded.** "The agent decides when to ask" with no fixed guarantee beside it gives a build that may never ask, or always ask.
- **Stating the obvious.** Invariants any capable builder would honour anyway bury the ones that matter.
- **Quizzing the user.** A long list of open questions hands the drafting back to them. Draft, then ask about what's left.
- **Guessing instead of asking.** The opposite error: settling something only the user knows because asking felt like friction.
- **Carrying a prototype's lessons across as mechanism.** Keep what turned out to matter. Leave how it was solved.
- **Going past the map.** Suggesting a stack, a schema or a first milestone is out of scope even when it seems helpful.

## Key Principles

- **The map is enough, and no more.** Enough for a capable builder to get the shape right, and open enough that two builders would build it differently.
- **Invariants over instructions.** Say what must stay true and let the builder find the way there. Statements about what holds survive a rewrite. Statements about how to build it don't.
- **Each layer earns the next.** Intent gives the question, primitives give the words, capabilities use the words, interfaces make the capabilities reachable.
- **Same kind of thing.** The most valuable move at the primitive layer is noticing that several things are one.
- **Judgment next to a guarantee.** Wherever someone decides, state what is fixed regardless.
- **Failure is visible.** For every primitive, capability and interface, the map says what the product does when it can't do its job. It never goes quiet.
- **The product owns the map.** It changes when the product changes, in place, and it carries no trace of the features that shaped it.
- **Rediscovery is fine.** A build from this map will relearn details an earlier attempt already knew. That cost is accepted, in exchange for a map that doesn't prescribe.
