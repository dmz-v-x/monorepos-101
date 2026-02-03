## Nx Workspace Anatomy  

### 1. What a “workspace” means in Nx

In Nx, a **workspace** means:

> One repository that Nx understands as a single system.

It does **not** mean:
- One app
- One package
- One deployment unit

It means:
- Nx can see **all projects**
- Nx can see **how they relate**
- Nx can make **global decisions**

When we say *Nx workspace*, we mean:
> “The full graph Nx operates on”

---

### 2. The three layers of an Nx workspace

An Nx workspace has **three conceptual layers**.

### Layer 1 — Package manager layer (pnpm / npm / yarn)

Responsible for:
- Installing dependencies
- Linking local packages
- Running scripts

Files involved:
- package.json
- pnpm-lock.yaml
- node_modules/
- pnpm-workspace.yaml

Nx **does not replace** this layer.

---

### Layer 2 — Nx structure layer (this module)

Responsible for:
- Defining projects
- Defining targets
- Defining boundaries
- Defining metadata

Files involved:
- nx.json
- project.json
- apps/ and libs/ folders

This is where Nx actually begins.

---

### Layer 3 — Execution layer (later modules)

Responsible for:
- Running builds
- Running tests
- Running lint
- Caching outputs

This comes later (Modules 4–5).

---

### 3. Standard Nx folder layout

Typical Nx workspace:

    repo/
      apps/
        api/
        web/
      libs/
        auth/
        shared/
      nx.json
      package.json
      pnpm-workspace.yaml

Why Nx prefers this layout:

- apps → deployable units
- libs → reusable code
- Clear separation of intent
- Cleaner dependency graph
- Easier rule enforcement

This is a **convention**, not a rule — but it scales best.

---

### 4. What is a “project” in Nx?

In Nx, a **project** is:

> Any logical unit that Nx can build, test, lint, or reason about.

A project can be:
- An app
- A library
- A backend service
- A frontend app
- A utility package

Rule to remember:

> If Nx knows about it, it is a project.

Nx does **not care** if it is published or deployed.

---

### 5. How Nx detects projects

Nx detects projects through **explicit configuration**.

### Option 1 — project.json (recommended)

    apps/api/
      project.json
      src/

This file explicitly declares:
- Project name
- Project type
- Targets

---

### Option 2 — package.json (legacy / compatibility)

Nx can infer a project if:
- A folder has package.json
- Scripts exist

Problems with this approach:
- Less explicit
- Harder to scale
- Harder to enforce architecture

Senior teams prefer **project.json**.

---

### 6. project.json — the heart of a project

Minimal example:

    {
      "name": "api",
      "projectType": "application",
      "targets": {
        "build": {
          "executor": "@nx/node:build"
        },
        "test": {
          "executor": "@nx/jest:jest"
        }
      }
    }

This file answers:
- What is this project?
- What can it do?
- How does Nx run those tasks?

Nx reads **this file only** to understand the project.

---

### 7. Targets — what a project can do

A **target** is:

> A named task that can be run on a project.

Common targets:
- build
- test
- lint
- serve

Targets describe *intent*, not execution.

---

### 8. Executors — how targets actually run

An **executor** is:

> The implementation behind a target.

Examples:
- @nx/node:build
- @nx/js:tsc
- @nx/jest:jest

Mental model:
- Target = what
- Executor = how

Nx itself does not build or test — executors do.

---

### 9. nx.json — workspace-level brain

nx.json controls **global Nx behavior**.

Example (simplified):

    {
      "affected": {
        "defaultBase": "main"
      },
      "tasksRunnerOptions": {
        "default": {
          "runner": "nx/tasks-runners/default"
        }
      }
    }

nx.json defines:
- Affected logic defaults
- Caching behavior
- Target defaults
- Workspace-wide rules

Think of it as:
> “The brain of the workspace”

---

### 10. Apps vs Libs — intent matters more than folders

Apps:
- Deployed
- Entry points
- Can depend on libs

Libs:
- Reusable
- Not deployed
- Should not depend on apps

Nx can enforce this separation later.

---

### 11. Tags — architectural rules

Projects can have tags:

    "tags": ["scope:auth", "type:lib"]

Tags allow:
- Dependency constraints
- Team ownership
- Architecture rules

Tags encode **intent**, not behavior.

---

### 12. Senior-level insight

Nx does not care about files.

Nx cares about:
- Projects
- Targets
- Graph relationships

If you understand this:
- Debugging Nx becomes easy
- Refactoring becomes safe
- CI becomes predictable

---

## One-sentence takeaway

An Nx workspace is a graph of explicitly defined projects, each with declared targets, governed by workspace-wide rules.



