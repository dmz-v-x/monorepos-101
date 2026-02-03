## Dependency Graph & Affected Logic 

### 1. First: the hard limit of pnpm

pnpm is excellent at:

- Installing dependencies
- Linking local packages
- Enforcing strict dependency boundaries
- Running scripts with filters

But pnpm does **not** know:

- Which files changed
- Which projects are impacted by those file changes
- What can be safely skipped
- What must be rebuilt or retested

pnpm executes **what you ask it to execute**.  
Nx decides **what needs to be executed**.

This is the fundamental difference.

---

### 2. The single question Nx is built to answer

Every real system eventually hits this question:

> “I changed something — what is affected?”

When humans answer this:
- They guess
- They overbuild
- They miss cases
- They waste CI time

When Nx answers this:
- The answer is deterministic
- The answer is automated
- The answer is correct

Everything in Nx exists to answer this one question.

---

### 3. What Nx means by a “graph”

A graph is just:

- Nodes → projects
- Edges → dependencies

Nx builds **multiple graphs**, each with a specific responsibility.

This is extremely important.

---

### 4. Dependency graph vs Project graph vs Task graph

Nx works with **three different graphs**.

### Dependency Graph
- Shows package-to-package dependencies
- Conceptual idea you already know from pnpm
- Example: auth-lib depends on shared-utils

### Project Graph (Nx-specific)
- Nx’s internal representation of all projects
- Built from config + code analysis
- Used to answer “what is affected?”

### Task Graph
- Built from the project graph + requested targets
- Decides execution order
- Used when actually running tasks

Think of it like this:

Dependency graph → Project graph → Task graph  
Each layer adds more intelligence.

---

### 5. The Nx Project Graph

The **project graph** answers:

> “Which projects depend on which other projects?”

Example:

	shared-utils
	     ↑
	   auth-lib
	     ↑
	     api

This graph is:

- Project-level (not file-level yet)
- Deterministic
- Cached by Nx
- Recomputed only when needed

This is the backbone of Nx.

---

### 6. How Nx builds the project graph

Nx builds the project graph by combining **multiple sources of truth**:

### 1. Workspace configuration
- nx.json
- project.json
- inferred projects (apps/libs)

### 2. Package dependencies
- package.json dependencies
- workspace:* relationships

### 3. Static code analysis
- TypeScript imports
- JavaScript imports
- Known framework patterns

### 4. Nx plugins
- @nx/js
- @nx/node
- @nx/react
- @nx/vite
- etc.

Each plugin teaches Nx:
- How to understand that ecosystem
- What counts as a dependency
- What files matter for a project

This is why Nx is framework-aware and pnpm is not.

---

### 7. Static vs Dynamic dependencies

### Static dependencies

Dependencies Nx can detect **without running code**.

Examples:
- import statements
- require statements
- package.json dependencies

Example:

	import { login } from '@company/auth-lib'

Nx can see this at analysis time.

---

### Dynamic dependencies

Dependencies determined **only at runtime**.

Example:

	const lib = require(process.env.LIB_NAME)

Nx cannot safely reason about this.

Nx assumes:
- Static dependencies are reliable
- Dynamic dependencies are opaque

This is why good architecture matters.

---

### 8. File-level vs Project-level analysis

### Project-level analysis
- Determines which projects depend on which
- Used to build the project graph

### File-level analysis
- Determines which files changed
- Used to compute “affected”

Nx combines both:

- File changes → affected projects
- Project graph → downstream impact

This combination is the superpower.

---

### 9. What “affected” actually means

Affected means:

> “Which projects’ behavior could change because of this file change?”

Not:
- Which projects were edited
- Which projects are nearby
- Which projects are convenient to rebuild

Only:
- Which projects are impacted by dependency rules

This is a critical mental shift.

---

### 10. How Nx computes “affected”

Nx does the following:

1. Reads git diff (or provided base/head)
2. Determines which files changed
3. Maps files → owning projects
4. Uses project graph to find dependents
5. Produces a list of affected projects
6. Builds a task graph only for those projects

All automatically.

No guessing.
No scripting.
No duplication.

---

### 11. Example: simple affected scenario

You change:

	packages/shared-utils/src/date.ts

Nx determines:

- shared-utils is affected
- auth-lib depends on shared-utils → affected
- api depends on auth-lib → affected
- web-app unrelated → NOT affected

Only these run.

This is impossible with pnpm alone.

---

### 12. nx affected commands

Typical commands:

	nx affected:build
	nx affected:test
	nx affected:lint

These mean:

- Build only affected projects
- Test only affected projects
- Lint only affected projects

You do NOT specify filters.
Nx computes them.

---

### 13. Comparing pnpm filters vs nx affected

### pnpm --filter
- Manual
- Human decides scope
- Error-prone at scale
- Script-level

### nx affected
- Automatic
- Graph-based
- Deterministic
- File-aware

pnpm executes.
Nx decides.

They are complementary, not competitors.

---

### 14. Why this changes CI fundamentally

With pnpm-only CI:
- Humans write conditional logic
- CI scripts grow complex
- Errors creep in

With Nx:
- CI asks “what changed?”
- Nx answers precisely
- CI becomes simple again

This reduces:
- CI time
- CI cost
- Cognitive load

---

### 15. Senior-level insight

Nx removes *decision-making* from humans.

That is the real value.

When humans stop deciding:
- Systems become predictable
- Builds become reliable
- Teams move faster

---

### 16. One-sentence takeaway

Nx’s dependency and affected graphs allow it to automatically determine what needs to be rebuilt or retested — something pnpm cannot do by itself.


