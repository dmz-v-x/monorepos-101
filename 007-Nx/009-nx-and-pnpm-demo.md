## Complete Nx + pnpm Monorepo Demo  

### 0. What We Are Building

We will build a **realistic monorepo** with:

- 1 backend app (api)
- 1 worker service
- 2 shared libraries
- pnpm as package manager
- Nx for orchestration, caching, affected logic

### Final structure

	repo/
	├─ apps/
	│  ├─ api/
	│  └─ worker/
	├─ packages/
	│  ├─ auth-lib/
	│  └─ shared-utils/
	├─ nx.json
	├─ pnpm-workspace.yaml
	├─ package.json
	└─ tsconfig.base.json

This setup uses **everything you learned**.

---

### 1. Why Nx Exists

pnpm alone can:
- install dependencies
- link workspace packages
- enforce strict correctness

But pnpm **cannot**:
- decide what is affected
- cache task results
- skip unnecessary work
- optimize CI intelligently

Nx exists to:
- build graphs
- make decisions
- cache work
- eliminate recomputation

We will **prove this practically**.

---

### 2. Initial Setup (pnpm + Nx)

### 2.1 Enable pnpm (via Corepack)

	corepack enable
	corepack prepare pnpm@latest --activate

Why:
- Ensures consistent pnpm version
- Avoids global installs

---

### 2.2 Create repo & install Nx

	mkdir nx-demo
	cd nx-demo
	pnpm init -y
	pnpm add -D nx

---

### 2.3 Define pnpm workspace

Create `pnpm-workspace.yaml`

	packages:
	  - "apps/*"
	  - "packages/*"

Why this matters:
- Explicit workspace definition
- pnpm does not guess
- Nx relies on this structure

---

### 3. Nx Workspace Anatomy

### 3.1 Initialize Nx

	npx nx init --preset=apps

This creates:
- nx.json
- base tsconfig
- Nx workspace awareness

Nx now understands:
- this is a monorepo
- projects will exist inside it

---

### 4. Creating Projects (Nx Projects)

We will create **real Nx projects**, not just folders.

---

### 4.1 Create shared libraries

	nx g @nx/js:lib shared-utils --directory=packages
	nx g @nx/js:lib auth-lib --directory=packages

Each command creates:
- project.json
- src/
- proper Nx metadata

### Why this matters

Nx now knows:
- these are libraries
- they can be depended on
- they participate in the graph

---

### 4.2 Create applications

	nx g @nx/node:app api --directory=apps
	nx g @nx/node:app worker --directory=apps

Now Nx knows:
- api and worker are applications
- they can have build/test/serve targets

---

### 5. Nx Project Configuration (project.json)

Let’s inspect **apps/api/project.json**

	name: api
	projectType: application
	sourceRoot: apps/api/src

	targets:
	  build:
	    executor: @nx/js:tsc
	    outputs:
	      - dist/apps/api
	    options:
	      outputPath: dist/apps/api
	      tsConfig: apps/api/tsconfig.app.json

### What this does

- Registers a build task
- Defines inputs & outputs
- Enables caching
- Makes task graph possible

This replaces:
- manual scripts
- fragile CI logic

---

### 6. Linking Local Packages

Inside `apps/api/package.json`

	dependencies:
	  @nx-demo/auth-lib: workspace:*
	  @nx-demo/shared-utils: workspace:*

What happens:
- pnpm links local packages
- no registry lookup
- no version skew
- Nx graph becomes accurate

This is **critical**.

---

### 7. Dependency Graph 

Now the graph looks like:

	shared-utils
	     ↑
	  auth-lib
	     ↑
	      api
	      worker

Nx builds this automatically using:
- pnpm strict deps
- workspace linking
- import analysis

You never draw this graph.
Nx computes it.

---

### 8. Targets & Executors

Each project defines targets:

- build
- test
- lint

Example from `packages/auth-lib/project.json`

	targets:
	  build:
	    executor: @nx/js:tsc
	    outputs:
	      - dist/packages/auth-lib

Executors:
- define *how* tasks run
- abstract away scripts
- allow caching & orchestration

---

### 9. Running Tasks 

Run:

	nx build api

Nx will:
- build shared-utils
- build auth-lib
- build api
- in correct order

No guessing.
No manual scripting.

---

### 10. Affected Logic 

Now change a file:

	packages/shared-utils/src/date.ts

Run:

	nx affected --target=build

Nx decides:
- shared-utils affected
- auth-lib affected
- api affected
- worker affected ONLY if it depends

Unrelated projects are skipped.

pnpm **cannot do this**.
Nx can.

---

### 11. Task Graph 

Nx internally builds:

- Project graph (relationships)
- Task graph (execution order)

This allows:
- parallel execution
- dependency-aware sequencing
- skipping work safely

---

### 12. Caching

Run build once:

	nx build api

Run it again:

	nx build api

Result:

	✔ nx run api:build (cached)

Meaning:
- task not executed
- outputs restored
- zero CPU usage

This works because:
- inputs unchanged
- outputs declared
- Nx hash matches

---

### 13. Local vs Remote Cache

## Local cache
- speeds up your machine

## Remote cache (Nx Cloud)
- speeds up CI
- speeds up teammates

CI can:
- pull results
- skip execution
- finish in minutes

---

### 14. pnpm + Nx Together 

pnpm provides:
- strict dependency correctness
- no hidden imports
- deterministic installs

Nx provides:
- graph intelligence
- affected logic
- task caching
- orchestration

Together:
- correctness + performance
- safety + speed

---

### 15. CI Without Nx 

Without Nx, CI would look like:

	build everything
	test everything
	lint everything

With Nx:

	nx affected --target=lint
	nx affected --target=test
	nx affected --target=build

CI now runs:
- minimum necessary work
- based on real impact
- automatically

---

### 16. Senior-Level Insight 

Nx does **not** make things faster by magic.

Nx removes:
- duplicated work
- human guessing
- unnecessary computation

Performance is a *side effect* of correctness.

---

### 17. Final Mental Model

pnpm:
- manages dependencies

Nx:
- manages decisions

Together:
- scale monorepos safely

---

### One-Sentence Summary

Nx builds an intelligent graph on top of pnpm’s strict dependency model to decide, cache, and orchestrate exactly what work needs to run — nothing more.

