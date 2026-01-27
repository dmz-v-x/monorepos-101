## Introduction to pnpm

## 1. First: what was broken before pnpm?

Before pnpm, most teams used:

- npm
- Yarn (classic)

These tools worked reasonably well for small projects.

But at scale, teams didn’t face small annoyances —
they faced **systemic problems**.

---

## 2. Problem 1 — Massive disk duplication

With npm and Yarn classic:

- Every project installs its own dependencies
- Even if versions are identical

Example:

- 20 projects
- All depend on lodash@4.17.21

Result:

- lodash is stored 20 times on disk

This causes:

- Huge disk usage
- Slow installs
- Slow CI machines
- Wasted storage by design

The system was fundamentally inefficient.

---

## 3. Problem 2 — Hidden dependencies

Hoisted node_modules allow this situation:

    import axios from "axios";

Even if:

- axios is NOT listed in package.json
- It happens to exist somewhere higher in the folder tree

This leads to:

- “Works on my machine”
- CI-only failures
- Production-only bugs

This is one of the most painful problems in the JavaScript ecosystem.

---

## 4. Problem 3 — Non-deterministic installs

Historically, even with lockfiles:

- npm and Yarn could resolve dependencies differently
- Install order could change results
- Transitive dependencies could drift

Outcome:

- Two developers
- Same repo
- Different dependency trees

CI behaving differently from local machines.

Trust in the system erodes.

---

## 5. Problem 4 — Monorepos made everything worse

When monorepos arrived, teams suddenly had:

- Hundreds of packages
- Thousands of dependencies
- Dozens or hundreds of developers

All previous problems multiplied:

- Disk usage exploded
- Installs became extremely slow
- Dependency-related bugs increased dramatically

The ecosystem needed a **new foundation**, not incremental fixes.

---

## 6. pnpm’s core idea 

pnpm started with two simple but powerful ideas.

First question:

Why are we storing and installing the same files over and over again?

Second rule:

A package may only access dependencies it explicitly declares.

These two ideas define pnpm completely.

---

## 7. Plain-English definition

pnpm exists to make dependency management **fast, strict, and deterministic**, especially at monorepo scale.

That is the mission statement.

---

## 8. How pnpm differs in philosophy

Comparison at a high level:

npm / Yarn classic:
- Convenience-first
- Hoisting everywhere
- Duplicate installs
- Hidden dependencies possible
- Bugs fixed after they appear

pnpm:
- Correctness-first
- Strict dependency isolation
- Single global store
- Explicit dependencies only
- Bugs prevented upfront

pnpm prevents entire classes of problems instead of reacting to them later.

---

## 9. Why pnpm fits monorepos perfectly

Monorepos require:

- Clear boundaries
- Deterministic behavior
- High performance
- Safety at scale

pnpm provides all four by default.

This is why modern tools and teams prefer it:

- Nx
- Turborepo
- Large engineering organizations

pnpm aligns with how monorepos actually work.

---

## 10. Senior-level insight

pnpm did not try to be a faster npm.

It redefined how dependency management should work.

That is why:

- It feels different
- It feels strict
- It scales where others struggle

---

## One-sentence takeaway

pnpm exists to eliminate dependency duplication, hidden dependencies, and non-deterministic installs — problems that become critical at monorepo scale.
