## Strict dependency resolution (pnpm)

## 1. First: what “dependency resolution” means

Dependency resolution is simply:

How Node.js figures out **where an imported package comes from**.

When you write:

    import express from "express";

The system must answer:

- Where is `express` located?
- Which version is used?
- Is this package allowed to be used here?

This lookup process is called dependency resolution.

---

## 2. How Node.js resolves modules 

Node.js resolves imports by:

1. Looking in the current folder’s `node_modules`
2. If not found, moving up to the parent folder
3. Repeating until it reaches the filesystem root

This behavior is **not a bug**.
It’s how Node.js has always worked.

Package managers decide **what Node.js can see**.

---

## 3. How npm / Yarn classic behave

npm and Yarn classic use **hoisted node_modules**.

This means:

- Dependencies are placed high up (often at repo root)
- Many packages share the same physical dependencies

Result:

- Node.js can “see” dependencies from parent folders
- Packages can accidentally import things they didn’t declare

Example:

    // auth-lib/package.json
    // axios is NOT listed here

    import axios from "axios"; // works locally 😬

Why this works:

- Another package installed axios
- axios got hoisted
- Node.js finds it while walking upward

This creates **hidden dependencies**.

---

## 4. Why hidden dependencies are dangerous

Hidden dependencies cause:

- “Works on my machine” bugs
- CI-only failures
- Production-only crashes
- Fear of refactoring
- Accidental coupling between packages

Worst part:

- The code *looks correct*
- The bug is invisible until later

This is one of the most painful JavaScript ecosystem problems.

---

## 5. What “strict dependency resolution” means

Strict dependency resolution means:

A package can ONLY import dependencies that are explicitly listed in its own `package.json`.

Nothing more  
Nothing less  

If a dependency is not declared:

- The import fails immediately

This is intentional and correct.

---

## 6. How pnpm enforces strict dependency resolution

pnpm enforces strictness structurally, not by linting.

It does this by:

- Using non-hoisted dependency layouts
- Creating isolated `node_modules` views per package
- Exposing only declared dependencies via symlinks
- Preventing upward dependency leakage

Node.js still resolves modules normally —
but it only sees what pnpm allows it to see.

---

## 7. What pnpm’s node_modules isolation means in practice

For a package `auth-lib`:

- It only has access to:
  - its own dependencies
  - its declared peer dependencies

It CANNOT access:

- dependencies of sibling packages
- dependencies hoisted at the repo root
- undeclared transitive dependencies

This guarantees correctness.

---

## 8. Practical example: forgetting a dependency

You write:

    import zod from "zod";

But `package.json` does NOT include zod.

With pnpm:

- Build fails immediately
- Error is clear
- Fix is obvious

You run:

    pnpm add zod

Problem solved.

With hoisted managers:

- It might work locally
- It might fail in CI
- It might fail in production

pnpm surfaces the bug early.

---

## 9. Why this strictness is a GOOD thing

Many beginners think:

“pnpm is annoying”
“pnpm broke my code”

Reality:

- The code was already wrong
- pnpm refused to hide the mistake

pnpm moves bugs:

- from runtime → install-time
- from production → development
- from hard-to-debug → obvious

This saves massive time long-term.

---

## 10. Why strict resolution is critical in monorepos

Monorepos have:

- many packages
- shared utilities
- multiple teams
- frequent refactors

Hidden dependencies in monorepos lead to:

- accidental cross-team coupling
- fragile builds
- dependency graph lies
- broken incremental builds

pnpm’s strictness ensures:

- dependency graph is accurate
- boundaries are real
- tooling can reason safely

This enables:
- caching
- incremental builds
- safe refactors

---

## 11. Practical commands related to strictness

### Check missing dependencies 

If pnpm throws an error about a missing dependency:

1. Identify which package failed
2. Add the dependency to that package:

    pnpm --filter <package-name> add <dependency>

Example:

    pnpm --filter auth-lib add axios

---

### Add dependency at workspace root 

For tooling dependencies:

    pnpm add -D -w eslint

The `-w` flag means workspace root.

---

### Detect undeclared dependencies (advanced)

pnpm will naturally fail on undeclared imports.
No extra tooling is required.

This is enforcement by design.

---

## 12. Common reactions and the correct mindset

Wrong mindset → Correct mindset

“it worked before” → “it was broken before”  
“pnpm is too strict” → “pnpm is correctly strict”  
“this slows me down” → “this prevents future bugs”  

This mindset shift is essential to using pnpm effectively.

---

## 13. Senior-level insight

Strict dependency resolution moves errors to the earliest possible moment.

Earlier errors are:

- cheaper
- easier
- safer

pnpm enforces correctness structurally, not socially.

That’s why it scales.

---

## One-sentence takeaway

pnpm enforces strict dependency resolution so packages can only use what they explicitly declare, preventing hidden dependencies and long-term instability.
