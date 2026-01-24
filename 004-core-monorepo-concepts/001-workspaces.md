## Workspaces


## 1. First: forget monorepos for a moment

Let’s start with a basic problem.

Imagine you have:

- Multiple projects  
- Multiple `package.json` files  
- You want to manage them together  

Normally, JavaScript tooling assumes:

**One project = one `package.json`**

This assumption breaks down as soon as you have more than one project.

👉 **Workspaces exist to break this assumption.**

---

## 2. What is a “workspace”? 

A **workspace** is a way to tell your package manager:

> “This repository contains multiple packages that belong together.”

That’s it.

A workspace:

- Groups multiple packages  
- Keeps them in one repository  
- Manages them under one dependency system  

---

## 3. Simple definition

A workspace is a **collection of packages managed together inside a single repository**.

---

## 4. What problem workspaces solve

Without workspaces:

- Each package is isolated  
- Dependencies are duplicated  
- Linking local packages is manual and painful  
- Tools don’t understand relationships  

With workspaces:

- Packages are discoverable to the package manager  
- Dependencies are shared efficiently  
- Local packages can be linked automatically  
- Tooling can reason about relationships  

👉 **Workspaces are the entry point to monorepos.**

---

## 5. What counts as a “package” in a workspace?

In a workspace, a package is simply:

- A folder  
- With a `package.json`  

Examples:

- A frontend app  
- A backend service  
- A shared library  
- A utility package  

If it has a `package.json`, it can be a workspace package.

---

## 6. Visual structure 

A typical workspace layout:

    repo/  
     ├─ package.json  
     ├─ pnpm-workspace.yaml  
     ├─ apps/  
     │   ├─ api/  
     │   │   └─ package.json  
     │   └─ web/  
     │       └─ package.json  
     └─ packages/  
         ├─ shared-utils/  
         │   └─ package.json  
         └─ auth-lib/  
             └─ package.json  

Every folder with a `package.json` is part of the workspace.

---

## 7. The root package.json is special

The root `package.json`:

- Represents the workspace root  
- Holds shared scripts and dev dependencies  
- Acts as the anchor for tooling  

For **pnpm**, workspace membership itself is defined in:

**`pnpm-workspace.yaml`**

Example:

    packages:
      - "apps/*"
      - "packages/*"

This tells pnpm which folders belong to the workspace.

Note:

- npm / Yarn define workspaces inside `package.json`
- pnpm officially recommends `pnpm-workspace.yaml`

---

## 8. Important clarification

Workspaces are **NOT**:

- A build system  
- A CI/CD tool  
- A task runner  
- A cache  

They are a **package-management concept**.

They tell the package manager:

> “These packages belong together.”

---

## 9. Real-world analogy

Office building analogy:

- Workspace = the building  
- Packages = offices inside the building  

Each office works independently, but shares infrastructure.

---

## 10. Before anything: what tools you already have

When you install **Node.js**, you get:

- `node` → JavaScript runtime  
- `npm` → default package manager  
- `npx` → npm helper  

You **do not** automatically get pnpm.

And installing pnpm the wrong way causes version mismatch problems.

This is why **Corepack exists**.

---

## 11. What is Corepack?

Corepack is a tool that **ships with Node.js**.

Its job is:

> To manage package managers (pnpm, yarn) safely and consistently.

Instead of you doing:

    npm install -g pnpm

Corepack:

- Downloads pnpm for you  
- Pins the version  
- Ensures everyone uses the same one  

Think of Corepack as:

> “npm, but for package managers themselves”

---

## 12. What does `corepack enable` do?

Command:

    corepack enable

What happens:

- Corepack is activated inside Node.js  
- Commands like `pnpm` are now managed by Corepack  
- Corepack intercepts those commands  

Important:

- This does **not** install pnpm yet  
- It only turns Corepack on  

---

## 13. What does `corepack prepare pnpm@latest --activate` do?

Command:

    corepack prepare pnpm@latest --activate

Break it down:

### `corepack prepare`
Tells Corepack to download a package manager.

### `pnpm@latest`
Tells Corepack to fetch the latest stable pnpm version from the internet.

### `--activate`
Makes that version the active pnpm version.

After this:

- `pnpm` is installed  
- `pnpm` is version-controlled  
- Everyone on the team gets the same pnpm  

This is safer than `npm install -g pnpm`.

---

## 14. What is pnpm?

npm installs dependencies like this:

- Copies dependencies into every project  
- Creates large `node_modules` folders  
- Allows accidental dependency access  

pnpm works differently.

pnpm:

- Downloads each dependency once  
- Stores it in a global content-addressable store  
- Links projects to it  
- Enforces strict dependency rules  

Results:

- Faster installs  
- Less disk usage  
- Fewer hidden bugs  

This strictness is **why pnpm is preferred for monorepos**.

---

## 15. Creating the workspace root

Commands:

    mkdir my-monorepo
    cd my-monorepo
    pnpm init -y

What happens:

- You create the monorepo folder  
- You create the root `package.json`  
- This file represents the workspace root  

The root usually:

- Does not run an app  
- Holds shared config and tooling  

---

## 16. Creating `pnpm-workspace.yaml`

File:

    pnpm-workspace.yaml

Content:

    packages:
      - "apps/*"
      - "packages/*"

What this does:

- pnpm scans these folders  
- Any folder with `package.json` becomes a workspace package  

Without this file:

👉 You do **not** have a real workspace.

---

## 17. Creating workspace packages

Commands:

    mkdir -p apps/web packages/shared-utils
    cd apps/web
    pnpm init -y

Now:

- `apps/web` is a workspace package  
- It has its own `package.json`  
- pnpm recognizes it automatically  

Repeat this for APIs, libraries, workers, etc.

---

## 18. What does `pnpm install` at the root do?

Command:

    pnpm install

pnpm will:

- Read `pnpm-workspace.yaml`  
- Discover all workspace packages  
- Install dependencies for all of them  
- Create **one** `pnpm-lock.yaml`  
- Link local packages automatically  

This replaces running `npm install` in every folder.

---

## 19. Local package linking with `workspace:*`

In `apps/web/package.json`:

    "dependencies": {
      "shared-utils": "workspace:*"
    }

This means:

- Always use the local `shared-utils` package  
- Never fetch it from npm  
- Changes apply instantly  

This solves:

- Version skew  
- Publishing pain  
- Dependency hell  

---

## 20. Common commands 

- Initialize root: `pnpm init -y`  
- Install everything: `pnpm install`  
- Add dep to one package: `pnpm --filter <pkg> add <dep>`  
- Add root dev dep: `pnpm add -D -w <dep>`  

---

## Final mental model

- Corepack manages pnpm safely  
- pnpm manages dependencies efficiently  
- Workspaces group packages  
- `pnpm-workspace.yaml` defines membership  
- `workspace:*` links local code  
- One install manages everything  

---

## One-sentence takeaway

Workspaces let multiple packages live and be managed together inside one repository, while pnpm and Corepack provide the safe, efficient foundation that makes this possible.
