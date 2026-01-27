## Linking local packages 

## 1. First: what does “linking” mean? 

In normal npm usage:

- You publish a package to npm
- Another project installs it from the internet
- Code is copied into node_modules

In a monorepo, this would be slow and painful.

So instead, pnpm does **linking**.

Linking means:

- One package uses another package’s code
- Directly from the same repository
- Without publishing
- Without copying files

The packages are **connected locally**.

---

## 2. Plain-English definition 

Linking local packages means pnpm connects workspace packages together so they behave like installed dependencies, but come from the same repository.

Key idea:
- They feel like npm packages
- They are imported the same way
- But they are resolved locally

---

## 3. Why linking is necessary in monorepos

In a monorepo you usually have:

- Apps (things you run)
- Libraries (things apps depend on)
- Shared utilities

Example structure:

    repo/
     ├─ apps/
     │   └─ api/
     │       └─ package.json
     └─ packages/
         ├─ auth-lib/
         │   └─ package.json
         └─ shared-utils/
             └─ package.json

The api app needs code from:

- auth-lib
- shared-utils

Linking is how this dependency relationship works **cleanly**.

---

## 4. How linking works conceptually 

Think in package terms:

- auth-lib is a package
- api depends on auth-lib
- The dependency is declared in package.json
- Imports look exactly like npm imports

Example code inside api:

    import { login } from "@company/auth-lib";

There is:
- no relative path
- no special syntax
- no monorepo-specific code

All complexity is handled by the package manager.

---

## 5. How pnpm knows a dependency is local

pnpm links a dependency locally only if **all** of these are true:

1. Both packages are part of the same workspace  
   (listed in pnpm-workspace.yaml)

2. The dependency name matches exactly  
   (name field in package.json)

3. The version range allows workspace resolution  
   (for example: workspace:* or compatible version)

If these conditions are met, pnpm decides:

“This dependency exists locally. I will link it.”

No registry lookup.
No download.

---

## 6. The recommended way: workspace protocol 

In the consuming package (for example apps/api/package.json):

    {
      "dependencies": {
        "@company/auth-lib": "workspace:*"
      }
    }

What this means in plain English:

- Use the local workspace version
- Never fetch from npm
- Always stay in sync

This is the safest and clearest way to link local packages.

---

## 7. Practical step-by-step example 

Step 1: define workspace (pnpm-workspace.yaml)

    packages:
      - "apps/*"
      - "packages/*"

Step 2: create packages

    apps/api/package.json
    packages/auth-lib/package.json

auth-lib/package.json:

    {
      "name": "@company/auth-lib",
      "version": "1.0.0"
    }

api/package.json:

    {
      "name": "api",
      "dependencies": {
        "@company/auth-lib": "workspace:*"
      }
    }

Step 3: install from root

    pnpm install

pnpm will now link auth-lib into api automatically.

---

## 8. What pnpm actually does under the hood

pnpm does NOT:

- copy auth-lib files
- duplicate code
- flatten everything

Instead, pnpm:

- builds a dependency graph
- creates symlinks
- enforces strict dependency boundaries

The api package sees auth-lib as if it were installed,
but the files come from the same repo.

---

## 9. What linking is NOT 

Linking is NOT:

- Git submodules
- Copy-pasting folders
- Relative imports like ../../auth-lib
- npm link (manual, global, unsafe)

Linking IS:

- package-level dependency
- explicit
- tracked
- enforced

This keeps:
- APIs clean
- ownership clear
- tooling accurate

---

## 10. Why relative imports are a bad idea 

Bad approach:

    import { login } from "../../packages/auth-lib";

Problems:

- Paths break when folders move
- No dependency declaration
- Tooling cannot build a dependency graph
- Refactors become dangerous

Good approach:

    import { login } from "@company/auth-lib";

Benefits:

- Explicit dependency
- pnpm enforces correctness
- Graph is accurate
- Refactors are safe

---

## 11. What happens when you change a linked package

This is the **superpower** of monorepos.

If you:

- change code in auth-lib
- save the file

Then:

- api immediately sees the change
- no publish
- no reinstall
- no version bump

This enables:

- fast iteration
- atomic refactors
- consistent behavior everywhere

---

## 12. Strictness still applies 

Even though packages are local:

- auth-lib can only use its declared dependencies
- api can only use its declared dependencies

pnpm does NOT relax rules for local packages.

This is why monorepos remain safe at scale.

---

## 13. Common beginner mistakes

- Forgetting to add dependency in package.json
- Mismatch between package name and dependency name
- Forgetting pnpm-workspace.yaml
- Using relative imports instead of package imports
- Expecting pnpm to auto-link without workspace config

pnpm will not guess.
It will fail fast (by design).

---

## 14. Advanced: version constraints with workspace protocol

Examples:

    "workspace:*"        → always use local
    "workspace:^1.0.0"   → enforce compatible local version
    "workspace:~1.2.0"   → stricter local constraint

This allows:
- gradual versioning
- future publishing
- controlled evolution

---

## 15. Senior-level mental model 

Local package linking gives you:

- the **discipline of packages**
- the **speed of local code**

This is why monorepos scale.

Without linking:
- duplication grows
- coordination explodes
- refactors stop

With linking:
- boundaries stay explicit
- changes propagate safely
- systems stay evolvable

---

## One-sentence takeaway 

pnpm links local workspace packages so they behave like real dependencies, without publishing, duplication, or loss of safety.
