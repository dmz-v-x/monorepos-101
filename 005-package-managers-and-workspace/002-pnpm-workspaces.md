## pnpm Workspaces

### 1. First: why pnpm exists

pnpm was created to fix **structural problems** in npm and Yarn:

- Hidden dependencies (packages using deps they didn’t declare)
- Massive disk duplication
- Slow installs in large repos
- Non-deterministic behavior
- Weak isolation in monorepos

These problems get **worse as projects grow**.

pnpm’s philosophy is:

Correctness first  
Performance second  
But achieve both

pnpm does not rely on “best practices”.
It **enforces correctness by design**.

---

### 2. Plain-English definition

pnpm workspaces manage multiple packages in one repository with:

- strict dependency isolation  
- extremely fast installs  
- minimal disk usage  

This single sentence explains why pnpm dominates monorepos today.

---

### 3. The BIG difference: pnpm is strict by default

In pnpm:

- A package can ONLY import dependencies listed in its own `package.json`
- No accidental access to hoisted dependencies
- No hidden coupling
- No “it works on my machine” surprises

If you forget to declare a dependency:

- Your code fails immediately  
- The error is loud and obvious  

This is a **feature**, not a downside.

---

### 4. Why this strictness matters in monorepos

Earlier problems you learned about:

- Hidden dependencies  
- Accidental coupling  
- Refactoring fear  
- CI surprises  

pnpm prevents these **structurally**.

Instead of trusting developers to “do the right thing”:

pnpm enforces correctness automatically.

At scale, this is the difference between:
- a healthy monorepo  
- a collapsing one  

---

### 5. How pnpm workspaces are defined

pnpm uses a dedicated config file:

pnpm-workspace.yaml

Example:

packages:
  - "apps/*"
  - "packages/*"

What this means:

- Only these folders are workspace members
- Any folder inside them with a `package.json` becomes a package
- Nothing else is accidentally included

This explicitness matters a lot in large repos.

---

### 6. Practical setup: pnpm workspace from scratch

#### Step 1: enable pnpm safely (via Corepack)

Corepack ships with Node.js (**before v25 of node.js - If using Node.js 25 or newer: Corepack is not pre-installed. You must install it manually via npm before running your command**) and manages package managers.

Run once:

    corepack enable  
    corepack prepare pnpm@latest --activate  

Verify:

    pnpm --version  

If you see a version, pnpm is ready.

---

#### Step 2: create the workspace root

    mkdir my-monorepo  
    cd my-monorepo  
    pnpm init -y  

This creates the root `package.json`.

This root usually:
- does NOT run an app
- holds shared config and tooling

---

#### Step 3: create pnpm-workspace.yaml

At the repo root:

`pnpm-workspace.yaml`

Content:

packages:
  - "apps/*"
  - "packages/*"

This single file turns the repo into a pnpm workspace.

---

#### Step 4: create workspace packages

    mkdir -p apps/api packages/auth-lib  

Initialize each package:

    cd apps/api  
    pnpm init -y  

    cd ../../packages/auth-lib  
    pnpm init -y  
    
    cd ../../  

Now you have multiple workspace packages.

---

#### Step 5: install dependencies once

From the root:

`pnpm install`  

pnpm will:

- discover all workspace packages
- install dependencies for all of them
- create ONE pnpm-lock.yaml
- link local packages automatically

You never run installs per-package again.

---

### 7. Local packages in pnpm 

Inside apps/api/package.json:

    {
      "dependencies": {
        "@company/auth-lib": "workspace:*"
      }
    }

What this does:

- Tells pnpm to use the local workspace package
- Prevents fetching from npm
- Keeps all consumers in sync

Run:

`pnpm install` 

Verify:

`pnpm why @company/auth-lib`  

You’ll see it resolved from the workspace.

---

### 8. pnpm’s killer feature: the global store

pnpm does NOT copy dependencies into every project.

Instead:

- One global content-addressable store
- Each dependency version stored once
- Files are hard-linked into projects

Result:

- 100 projects using react
- 1 copy of react on disk

Benefits:

- Massive disk savings
- Faster installs
- Deterministic dependency resolution

This is one of pnpm’s biggest advantages.

---

### 9. node_modules structure in pnpm

pnpm creates a **non-flat node_modules** structure.

At first glance:
- it looks strange
- it looks nested
- it looks unfamiliar

But this structure:

- enforces strict package boundaries
- prevents hidden dependencies
- still works perfectly with Node.js resolution

This is intentional.
Flat node_modules caused most dependency bugs.

pnpm fixes that.

    npm / yarn node_modules (FLAT)
    project/
    │
    ├─ node_modules/
    │  ├─ express/
    │  ├─ body-parser/
    │  ├─ debug/
    │  ├─ accepts/
    │  ├─ mime-types/
    │  ├─ qs/
    │  └─ ... (hundreds more)
    │
    ├─ package.json


    pnpm structure (VISUAL)
    project/
    │
    ├─ node_modules/
    │  ├─ .pnpm/
    │  │  ├─ express@4.18.2/
    │  │  │  └─ node_modules/
    │  │  │     ├─ body-parser/
    │  │  │     ├─ debug/
    │  │  │     └─ accepts/
    │  │  │
    │  │  ├─ body-parser@1.20.1/
    │  │  │  └─ node_modules/
    │  │  │     ├─ bytes/
    │  │  │     └─ qs/
    │  │  │
    │  │  └─ debug@4.3.4/
    │  │     └─ node_modules/
    │  │        └─ ms/
    │  │
    │  ├─ express → .pnpm/express@4.18.2/node_modules/express
    │
    ├─ package.json

---

### 10. Practical strictness example 

If api imports something it didn’t declare:

import zod from "zod";

But `zod` is not in api/package.json:

- npm → might work accidentally
- pnpm → FAILS immediately

This forces correct dependency graphs.

This is why pnpm monorepos stay sane.

---

### 11. Why pnpm is preferred for monorepos today

pnpm gives you:

- strict dependency correctness  
- fast installs  
- minimal disk usage  
- deterministic builds  
- excellent CI performance  
- perfect compatibility with Nx and Turborepo  

No other package manager combines all of these as well.

---

### 12. When pnpm may NOT be ideal

pnpm might not be ideal if:

- you rely on very old tooling
- your OS or environment forbids symlinks
- you don’t control your CI environment

For modern backend and frontend monorepos:

pnpm is the default choice.

---

### 13. Advanced workspace commands

Run a script in all packages:

`pnpm -r run build`  

Run a script in one package:

`pnpm --filter api run build`  

Add a dependency to one package:

`pnpm --filter api add zod`  

Add a dev dependency at the root:

`pnpm add -D -w typescript`  

These commands are core to daily monorepo work.

---

### One-sentence takeaway

pnpm workspaces provide strict, fast, and deterministic dependency management, making them the best foundation for large, modern monorepos.
