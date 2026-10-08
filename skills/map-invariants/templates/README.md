# Invariants

This directory is the map of the product. Each file is a short description followed by statements that hold in every build of it.

- [INTENT.md](INTENT.md): what it's for, and what would make it pointless. Read it first.
- `primitives/`: what it's made of.
- `capabilities/`: what it does with those things.
- `interfaces/`: who or what touches it from outside, and what passes between them.

## Reading the Map

- The layers organize the map, not the code. A primitive is not a data type and a capability is not a service. Hold each rule wherever it holds best.
- Where the map says nothing, the choice is the builder's.
- Bold statements are the ones the product would be a different product without.
- An invariant that says it catches up may be held sooner, never later.
- A build that can't keep an invariant raises it with the product's owner. It never becomes a quiet exception.
- When the product changes, these files change in place.
