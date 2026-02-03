## Complete pnpm Monorepo Demo 

### 0. Prerequisites (what you need before starting)

You only need:

- Node.js ≥ 18 (LTS recommended)
- npm (comes with Node)

You do NOT need pnpm yet.

---

### 1. Installing pnpm correctly (Corepack)

### 1.1 What is Corepack?

Corepack is a tool that ships with Node.js.
Its job is to:

- Manage package managers (pnpm, yarn)
- Ensure consistent versions across machines
- Avoid “works on my machine” issues

Instead of globally installing pnpm, we **activate it via Corepack**.

---

### 1.2 Enable Corepack

    corepack enable

What this does:

- Enables Corepack shims
- Allows `pnpm` to be resolved automatically
- Does NOT install pnpm yet

---

### 1.3 Install & activate the latest pnpm

    corepack prepare pnpm@latest --activate

What this does:

- Downloads the latest pnpm
- Pins it for your environment
- Makes `pnpm` available as a command

Verify:

    pnpm --version

---

### 2. Creating the monorepo root

### 2.1 Create the repository

    mkdir my-monorepo
    cd my-monorepo

---

### 2.2 Initialize root package.json

    pnpm init -y

This creates:

    package.json

Important rule:
The **root is NOT a real package**.
It exists only to coordinate the workspace.

---

### 2.3 Mark root as private (VERY IMPORTANT)

Edit `package.json`:

    {
      "name": "my-monorepo",
      "private": true
    }

Why this matters:

- Prevents accidental publishing
- Required for all monorepos

---

### 3. Defining the workspace (pnpm-workspace.yaml)

### 3.1 Create the file

At the repo root:

    pnpm-workspace.yaml

---

### 3.2 Define workspace packages

    packages:
      - "apps/*"
      - "packages/*"

Meaning:

- Any folder under `apps/` with package.json → workspace package
- Any folder under `packages/` with package.json → workspace package

pnpm will NOT guess.
This file is mandatory.

---

### 4. Creating workspace packages

### 4.1 Create folders

    mkdir -p apps/api
    mkdir -p apps/web
    mkdir -p packages/shared-utils
    mkdir -p packages/auth-lib

---

### 4.2 Initialize each package

    cd apps/api
    pnpm init -y

    cd ../web
    pnpm init -y

    cd ../../packages/shared-utils
    pnpm init -y

    cd ../auth-lib
    pnpm init -y

Go back to root:

    cd ../../

---

### 4.3 Name the packages properly

Edit package.json files:

apps/api/package.json

    {
      "name": "@company/api",
      "private": true
    }

apps/web/package.json

    {
      "name": "@company/web",
      "private": true
    }

packages/shared-utils/package.json

    {
      "name": "@company/shared-utils"
    }

packages/auth-lib/package.json

    {
      "name": "@company/auth-lib"
    }

These names are how pnpm links packages.

---

### 5. Installing dependencies 

From repo root:

    pnpm install

What happens:

- One pnpm-lock.yaml is created
- Dependencies are installed once
- pnpm store is populated
- node_modules are linked correctly

This is a **deterministic install**.

---

### 6. Creating real code

### 6.1 shared-utils

packages/shared-utils/src/index.js

    export function formatDate(date) {
      return new Date(date).toISOString()
    }

packages/shared-utils/package.json

    {
      "name": "@company/shared-utils",
      "main": "src/index.js"
    }

---

### 6.2 auth-lib depends on shared-utils

packages/auth-lib/package.json

    {
      "name": "@company/auth-lib",
      "dependencies": {
        "@company/shared-utils": "workspace:*"
      }
    }

packages/auth-lib/src/index.js

    import { formatDate } from "@company/shared-utils"

    export function login(user) {
      return {
        user,
        loggedInAt: formatDate(Date.now())
      }
    }

No publishing.
No version coordination.
Pure local linking.

---

### 6.3 api depends on auth-lib

apps/api/package.json

    {
      "name": "@company/api",
      "private": true,
      "dependencies": {
        "@company/auth-lib": "workspace:*"
      },
      "scripts": {
        "build": "node src/index.js",
        "test": "echo API tests"
      }
    }

apps/api/src/index.js

    import { login } from "@company/auth-lib"

    console.log(login("alice"))

---

### 7. Strict dependency enforcement 

Try importing something undeclared:

    import axios from "axios"

Result:

- Immediate error
- pnpm prevents hidden dependency bugs

This is **strict dependency resolution**.

---

### 8. Running scripts

### 8.1 Run build everywhere

    pnpm -r run build

Runs build in all packages that define it.

---

### 8.2 Run build only for api

    pnpm --filter @company/api run build

---

### 8.3 Run build for api AND its dependencies

    pnpm -r --filter @company/api... run build

Order:

1. shared-utils
2. auth-lib
3. api

Dependency graph respected.

---

### 8.4 Run tests for everything that depends on shared-utils

    pnpm -r --filter ...@company/shared-utils run test

---

### 9. Understanding node_modules 

pnpm creates:

- Global store (~/.pnpm-store)
- Virtual store (node_modules/.pnpm)
- Symlinks per package

Benefits:

- No duplication
- Strict isolation
- Fast installs
- Deterministic behavior

Do NOT modify node_modules manually.

---

### 10. Deterministic installs & lockfile

pnpm-lock.yaml:

- Must be committed
- Must never be regenerated in CI
- Guarantees same tree everywhere

CI rule:

    pnpm install --frozen-lockfile

If lockfile is wrong → CI fails (GOOD).

---

### 11. Filtering patterns

By path:

    pnpm --filter ./apps/** run build

Exclude legacy:

    pnpm -r --filter '!@company/legacy-*' run build

Multiple filters:

    pnpm -r --filter @company/api --filter @company/web run test

---

### 12. CI-style workflow

Typical pipeline:

    pnpm install --frozen-lockfile
    pnpm -r run lint
    pnpm -r --filter ./packages/** run test
    pnpm -r --filter ./apps/** run build

Simple.
Predictable.
Scales.

---

### 13. Mental model 

pnpm gives you:

- Correct dependency boundaries
- Deterministic installs
- Local package linking
- Workspace-aware execution

Tools like Nx/Turborepo add:

- Caching
- Incremental builds
- Affected detection

pnpm is the **foundation**.
