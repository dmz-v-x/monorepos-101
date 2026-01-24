## Local Packages

### 1. First: what does “local” mean here?

In monorepos, local does NOT mean “private”
and it does NOT mean “not reusable”.

👉 Local means: “lives inside the same repository”

That’s it.

---

### 2. Plain-English definition

A local package is a package that lives inside the same monorepo and can be used by other packages in that repo.

It’s still a real package.
It just isn’t downloaded from the internet.

---

### 3. Why local packages exist at all

Before monorepos, shared code lived:

- In separate repos
- Published to npm
- Versioned manually

This caused:

- Dependency hell
- Version skew
- Coordination pain

Local packages exist to remove that friction.

---

### 4. What makes something a “package”?

Very important rule:

👉 If a folder has a package.json, it is a package.

That’s it.

So this:

    packages
      └─auth-lib
        └─ package.json


Is a package, just like something on npm.

---

### 5. Example monorepo with local packages
    repo/
     ├─ apps/
     │   └─ api/
     │       └─ package.json
     └─ packages/
         ├─ auth-lib/
         │   └─ package.json
         └─ shared-utils/
             └─ package.json


Here:

auth-lib is a local package

shared-utils is a local package

api is also a package (an app)

All are peers inside the workspace.

---

### 6. How local packages are used

Inside `api/package.json`:

    {
      "dependencies": {
        "@company/auth-lib": "workspace:*"
      }
    }


This tells the package manager:

“Don’t download this from npm.
Use the local version inside this repo.”

This is what makes it local.

---

### 7. Important mental shift

Local packages:

- Feel like npm packages
- Are imported the same way
- But are resolved locally

Example import:

`import { authenticate } from "@company/auth-lib";`


The code looks the same as npm.
The difference is where it comes from.

---

### 8. Why this is powerful

Because now:

- No publishing required
- No version bumps
- No waiting for releases
- No version skew
- No duplication

Change the local package →
All consumers update immediately.

This is the heart of monorepos.

---

### 9. Are local packages “less safe”?

No.

Local packages:

- Can have tests
- Can have boundaries
- Can have APIs
- Can be versioned later if needed

The difference is:
👉 Coordination cost is removed

---

### 10. Local packages vs shared folders

❌ Shared folder (bad):

utils/
  helper.ts


Problems:

- No clear API
- No ownership
- No dependency tracking

✅ Local package (good):

    packages/shared-utils/
      package.json
      src/


Benefits:

- Explicit dependency
- Clear boundary
- Trackable usage

Monorepos prefer packages over folders.

---

### 11. Senior-level insight 

Local packages are how monorepos keep code shared but boundaries explicit.

This is how monorepos avoid becoming spaghetti.

---

### One-sentence takeaway 

A local package is a real package inside a monorepo that can be depended on without publishing or versioning overhead.
