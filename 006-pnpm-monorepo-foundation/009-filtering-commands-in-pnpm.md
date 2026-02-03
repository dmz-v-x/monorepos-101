## Filtering commands in pnpm

### 1. The core problem filtering solves

In a monorepo you usually have:

- Many packages
- Each with scripts like build, test, lint, dev

Example:

- apps/api
- apps/web
- packages/auth-lib
- packages/shared-utils
- packages/db
- … and many more

If you run this at the repo root:

    pnpm run build

pnpm will:

- Run `build` in **every workspace package**
- Even if most packages are unrelated to your change

This is often:

- Slow
- Wasteful
- Bad for developer experience

Filtering exists to solve this exact problem.

---

### 2. Plain-English definition

Filtering lets you run pnpm commands on **only a selected subset of workspace packages** instead of all of them.

---

### 3. The `--filter` flag

pnpm provides a special flag:

    pnpm --filter <target> <command>

This tells pnpm:

- Which packages to include
- Which command to run in those packages

Everything else is ignored.

This is the foundation of filtering.

---

### 4. Filtering by package name

If a package has this in its package.json:

    {
      "name": "@company/api"
    }

You can run a script only for that package:

    pnpm --filter @company/api build

Meaning:

- Run the `build` script
- Only in `@company/api`
- No other package is touched

This is the most common and safest form of filtering.

---

### 5. Filtering by folder path

You can also filter by filesystem path:

    pnpm --filter ./apps/api build

This is useful when:

- You remember the folder but not the package name
- You are navigating the repo structure directly

pnpm will:

- Find the package.json in that folder
- Treat it as the filter target

---

### 6. Filtering by dependency relationships

This is where pnpm becomes dependency-aware.

### 6.1 Run a command on a package AND its dependencies

    pnpm --filter @company/api... build

The `...` means:

- Include the package
- Include everything it depends on

So pnpm will:

- Build shared-utils (if api depends on it)
- Build auth-lib (if api depends on it)
- Build api last
- In the correct order

This uses the dependency graph automatically.

---

### 7. Filtering by dependents

You can also go the other way.

Run a command on a package AND everything that depends on it:

    pnpm --filter ...@company/shared-utils test

Meaning:

- Test shared-utils
- Test every package that uses shared-utils

This is perfect when:

- You change a shared library
- You want to validate all consumers
- You want confidence without testing everything

---

### 8. Combining multiple filters

You can specify multiple filters:

    pnpm --filter @company/api --filter @company/web test

Meaning:

- Run tests only in api
- Run tests only in web
- Ignore all other packages

This is very common in CI pipelines.

---

### 9. Excluding packages

You can exclude packages using `!`:

    pnpm --filter '!@company/legacy-*' build

Meaning:

- Build everything
- Except legacy packages

This is useful when:

- You are migrating code
- Some packages are intentionally skipped

---

### 10. Why filtering dramatically improves developer experience

Without filtering:

- Developers hesitate to run commands
- Feedback loops are slow
- CI scripts become long and fragile

With filtering:

- Commands feel cheap
- Feedback is fast
- Developers stay in flow

Filtering makes monorepos usable day-to-day.

---

### 11. Filtering vs monorepo task runners

Filtering is:

- Built into pnpm
- Low-level
- Deterministic

Task runners like:

- Nx
- Turborepo

They:

- Build on top of filtering
- Add caching
- Add orchestration
- Add affected detection

Filtering is the primitive everything else relies on.

---

### 12. Practical mental model

Think of filtering as:

“Selecting the packages first, then running the command.”

pnpm answers:

- Which packages?
- In what order?
- Then runs the script

You stop thinking about folders.
pnpm thinks in packages and dependencies.

---

### 13. Senior-level insight

Filtering is how monorepos avoid the “run everything” trap.

Without filtering:

- Every command scales with repo size
- Productivity collapses as repos grow

Filtering keeps work proportional to impact.

---

### 14. One-sentence takeaway

pnpm filtering lets you run commands only on relevant workspace packages (and their dependencies), saving time and keeping monorepos fast and manageable.
