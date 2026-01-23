## What is Package

### 1. Start even simpler than “package”

Before the word package, think about this:

When you write code, you often want to:

- Reuse it  
- Share it  
- Keep it separate from other code  
- Install it somewhere else  

A package is how we do that.

---

### 2. What is a package?

👉 A package is:

A self-contained unit of code that is bundled together and can be reused by other code.

That’s it.

It is:

- A unit  
- With a clear purpose  
- That can be used by other projects  

---

### 3. Package ≠ file ≠ folder

Let’s clearly separate these:

**File**

- One `.js` / `.ts` file  
- Very small unit  

**Folder**

- Groups files together  
- No meaning by itself  

**Package**

- A folder with meaning  
- Has:
  - Code  
  - Metadata  
  - A clear boundary  

👉 A package is a special folder with rules.

---

### 4. What makes a folder a “package”?

In the JavaScript ecosystem, a folder becomes a package when it has:

✅ A `package.json` file

This file tells the world:

- Name of the package  
- Version  
- Entry point  
- Dependencies  
- Scripts  

Example:

    {
      "name": "auth-utils",
      "version": "1.0.0"
    }

👉 No `package.json` = not a package

---

### 5. What usually lives inside a package?

A typical package contains:

- Source code  
- `package.json`  
- Maybe tests  
- Maybe config files  
- Maybe build output  

Example structure:

    auth-utils/
      ├─ src/
      ├─ package.json
      ├─ README.md

👉 Everything inside serves one purpose.

---

### 6. Why packages exist

Packages exist to solve three big problems:

**1. Reuse**

Write code once, use it everywhere.

**2. Isolation**

Changes in one package don’t accidentally break others.

**3. Dependency management**

You explicitly say:

“This code depends on that code”

---

### 7. Packages in backend projects

In backend systems, packages often represent:

- Auth logic  
- Database access  
- Validation utilities  
- Logging utilities  
- Shared types  
- API clients  

Instead of copy-pasting code, you create a package.

---

### 8. Packages inside a monorepo

In a monorepo:

- You don’t have one package  
- You have many packages  
- All inside one repository  
- All inside one codebase  

Example:

    repo/
      ├─ apps/api        → package
      ├─ apps/worker     → package
      ├─ packages/auth   → package
      ├─ packages/db     → package

👉 Each folder with `package.json` is a package.

---

### 10. Package vs application

**Application package**

- Runs as a service (API, worker)

**Library package**

- Used by other packages  

Both are still packages.

---

### 11. Common beginner misunderstandings

❌ **“Package means npm package only”**  
→ No. Internal packages exist too.

❌ **“Only published packages are real packages”**  
→ No. Most packages are private.

❌ **“One repo = one package”**  
→ Not in monorepos.

---

### 12. One-sentence definition

A package is a self-contained unit of code, defined by a `package.json`, that can be reused and depended on by other code.
