## Disk space efficiency (pnpm)

## 1. First: why disk usage even matters

At small scale:

- Few projects
- Few dependencies
- Disk feels unlimited
- You rarely notice inefficiency

At scale:

- Hundreds of projects
- Thousands of dependencies
- CI machines created and destroyed constantly
- Large monorepos with shared packages

Disk usage becomes:

- Cost (CI minutes, storage limits)
- Speed (I/O bottlenecks)
- Reliability (timeouts, corrupted installs)

Disk is not just “storage”.
Disk performance directly affects developer experience.

---

## 2. What happens with npm and Yarn classic

Traditional package managers work like this:

- Each project has its own `node_modules`
- Dependencies are copied into that folder
- Even if the same dependency exists elsewhere

Example:

- 30 projects
- Each has its own `node_modules`
- All use `react`, `lodash`, `zod`, etc.

Result:

- The same files exist **dozens of times**
- Thousands of duplicated files
- Gigabytes of wasted space

This leads to:

- Slower installs
- Slower file system operations
- Slower CI
- Higher storage usage

This duplication is not accidental — it’s how the system was designed.

---

## 3. pnpm’s disk model 

pnpm takes a fundamentally different approach.

pnpm:

- Stores each dependency file **once**
- Identifies files by **content**
- Reuses the same files across all projects
- Links them instead of copying

Key idea:

- One file on disk
- Many references to it

This is called a **content-addressable store**.

---

## 4. How pnpm actually saves disk space

Let’s break it down step by step.

### Step 1: Global store

pnpm maintains a global store on your machine.

Typical locations:

- Linux / macOS: `~/.local/share/pnpm/store`
- Windows: `%LOCALAPPDATA%\pnpm\store`

All dependency files live here.

### Step 2: Content-based storage

- Files are hashed by content
- Identical files share the same hash
- Only one copy exists, ever

If 100 projects use the same dependency version:

- Stored once
- Reused everywhere

### Step 3: Linking, not copying

Projects do NOT copy files.

Instead:

- pnpm uses hard links and symlinks
- Files appear inside `node_modules`
- But disk data is shared

To Node.js:
- Everything looks normal

To disk:
- No duplication happens

---

## 5. Real-world impact 

In real production systems, teams consistently observe:

- 50–80% reduction in disk usage
- 2–5x faster installs after cache is warm
- Much faster CI startup times
- Lower I/O pressure on machines

This is not theoretical.
This is observed in:

- Large monorepos
- CI environments
- Developer machines over time

The bigger the system, the bigger the win.

---

## 6. Why disk efficiency improves performance

Disk efficiency is not just about space.

Fewer files means:

- Faster directory traversal
- Faster dependency resolution
- Faster antivirus scanning
- Faster filesystem indexing
- Faster backups
- Faster cache restores

Modern filesystems struggle with:

- Huge numbers of small files

Traditional `node_modules` creates exactly that problem.

pnpm dramatically reduces filesystem stress.

---

## 7. Why CI benefits massively

CI environments are especially sensitive.

CI typically:

- Starts from a clean machine
- Installs dependencies every run
- Pays per minute of execution

With npm/Yarn:

- Large downloads every run
- Massive duplication
- Slow setup

With pnpm:

- Cache restores the store
- Dependencies are already there
- Linking is nearly instant

Result:

- Faster pipelines
- Lower CI costs
- More predictable build times

This alone justifies pnpm for many teams.

---

## 8. Why this matters even more in monorepos

Monorepos amplify the problem.

Monorepos have:

- Many packages
- Heavy dependency overlap
- Frequent installs across branches

Without disk efficiency:

- Disk usage grows exponentially
- CI becomes painfully slow
- Local development suffers

With pnpm:

- Shared dependencies are stored once
- Monorepos remain manageable
- Growth is linear, not exponential

This is why pnpm pairs so well with Nx and Turborepo.

---

## 9. Practical commands related to disk efficiency

### Check where the pnpm store lives

    pnpm store path

This shows the global content-addressable store location.

---

### See what is stored 

You normally don’t browse it, but you can inspect:

    ls $(pnpm store path)

You’ll see hashed directories, not project names.

---

### Remove unused packages from the store

Over time, old versions may accumulate.

To safely clean up:

    pnpm store prune

This removes packages that are no longer referenced by any project.

This is safe and recommended occasionally.

---

### Pre-warm the store 

You can prefetch packages without installing:

    pnpm store add lodash@4.17.21

Useful for:

- CI cache warming
- Offline setups

---

### Configure custom store location 

In `.npmrc`:

    store-dir=~/pnpm-store

Useful for:

- Shared CI cache volumes
- Controlled disk locations

---

## 10. Common misconception 

“Disk is cheap, so this doesn’t matter.”

Reality:

- CI minutes are expensive
- Developer waiting time is expensive
- I/O bottlenecks slow everything
- Cloud environments have limits
- SSD performance degrades under heavy load

Efficiency always matters at scale.

pnpm’s disk efficiency is not premature optimization —
it’s foundational engineering.

---

## 11. Senior-level insight

pnpm’s disk efficiency is not a side benefit.

It is a **scaling enabler**.

By treating dependencies as immutable artifacts and sharing them safely:

- Systems scale
- CI becomes affordable
- Developer experience improves automatically

This is the same philosophy used by:

- Git object storage
- Docker layers
- Build caches

Correctness and efficiency reinforce each other.

---

## One-sentence takeaway 

pnpm drastically reduces disk usage by storing each dependency file once and reusing it everywhere, which directly improves speed, cost, and scalability.
