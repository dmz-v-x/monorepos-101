## npm Workspaces

### 1. First: what npm actually is 

npm is:

- A package manager  
- It installs dependencies  
- It runs scripts  
- It manages `node_modules`  

Historically, npm assumed:

One repository = one package

This assumption breaks as soon as you have multiple projects in one repo.

Workspaces exist to break that assumption.

---

### 2. What “npm workspaces” means

npm workspaces tell npm:

“This repository contains multiple packages that belong together.”

In plain English:

- One repo  
- Many packages  
- Managed together  

That’s all npm workspaces are.

---

### 3. Why npm added workspaces

Before npm workspaces existed:

- Monorepos relied on hacks  
- Custom scripts glued things together  
- Tools like Lerna filled the gap  

npm added workspaces to:

- Officially support monorepos  
- Reduce fragile hacks  
- Provide a standard baseline  

npm workspaces are **official monorepo support**.

---

### 4. How npm knows it’s a workspace repo

npm reads the **root `package.json`**.

Example:

    {
      "private": true,
      "workspaces": ["apps/*", "packages/*"]
    }

What this means:

- `workspaces` lists folders that contain packages  
- npm scans those folders  
- Any folder with a `package.json` becomes a workspace package  

Why `private: true` matters:

- Prevents accidental publishing of the root package  
- Almost all monorepos should be private at the root  

---

### 5. Practical implementation: setting up npm workspaces

Step 1: create the repo root

    mkdir my-monorepo
    cd my-monorepo
    npm init -y

Step 2: edit root `package.json`

    {
      "private": true,
      "workspaces": ["apps/*", "packages/*"]
    }

This single change turns the repo into an npm workspace.

---

### 6. Creating workspace packages

Create folders:

    mkdir -p apps/api packages/auth-lib

Initialize each package:

    cd apps/api
    npm init -y
    cd ../../packages/auth-lib
    npm init -y
    cd ../../

Now you have:

- apps/api → package  
- packages/auth-lib → package  

npm sees both as workspace members.

---

### 7. Installing dependencies in an npm workspace

Run this **once at the root**:

    npm install

What npm does:

- Installs dependencies for all workspace packages  
- Creates one root `node_modules`  
- Links local workspace packages automatically  

You do NOT run `npm install` inside each package.

---

### 8. Local package linking

Suppose `apps/api` depends on `auth-lib`.

Edit `apps/api/package.json`:

    {
      "dependencies": {
        "auth-lib": "*"
      }
    }

Then run:

    npm install

What happens:

- npm detects `auth-lib` is a workspace package  
- npm links it locally  
- Nothing is downloaded from the internet  

This is how **local packages work in npm workspaces**.

---

### 9. Running scripts across workspaces

You can run scripts in all packages:

    npm run build --workspaces

Or target a specific package:

    npm run build --workspace=apps/api

This is basic orchestration, but it exists.

---

### 10. node_modules behavior in npm workspaces

npm uses **hoisting by default**.

This means:

- Dependencies are installed at the root when possible  
- Packages may access dependencies they did not declare  

This is:

- Convenient  
- But dangerous  

Hidden dependencies can appear.
This is one reason large monorepos outgrow npm workspaces.

---

### 11. What npm workspaces do NOT do

npm workspaces do NOT provide:

- Dependency graphs  
- Task orchestration based on dependencies  
- Caching  
- Incremental builds  

npm workspaces only solve:

- Dependency installation  
- Local package linking  

Everything else requires additional tooling.

---

### 12. When npm workspaces are “enough”

npm workspaces are sufficient when:

- The monorepo is small  
- Few packages exist  
- Few developers work on it  
- CI is simple  
- Performance requirements are low  

They are a **baseline**, not a scaling solution.

---

### One-sentence takeaway

npm workspaces let npm manage multiple packages inside one repository, providing basic monorepo support through shared installs and local package linking.
