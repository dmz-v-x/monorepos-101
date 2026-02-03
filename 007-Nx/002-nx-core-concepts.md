## Nx Core Concepts

This is the **most critical module** in the entire Nx journey.

If you rush commands before this clicks, Nx will feel:
- Magical
- Overengineered
- Confusing

If this module clicks, **every Nx command becomes obvious**.

Goal of this module:

> Build a **clear mental model** of Nx before touching any commands.

---

### 1. What Nx actually is

Let’s start by killing confusion.

### What Nx IS

Nx is:

- A **monorepo orchestration engine**
- A **decision-making system**
- A **graph-based task runner**
- A **build intelligence layer**

Nx answers questions like:

- What is affected by this change?
- What must run?
- What can be skipped?
- In what order should tasks run?
- Can I reuse previous results?

### What Nx is NOT

Nx is NOT:

- ❌ A package manager (pnpm does that)
- ❌ A compiler (TypeScript, Vite, etc. do that)
- ❌ A test runner (Jest, Vitest do that)
- ❌ A CI service
- ❌ A replacement for pnpm

Nx **sits above** your tools.
It coordinates them.

---

### 2. Workspace vs package manager responsibilities

This separation is everything.

### Package manager (pnpm) responsibilities

pnpm handles:

- Installing dependencies
- Linking workspace packages
- Enforcing strict dependency rules
- Running scripts
- Managing node_modules
- Lockfile determinism

pnpm does **execution**.

### Nx responsibilities

Nx handles:

- Understanding project relationships
- Building a project graph
- Deciding what is affected
- Ordering tasks
- Skipping unnecessary work
- Caching results
- Optimizing CI

Nx does **decision-making**.

### Mental model (lock this in)

pnpm = *how to run*  
Nx = *what should run*

---

### 3. Nx project graph

The **project graph** is the heart of Nx.

Nx scans your workspace and builds a graph of:

- All projects (apps + libs)
- Their dependencies
- Their relationships

Each project becomes a **node**.
Each dependency becomes an **edge**.

Example:

    shared-utils → auth-lib → api

Nx knows this automatically.

### Where this information comes from

Nx builds the graph using:

- package.json dependencies
- workspace configuration
- imports (in advanced setups)
- explicit project configuration

You do NOT maintain this graph manually.
Nx derives it.

---

### 4. Dependency graph vs Project graph vs Task graph

This distinction is crucial and often misunderstood.

### Dependency graph

- Package-level relationships
- Who depends on whom
- Example: api → auth-lib → shared-utils
- Derived mostly from package.json

This exists even without Nx.

---

### Project graph (Nx concept)

- Nx’s internal model of all projects
- Includes apps, libs, tools
- Knows project boundaries
- Knows tags, constraints, metadata

Dependency graph is **part of** the project graph.

Think:

Dependency graph = relationships  
Project graph = relationships + structure + intent

---

### Task graph (runtime graph)

This is dynamic.

When you run:

    nx build api

Nx builds a **task graph**:

- Which tasks must run
- In what order
- Which can run in parallel
- Which can be skipped

Example task graph:

    build shared-utils
          ↓
      build auth-lib
          ↓
        build api

This graph exists **per command execution**.

---

### 5. Targets (build, test, lint, etc.)

A **target** is a named task a project can run.

Common targets:

- build
- test
- lint
- serve
- typecheck

Targets live at the **project level**.

Example mental model:

Project: api  
Targets:
- build
- test
- lint

Targets answer:

> “What can this project do?”

They are **intent**, not implementation.

---

### 6. Executors (what actually runs tasks)

Targets don’t run code by themselves.

Executors do.

### Executor = implementation

An executor is:

- The thing that actually runs a target
- A wrapper around real tools

Example:

- build target → uses Vite executor
- test target → uses Vitest executor
- lint target → uses ESLint executor

Executors translate:

Nx task → actual command execution

Nx does not compile or test anything itself.
It **delegates**.

---

### 7. Caching (local vs remote)

Caching is not an optimization.
It’s a **core feature**.

### What Nx caches

Nx caches:

- Build outputs
- Test results
- Lint results

Based on:

- Source files
- Dependencies
- Configuration
- Environment

If inputs are unchanged:
👉 Nx reuses the result.

---

### Local cache

- Stored on your machine
- Speeds up repeated local runs
- Cleared with machine reset

---

### Remote cache

- Shared across team & CI
- CI can reuse dev results
- Devs can reuse CI results

This is a **massive productivity multiplier**.

---

### 8. Affected logic

This is the reason Nx exists.

Affected logic answers:

> “Given a change, what is actually impacted?”

Nx computes affected projects by:

1. Detecting changed files
2. Mapping files → projects
3. Walking the project graph
4. Determining downstream impact

Example:

Change in shared-utils →

Affected:
- shared-utils
- auth-lib
- api

Not affected:
- web (if unrelated)

Nx does this **deterministically**.
No guessing.
No bash scripts.

---

### 9. Why this changes everything

Before Nx:

- Humans decide
- CI guesses
- Everything rebuilds

With Nx:

- Machines decide
- CI is predictable
- Only necessary work runs

This changes:

- CI cost
- Feedback speed
- Developer confidence
- Refactor safety

---

### 10. The core mental model

Nx builds graphs so machines can decide what work matters.

Everything else:
- caching
- affected
- orchestration
- CI speed

comes from that.

---

### One-sentence takeaway

Nx is a graph-based orchestration engine that decides what tasks should run, in what order, and what can be skipped.



