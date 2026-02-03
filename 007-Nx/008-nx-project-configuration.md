## Nx Project Configuration (project.json)

### 1. First: what is an Nx “project”?

In Nx, a **project** is:

- An app, library, or tool
- With a defined root folder
- With defined tasks (targets)
- With defined inputs & outputs

A project is **not**:
- Just a folder
- Just a package.json

Nx needs **explicit metadata**.

---

### 2. How Nx knows something is a project

Nx detects projects in two ways:

1. project.json files (recommended)
2. package.json with nx configuration (legacy / implicit)

Modern Nx strongly prefers **project.json**.

---

### 3. Where project.json lives

Typical structure:

	repo/
	  apps/
	    api/
	      project.json
	      src/
	  packages/
	    auth-lib/
	      project.json
	      src/

Each project owns its own configuration.

---

### 4. Plain-English definition

project.json tells Nx:
- what this project is
- what tasks it supports
- how those tasks should run
- what inputs affect them
- what outputs they produce

---

### 5. Minimal project.json example

	api/project.json

	{
	  "name": "api",
	  "projectType": "application",
	  "sourceRoot": "apps/api/src",
	  "targets": {}
	}

This alone registers a project in Nx.

---

### 6. projectType

	projectType: "application" | "library"

Nx uses this to:
- apply defaults
- influence generators
- organize graph semantics

Rule of thumb:
- apps → application
- shared code → library

---

### 7. sourceRoot

	sourceRoot: "apps/api/src"

Nx uses sourceRoot to:
- track file-level changes
- compute affected projects
- improve graph accuracy

Without sourceRoot:
affected logic becomes weaker.

---

### 8. Targets

Targets define **what tasks a project can run**.

Examples:
- build
- test
- lint
- serve
- typecheck

Targets replace ad-hoc scripts.

---

### 9. Basic target definition

	{
	  "targets": {
	    "build": {
	      "executor": "@nx/js:tsc"
	    }
	  }
	}

Meaning:
- This project has a build task
- Nx knows how to run it

---

### 10. Executors

An executor is:
- A function
- Provided by Nx or plugins
- That runs a task

Examples:
- @nx/js:tsc
- @nx/node:build
- @nx/jest:test
- nx:run-commands

Executors are **how work is performed**.

---

### 11. Options

Targets usually have options:

	{
	  "build": {
	    "executor": "@nx/js:tsc",
	    "options": {
	      "outputPath": "dist/apps/api",
	      "tsConfig": "apps/api/tsconfig.json"
	    }
	  }
	}

Options are executor-specific.

---

### 12. Inputs 

Inputs define:
- what files affect this task

Example:

	"inputs": [
	  "default",
	  "^default"
	]

Meaning:
- this project’s files
- dependency project files

Nx hashes inputs to decide caching.

---

### 13. Named inputs

Defined in nx.json:

	namedInputs:
	  default:
	    - "{projectRoot}/**/*"
	  production:
	    - default
	    - "!{projectRoot}/**/*.spec.ts"

Targets reference named inputs.

---

### 14. Outputs

Outputs tell Nx:
- what files this task produces

Example:

	"outputs": ["{options.outputPath}"]

Nx restores outputs from cache instead of re-running tasks.

---

### 15. Cache behavior per target

	cache: true | false

Example:

	"build": {
	  "cache": true
	}

Most build/test/lint targets should be cached.

---

### 16. DependsOn

dependsOn defines task ordering:

	"dependsOn": ["^build"]

Meaning:
- build dependencies first
- then this project

This is **task graph control**.

---

### 17. Multiple configurations

Targets can have configurations:

	"configurations": {
	  "production": {
	    "optimization": true
	  },
	  "development": {
	    "optimization": false
	  }
	}

Run with:

	nx build api --configuration=production

---

### 18. Replacing package.json scripts

Instead of:

	"scripts": {
	  "build": "tsc"
	}

Use:

	project.json targets

Benefits:
- dependency-aware
- cacheable
- orchestrated
- CI-friendly

Scripts are dumb.
Targets are intelligent.

---

### 19. When to keep scripts

Keep scripts when:
- task is purely local
- task is one-off
- task does not need caching
- task does not affect others

Move to Nx when:
- task is shared
- task is slow
- task affects dependencies
- task runs in CI

---

### 20. Common mistakes 

❌ Writing outputs without declaring them  
❌ Using run-commands without inputs  
❌ Relying on relative imports  
❌ Overloading one target with many behaviors  

Nx rewards clarity.

---

### 21. Senior-level insight

project.json is not configuration clutter.
It is **executable architecture documentation**.

If project.json is clean:
- graphs are accurate
- caching is correct
- CI is fast
- refactors are safe

---

### 22. One-sentence takeaway 

project.json defines how Nx understands, executes, and optimizes work for a project.


