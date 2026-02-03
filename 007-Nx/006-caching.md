## Caching (Local & Remote)


### 1. First: reset your understanding of “caching”

Most beginners think caching means:

> “We save something and reuse it”

That is **too shallow**.

In Nx, caching means:

> “If the exact same task was already run with the exact same inputs, do not run it again.”

This is not an optimization.
This is **eliminating duplicate computation**.

---

### 2. The core problem caching solves

Without caching:

- Every build runs every time
- Every test runs every time
- CI repeats the same work over and over
- Developers wait for no reason

With caching:

- Previously computed results are reused
- Work is skipped safely
- Feedback becomes instant

Nx exists to **avoid recomputation**.

---

### 3. Plain-English definition

Task caching means reusing the output of a task when its inputs have not changed.

If nothing important changed:
→ do not redo the work

---

### 4. What Nx considers a “task”

A task in Nx is:

- A target
- Run for a specific project
- With a specific configuration

Example tasks:

- api:build
- shared-utils:test
- web:lint

Each of these tasks can be cached independently.

---

### 5. Inputs & outputs

Nx caching is based on **inputs and outputs**.

### Inputs

Inputs define **what affects the result of a task**.

Common inputs:

- Source files
- Dependency source files
- Configuration files
- Environment variables
- The command itself

If *any input changes* → cache is invalid

---

### Outputs

Outputs define **what the task produces**.

Examples:

- dist/
- build/
- coverage/
- .next/

Nx stores outputs in the cache.

---

### 6. Example: build task inputs & outputs

For an api build:

Inputs might include:

- apps/api/src/**
- packages/auth-lib/src/**
- tsconfig.json
- project.json
- package.json

Outputs might include:

- dist/apps/api

If all inputs are identical:
→ Nx restores dist instead of rebuilding

---

### 7. Cache invalidation rules

Nx cache is invalidated when:

- Any input file changes
- Any dependency input changes
- Any relevant config changes
- Any environment variable changes
- The command changes

Nx is **conservative**:
- It would rather miss a cache than return a wrong result

Correctness > speed.

---

### 8. Local cache behavior

By default, Nx uses a **local cache**.

This cache:

- Lives on your machine
- Is stored in .nx/cache (or node_modules/.cache)
- Persists across runs
- Is fast (filesystem-based)

Local cache benefits:

- Re-running commands feels instant
- Switching branches is fast
- Developers trust running tasks again

---

### 9. What a cache hit looks like

You run:

	nx build api

Nx prints:

	✔  nx run api:build (cached)

Meaning:

- Task was NOT executed
- Output was restored
- Time saved

This is why Nx feels “magical”.

---

### 10. What a cache miss means

A cache miss means:

- Inputs changed
- Task must run
- Output is recomputed
- Result is stored in cache

Cache misses are expected.
Cache hits are the win.

---

### 11. Remote cache (Nx Cloud)

Local cache helps one developer.

Remote cache helps **everyone**.

Remote cache means:

- Cache is shared across machines
- CI and dev machines reuse results
- Work is done once per change globally

This is huge.

---

### 12. Plain-English definition of remote cache

Remote caching means:

> “If anyone already ran this task, no one else needs to.”

Including CI.

---

### 13. Nx Cloud

Nx Cloud provides:

- Shared cache storage
- Secure access
- CI + local integration
- Visibility into cache hits

It plugs directly into Nx.

---

### 14. How CI speedups actually happen

Without remote cache:

- CI installs deps
- CI builds everything
- CI tests everything

With remote cache:

- Dev runs build locally
- Output is cached remotely
- CI pulls results
- CI skips execution

CI becomes **mostly restore operations**.

This can cut CI times by 60–90%.

---

### 15. Enabling caching

Caching is enabled by default in Nx.

You define which targets are cacheable in nx.json:

	namedInputs:
	  default:
	    - "{projectRoot}/**/*"

	targetDefaults:
	  build:
	    cache: true
	  test:
	    cache: true
	  lint:
	    cache: true

This tells Nx:
- Cache build, test, lint tasks

---

### 16. Inspecting cache behavior

Run any command with verbose output:

	nx build api --verbose

Nx will tell you:

- Whether the task was cached
- Why it was cached or not
- Which inputs were considered

This builds trust.

---

### 17. Comparing CI times

Before Nx cache:

- CI time: 25 minutes
- Most work duplicated

After Nx + remote cache:

- CI time: 5 minutes
- Mostly cache restores

This is not theoretical.
This is why companies adopt Nx.

---

### 18. Common cache mistakes

❌ Forgetting to declare outputs  
❌ Including unnecessary files as inputs  
❌ Using non-deterministic scripts  
❌ Relying on timestamps or random values  
❌ Writing to undeclared locations  

Caching exposes bad task design.

That’s a feature.

---

### 19. Senior-level insight

Caching does not make bad systems fast.
It makes *well-defined systems* fast.

Nx forces you to define:
- What matters
- What doesn’t
- What is reproducible

This improves architecture.

---

### 20. One-sentence takeaway 

Nx caching reuses task results based on precise inputs and outputs, making local development and CI dramatically faster.


