## Nx Targets & Executors

### 1 First: reset your mental model

Before Nx, you already knew this world:

- package.json
- scripts
- npm run build
- pnpm run test

Everything was:
- Script-based
- Human-decided
- Tool-agnostic

Nx does **not** delete this world.

Nx **wraps and orchestrates** it.

Important rule to memorize:

Nx does NOT replace your tools.  
Nx decides **when, where, and how often** they run.

---

### 2 What is a “target”?

A **target** in Nx is:

> A named task that Nx knows how to run for a project.

Examples of targets:
- build
- test
- lint
- serve
- e2e

Think of a target as:

> “Something this project can do”

Not how it does it — just **what** it does.

---

### 3 Targets vs scripts

### package.json script (old world)

    {
      "scripts": {
        "build": "tsc",
        "test": "vitest"
      }
    }

Problems at scale:
- Nx cannot reason about dependencies
- Nx cannot cache intelligently
- Humans decide what to run

---

### Nx target (new world)

    {
      "targets": {
        "build": {
          "executor": "@nx/js:tsc"
        }
      }
    }

Benefits:
- Nx understands the task
- Nx knows inputs & outputs
- Nx can cache
- Nx can skip work safely

Mental model:

Script = command  
Target = orchestrated task

---

### 4 Where targets live

Targets live in **project.json**.

Example:

    apps/api/project.json

    {
      "name": "api",
      "projectType": "application",
      "targets": {
        "build": {},
        "test": {}
      }
    }

Nx never scans random files.
Nx only reads declared configuration.

---

### 5 What is an executor?

An **executor** is:

> The implementation that actually runs a target.

If a target is “what”, executor is “how”.

Example:

    "build": {
      "executor": "@nx/js:tsc"
    }

This means:
- Nx calls the executor
- Executor runs TypeScript compiler
- Nx observes inputs & outputs

Nx itself does **not** compile code.

---

### 6 Target anatomy

Example target:

    "build": {
      "executor": "@nx/js:tsc",
      "outputs": ["{options.outputPath}"],
      "options": {
        "outputPath": "dist/apps/api",
        "main": "apps/api/src/main.ts",
        "tsConfig": "apps/api/tsconfig.app.json"
      }
    }

Each part means:

- executor → what tool runs
- options → tool configuration
- outputs → what files are produced (for caching)

Nx uses this information to:
- Cache results
- Detect changes
- Skip work safely

---

### 7 Default targets

Nx allows **default target configuration** in `nx.json`.

Example:

    {
      "targetDefaults": {
        "build": {
          "dependsOn": ["^build"],
          "cache": true
        }
      }
    }

This means:
- Every build target depends on dependency builds
- Build outputs are cached automatically

You avoid repeating config everywhere.

Senior takeaway:
Centralize behavior, not scripts.

---

### 8 Custom targets

Targets are not limited to build/test.

Example custom target:

    "docker": {
      "executor": "nx:run-commands",
      "options": {
        "command": "docker build -t api ."
      }
    }

Nx now understands:
- docker is a real task
- It can be cached (if configured)
- It can be orchestrated

Anything can be a target.

---

### 9 Executors vs run-commands (important choice)

There are two ways to run work:

### Specialized executor (preferred)

- @nx/js:tsc
- @nx/jest:jest
- @nx/vite:build

Pros:
- Nx understands inputs & outputs
- Best caching
- Best affected detection

---

### run-commands executor (generic)

    "executor": "nx:run-commands"

Pros:
- Easy
- Flexible

Cons:
- Nx sees it as a black box
- Weaker caching

Senior rule:
Use real executors when possible.
Use run-commands when necessary.

---

### 10 Migrating from package.json scripts (step by step)

### Step 1 — Identify core scripts

Common scripts:
- build
- test
- lint

These are good candidates for targets.

---

### Step 2 — Move logic into targets

Before:

    npm run build

After:

    nx build api

Nx now controls execution.

---

### Step 3 — Keep scripts temporarily (important)

You can keep scripts like:

    "scripts": {
      "build": "nx build api"
    }

This helps gradual migration.
No big bang required.

---

### 11 When NOT to migrate scripts

Do NOT migrate scripts that are:

- One-off utilities
- Dev-only helpers
- Rarely used commands
- Personal workflows

Examples:
- reset-db
- seed-local
- open-coverage

Nx targets are for:
- Repeatable
- Shared
- CI-relevant tasks

---

### 12 Common beginner mistake

❌ Migrating every script blindly  
❌ Turning Nx into a script dump  
❌ Over-configuring early

Nx is about:
- Decision automation
- Correct execution
- Reduced human judgment

---

### 13 Senior-level insight

Targets are the **contract** between your code and Nx.

Good targets:
- Are explicit
- Are stable
- Are shared across teams

Bad targets:
- Are ad-hoc
- Encode personal workflows
- Change frequently

---

### One-sentence takeaway

Nx targets describe what projects can do, executors describe how they do it, and Nx orchestrates execution safely and efficiently.

