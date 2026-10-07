# Invariant-Driven Development

[![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

An agent skill for starting a new product with a map: what it's for, what it's made of, what it does and who touches it, written as statements that must always hold.

The map is a `docs/invariants/` directory. It's meant to be enough, on its own, for a capable agent to build a working product of the right shape, while leaving how it's built open. It lives with the product, not with a feature.

## Skills

| Skill | What it does |
|-------|-------------|
| [map-invariants](skills/map-invariants/SKILL.md) | Interviews you about a new product and writes `docs/invariants/`, one layer at a time, then stops |

## Install

This repository is a Claude Code plugin and its own single-plugin marketplace:

```
/plugin marketplace add willregelmann/invariant-driven-development
/plugin install invariant-driven-development@invariant-driven-development
```

The skill activates when you say something like "let's start a new project, I want to build X", or you can invoke it by name.

## The Map

```
docs/invariants/
  README.md              how to read the map, copied from a standard template
  INTENT.md              what it's for, and what would make it pointless
  primitives/<NOUN>.md   what it's made of
  capabilities/<VERB>.md what it does with those things
  interfaces/<NAME>.md   who or what touches it from outside
```

Each file is a short description followed by a list of invariants. An invariant is true at every moment, in every build of the product:

| Not an invariant | Invariant |
|------------------|-----------|
| "Memories are rows in a SQLite table" | "A memory's content is never edited. When it turns out to be wrong, a new memory supersedes it." |
| "Listeners can ask questions" | "No question is dropped. Every question is answered, put off out loud, or declined out loud." |

The skill drafts and you correct. It asks only what you alone know: who it's for, what would make it pointless, what it must work with, and which of two reasonable shapes you want.

## What It Doesn't Do

It stops at the map. Choosing a stack, writing a plan and writing code are separate work. It's for products that don't exist yet, before any architectural decision has been made.

## Structure

```
.claude-plugin/
  plugin.json        # plugin manifest
  marketplace.json   # single-plugin marketplace, so the repo installs itself
skills/
  map-invariants/
    SKILL.md
    templates/README.md  # copied unchanged into every map
```

No build system, no dependencies. The artifacts are markdown files.

## License

MIT
