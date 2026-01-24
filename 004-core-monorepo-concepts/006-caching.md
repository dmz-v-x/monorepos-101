## Caching

### 1. First: what problem is caching solving?

Without caching:

- Every task runs every time  
- Even if nothing changed  
- Even if you already ran it before  

This leads to:

- Slow builds  
- Slow tests  
- Slow CI  
- Wasted CPU and time  

Caching exists to **stop repeating the same work**.

---

### 2. Plain-English definition

Caching means **reusing the result of a task when its inputs have not changed**.

If nothing important changed:  
Do not redo the work.

---

### 3. Very simple example (outside monorepos)

Imagine this flow:

- You compile TypeScript  
- You do not change any `.ts` files  
- You run the build again  

Recompiling everything is pointless.

A cache says:

“I already did this exact work.  
Here is the result.”

The build finishes instantly.

---

### 4. What counts as “inputs” to a task

This is extremely important.

For a task like `build`, inputs usually include:

- Source files (for example `.ts`, `.js`)  
- Dependency code  
- Configuration files (tsconfig, build config)  
- Environment variables  
- The task command itself  

Rules:

- If **any input changes** → cache is invalid  
- If **no inputs change** → cache can be reused  

This is how correctness is preserved.

---

### 5. What actually gets cached

Caching stores **task results**, not just files.

Examples:

- Build outputs (for example `dist/`)  
- Test results  
- Lint results  

So instead of:

- Running the task again  

The tool:

- Restores the output instantly  

---

### 6. Caching in a monorepo context

In a monorepo you have:

- Many packages  
- Many tasks  
- Many repeated executions  

Caching allows:

- Package A build to be reused  
- Package B test results to be reused  
- Only changed packages to re-run  

This saving compounds quickly as the repo grows.

---

### 7. Local cache vs remote cache

There are two main kinds of cache.

Local cache:

- Stored on your machine  
- Speeds up repeated local runs  
- Deleted if you clear cache or reinstall  

Remote cache:

- Stored on a shared server  
- CI can reuse results from developer machines  
- Developers can reuse results from CI  

Remote cache is a **massive productivity multiplier**.

---

### 8. Why caching is safe

A common concern is:

“What if cached results are wrong?”

Good monorepo tools:

- Hash inputs carefully  
- Include dependency changes  
- Include config changes  
- Invalidate cache when needed  

Result:

- Cache hits are safe  
- Cache misses automatically re-run tasks  

Correctness is never sacrificed.

---

### 9. Why caching changes developer behavior

Without caching:

- Developers avoid running tests  
- CI feels slow and noisy  
- Feedback loops are long  

With caching:

- Running tasks feels cheap  
- Developers run tests more often  
- CI becomes fast and quiet  

This indirectly improves code quality.

---

### 10. Senior-level insight

Monorepos **without caching** scale linearly.  
Monorepos **with caching** scale sub-linearly.

That difference determines whether a monorepo succeeds or fails at scale.

---

### One-sentence takeaway

Caching avoids re-running tasks by reusing previous results when the task’s inputs have not changed.
