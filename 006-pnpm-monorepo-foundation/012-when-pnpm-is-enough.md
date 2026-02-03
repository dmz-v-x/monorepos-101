## When pnpm is enough



### 1. Small-to-medium monorepos

### What this module is really about

This module answers one senior-level question:

Do we really need Nx / Turborepo yet — or is pnpm enough?

A good engineer must be able to confidently say:

- “Yes, pnpm is enough”
- OR “No, now we need orchestration”

---

### What “small-to-medium monorepo” actually means

This is **not** about company size.
It is about **code complexity and coordination cost**.

Typical characteristics:

- 2–10 packages total
- 1–3 applications
- A few shared libraries
- Shallow dependency graph
- One team (or tightly aligned teams)

Example structure:

    repo/
      apps/
        api/
        web/
      packages/
        shared-utils/
        auth-lib/

If you can explain the whole repo and build flow in under 2 minutes,
it is still small-to-medium.

---

### Problems at this size

At this stage you may have:

- Some shared code
- Some local linking
- Basic CI
- Simple scripts

But you do NOT yet have:

- CI explosion
- Massive rebuild times
- Complex dependency chains
- Multiple teams stepping on each other

The problems are still manageable.

---

### What pnpm already gives you

With pnpm alone, you already have:

- Workspaces
- Local package linking
- workspace:* (no version skew)
- Strict dependency correctness
- Deterministic installs
- Filtering (--filter)
- Recursive execution (-r)

For this scale, this is enough.

---

### Why adding tools too early is harmful

Adding Nx/Turborepo too early introduces:

- More configuration
- More concepts
- More mental overhead
- More debugging surface

Without real pain, cost > benefit.

This is called **premature complexity**.

---

### Senior rule of thumb

If you can clearly explain every build and script **without diagrams**,
you don’t need orchestration yet.

---

### One-sentence takeaway

For small-to-medium monorepos, pnpm alone provides enough structure, correctness, and speed.

---

### 2. Shared libraries + few services

### What this setup looks like

Very common real-world setup:

    repo/
      apps/
        api/
        worker/
      packages/
        shared-utils/
        auth-lib/
        db-lib/

Usually:

- 1–3 services
- 2–5 shared libraries

---

### Dependency graph shape

Graph is shallow and easy to reason about:

    shared-utils
          ↑
        auth-lib
          ↑
          api

This matters because humans can still reason about impact.

---

### Why pnpm fits perfectly here

pnpm already gives:

- Local package linking
- Strict boundaries
- Filtering
- Recursive execution

Example workflows:

Build API + dependencies:

    pnpm --filter api... run build

Test everything:

    pnpm -r run test

Test shared library + consumers:

    pnpm --filter ...shared-utils run test

Nothing more is needed yet.

---

### Common mistake

“Big companies use Nx, so we should too”

This is not a technical reason.

---

### One-sentence takeaway

Shared libraries + few services still fit perfectly within pnpm’s capabilities.

---

### 3. CI simplicity

### What “simple CI” means

Simple CI means:

- One pipeline
- Clear steps
- Predictable behavior
- Easy debugging

Not “small” — **simple**.

---

### Typical CI at this stage

    pnpm install
    pnpm -r run lint
    pnpm -r run test
    pnpm --filter api... run build

Readable.
Obvious.
Stable.

---

### Why pnpm keeps CI simple

pnpm provides:

- Deterministic installs
- Fast installs
- Strict dependency correctness
- Workspace awareness
- Filtering

So CI does not need clever logic.

---

### Senior rule

Optimize CI only when CI becomes the bottleneck — not when you fear it might.

---

### One-sentence takeaway

pnpm keeps CI simple longer than most teams expect.

---

### 4. pnpm + basic scripts

### What “basic scripts” means

Plain package.json scripts:

    {
      "scripts": {
        "build": "tsc",
        "test": "vitest",
        "lint": "eslint ."
      }
    }

No extra tooling.

---

### Why scripts are powerful with pnpm

pnpm allows scripts to be:

- Recursive
- Filtered
- Dependency-aware

This turns simple scripts into monorepo-ready workflows.

---

### Common workflows

Build everything:

    pnpm -r run build

Build one app + dependencies:

    pnpm --filter api... run build

Lint everything:

    pnpm -r run lint

---

### When scripts start hurting

Scripts start to struggle when:

- Filters are repeated everywhere
- CI logic grows conditional
- Developers forget what to run
- Builds are slow even with filtering

These are signals — not failures.

---

### One-sentence takeaway

pnpm + basic scripts can take you surprisingly far before orchestration is needed.

---

### 5. When pnpm starts to struggle

### Important mindset

pnpm is not failing.
The system is outgrowing human reasoning.

---

### Core problem

Humans become the bottleneck.

People must decide:

- What to build
- What to test
- What is affected

And they guess incorrectly.

---

### Clear signals

1. Builds/tests feel slow even with filtering
2. CI scripts become conditional monsters
3. “What is affected?” is asked frequently
4. Filter logic is duplicated everywhere
5. CI cost or time becomes painful

pnpm is not designed to solve this layer.

---

### What pnpm does NOT do

pnpm does not:

- Track file-level changes
- Compute affected projects
- Cache task outputs
- Skip work automatically

These are orchestration concerns.

---

### One-sentence takeaway

pnpm struggles when humans must manually decide what work is affected.

---

### 6. Signs you need orchestration

### What orchestration actually means

Orchestration means:

The tool decides what runs, in what order, and what is skipped.

Automatically.

---

### Concrete signs

- Affected logic appears everywhere
- CI pipelines are full of conditionals
- Build/test times are unpredictable
- Developers avoid running tests locally
- CI cost becomes a management concern

These are **hard signals**, not opinions.

---

### What orchestration provides

- Dependency graph awareness
- Affected detection
- Task caching (local + remote)
- Ordered execution
- Predictable pipelines

pnpm alone cannot do this.

---

### One-sentence takeaway

You need orchestration when execution decisions become complex and error-prone for humans.

---

### 7. Avoiding premature complexity

### What premature complexity is

Adding tools before the problems exist.

This is speculation, not engineering.

---

### Hidden cost of early orchestration

- Extra configuration
- Learning curve
- Debugging tool behavior
- Tool lock-in

Without pain, these costs dominate.

---

### How future-proof correctly

- Keep structure clean
- Use local packages correctly
- Maintain strict dependencies
- Use simple scripts
- Avoid hacks

This makes later adoption easy.

---

### Replaceability principle

Always ask:

“Can we remove this tool easily later?”

pnpm + scripts → easy to remove  
Heavy orchestration → harder to undo



