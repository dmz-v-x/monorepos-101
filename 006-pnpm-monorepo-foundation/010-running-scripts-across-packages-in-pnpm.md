## Running scripts across packages in pnpm

### 1. What “running scripts” means

In JavaScript projects, scripts live in `package.json`.

Example:

    {
      "scripts": {
        "build": "tsc",
        "test": "vitest",
        "lint": "eslint ."
      }
    }

In a **single project**:

    pnpm run build

Runs exactly one script in one package.

---

### 2. Why this becomes a problem in monorepos

In a monorepo:

- You have many packages
- Each package has its own `package.json`
- Many packages share the same script names (`build`, `test`, `lint`)

Example:

- apps/api → build
- apps/web → build
- packages/auth-lib → build
- packages/shared-utils → build

Now the question becomes:

“How do I run `build` across many packages at once?”

This is where recursive execution comes in.

---

### 3. Workspace-wide (recursive) script execution

pnpm provides a **recursive** mode using `-r`.

Command:

    pnpm -r run build

What this means:

- `-r` = recursive
- pnpm looks at all workspace packages
- Finds those that define a `build` script
- Runs `build` in each of them

Packages without a `build` script are skipped automatically.

---

### 4. Plain-English definition

Recursive script execution runs the same script across multiple workspace packages.

---

### 5. How pnpm decides which packages to run

When you run:

    pnpm -r run build

pnpm:

1. Reads `pnpm-workspace.yaml`
2. Detects all workspace packages
3. Checks each package’s `package.json`
4. Selects only packages that define `build`
5. Runs the script in those packages

You do **not** need to manually list packages.

---

### 6. Parallel vs ordered execution 

By default:

- pnpm runs scripts **in parallel** when possible
- This makes execution fast

However, sometimes order matters.

Example:

- shared-utils must build before auth-lib
- auth-lib must build before api

pnpm understands dependency relationships.

---

### 7. Running scripts in dependency order

To force strict ordering:

    pnpm -r --workspace-concurrency=1 run build

This does:

- Runs one package at a time
- Respects dependency order
- Ensures dependencies build first

This is useful when:

- Build outputs are required
- Packages depend on generated files

---

### 8. Combining recursion with filtering

In real monorepos, you almost always combine `-r` with `--filter`.

Example:

    pnpm -r --filter @company/api... run build

This means:

- `-r` → recursive execution
- `--filter @company/api...` → api and its dependencies
- `run build` → run build script

pnpm will:

1. Find api
2. Find everything api depends on
3. Run builds in correct order
4. Skip all unrelated packages

This is dependency-aware execution.

---

### 9. Filtering vs recursion

Think of it like this:

- `-r` answers: **how many packages**
- `--filter` answers: **which packages**

They solve different problems.

You almost always use them together.

---

### 10. Running scripts across all packages intentionally

Some tasks are meant to run everywhere.

Examples:

- Linting
- Formatting
- Type checking

Command:

    pnpm -r run lint

This ensures:

- All packages follow standards
- No package is forgotten

---

### 11. Why this is useful in CI

CI pipelines often need to:

- Lint everything
- Build only apps
- Test only affected packages

pnpm allows:

- Simple commands
- No custom bash loops
- No fragile scripts

Example CI steps:

    pnpm -r run lint
    pnpm -r --filter ./apps/** run build
    pnpm -r --filter @company/api... run test

---

### 12. What pnpm does NOT do here 

pnpm does NOT:

- Cache task results
- Detect file-level changes
- Skip tasks automatically based on diffs
- Compute “affected” projects by itself

That intelligence is provided by tools like:

- Nx
- Turborepo

pnpm provides **correct execution**, not optimization.

---

### 13. Common beginner mistakes

Avoid these:

- Running `pnpm run build` expecting it to target one package
- Forgetting `-r` in monorepos
- Writing manual loops over folders
- Confusing filtering with recursion

Let pnpm do the coordination.

---

### 14. Senior-level insight

pnpm is responsible for:

- Dependency correctness
- Script execution
- Workspace awareness

Higher-level tools handle:

- Caching
- Incremental builds
- Affected detection

Do not mix responsibilities.

---

### 15. One-sentence takeaway

pnpm runs scripts across workspace packages using recursive execution (`-r`) and filtering (`--filter`), giving safe, dependency-aware control over large monorepos.
