## Why Nx Exists

### 1. First: Nx was not created to replace pnpm

This is the most important reset.

Nx does **not** replace:

- pnpm
- npm
- Yarn

Nx assumes you already have a package manager.

pnpm’s job is:

- Install dependencies
- Link packages
- Enforce correctness
- Run scripts

Nx exists because **something else breaks** — not dependency installation.

---

### 2. Why pnpm alone eventually breaks down

pnpm is excellent at what it does:

- Strict dependency resolution
- Fast installs
- Deterministic lockfiles
- Workspace linking
- Filtering commands

But pnpm has a **hard limit**.

pnpm can execute commands, but it **cannot decide**:

- What *should* run
- What *must* run
- What *can be skipped*

At small scale, humans can decide this.
At larger scale, humans become the bottleneck.

This is the crack where Nx enters.

---

### 3. The real problem: deciding what is “affected”

Consider a change:

    packages/shared-utils/src/date.ts

Question:

- Do we rebuild api?
- Do we rebuild web?
- Do we run all tests?
- Do we run only some tests?

With pnpm, you *can* run:

    pnpm --filter api... build

But someone has to **decide** that this is the right filter.

At scale, this decision is:

- Repetitive
- Error-prone
- Inconsistent
- Different per developer

This is not a tooling problem.
This is a **decision problem**.

---

### 4. Humans vs machines deciding “affected”

Let’s be very explicit.

### Human-driven system (pnpm-only)

Humans must:
- Inspect changes
- Reason about dependencies
- Guess impact
- Choose filters
- Hope they are correct

This works when:
- Few packages
- Shallow graphs
- Few people

It fails when:
- Graphs grow
- Teams grow
- CI runs constantly

Humans are bad at consistent decision-making under scale.

---

### Machine-driven system (Nx)

Machines:
- Build a dependency graph
- Track file-level changes
- Compute affected projects
- Decide execution order
- Skip unnecessary work

Nx replaces **human judgment** with **deterministic computation**.

This is the core value.

---

### 5. CI explosion & rebuild waste

Let’s look at what happens without orchestration.

Change one shared file → rebuild everything.

Why?

Because the system does not *know* what is affected.

So CI defaults to safety:

- Build all apps
- Test all packages
- Lint everything

This leads to:

- Slow CI
- High cloud cost
- Developers waiting
- Teams batching changes
- Avoiding refactors

This is called **CI explosion**.

It is not caused by monorepos.
It is caused by lack of orchestration.

---

### 6. Why scripts + filters don’t scale

pnpm gives you tools like:

    pnpm -r run build
    pnpm --filter api... run build

These are powerful primitives.

But at scale, teams start doing:

- Copying filters into scripts
- Copying logic into CI YAML
- Writing conditional bash logic
- Maintaining parallel logic in multiple places

Example smell:

    if changed shared-utils:
      build shared-utils
      build api

This logic:
- Is duplicated
- Drifts over time
- Breaks silently
- Lives outside the system’s knowledge

Scripts + filters scale **execution**, not **decision-making**.

---

### 7. What “orchestration” actually means

Orchestration is often misunderstood.

It does **not** mean:
- Running scripts
- Parallel execution
- Fancy pipelines

Orchestration means:

> A system automatically decides **what tasks to run**, **in what order**, and **what can be skipped**, based on the dependency graph and changes.

Key word:
**Automatically**

This removes:
- Guessing
- Manual filters
- Conditional CI logic
- Tribal knowledge

Nx is an orchestration engine.

---

### 8. Where Nx fits exactly

Nx sits **above** pnpm.

Responsibilities split cleanly:

pnpm:
- Dependency installation
- Linking
- Strict correctness
- Script execution

Nx:
- Project graph
- Task graph
- Affected detection
- Execution ordering
- Caching
- CI optimization

Nx does not replace pnpm.
It **uses pnpm as a foundation**.

---

### 9. Nx vs Turborepo

Both Nx and Turborepo exist for the same reason:

Remove human decision-making from builds and CI.

High-level difference:

- Nx is graph-first and architecture-aware
- Turborepo is pipeline-first and simpler

Nx is better when:
- Dependency graphs are complex
- You want architectural rules
- You want deep affected logic
- You manage large monorepos

Turborepo is better when:
- You want simpler mental models
- You have fewer constraints
- You want fast wins

This comparison will be explored deeply later.
For now, know **why Nx exists**, not why it competes.

---

### 10. The core insight

Nx exists because **humans are bad at deciding what work is affected at scale**.

pnpm executes commands well.
Nx decides *which commands should run*.

That is the entire reason Nx exists.

---

### One-sentence takeaway

Nx exists to replace human guesswork with machine-computed decisions about what needs to be built, tested, or skipped in a monorepo.
