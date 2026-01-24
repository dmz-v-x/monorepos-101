## Local Packages 

### 1. First: what does “local” mean here?

In monorepos, local does NOT mean:

- private  
- unsafe  
- hacky  
- temporary  

Local simply means:

Lives inside the same Git repository.

That’s it.

A local package is still:

- structured  
- reusable  
- importable  
- testable  

The only difference is where it lives.

---

### 2. Plain-English definition

A local package is a package that lives inside the same monorepo and can be used by other packages in that repo without downloading it from the internet.

It is still a real package.  
It just isn’t fetched from npm.

---

### 3. Why local packages exist at all

Before monorepos, shared code lived:

- In separate Git repositories  
- Published to npm or private registries  
- Versioned manually  
- Upgraded later by consumers  

This caused:

- Dependency hell  
- Version skew  
- Coordination pain  
- CI explosion  

Local packages exist to remove this friction.

---

### 4. What makes something a “package”? 

Rule:

If a folder has a `package.json`, it is a package.

Nothing more.  
Nothing less.

So this:

    packages/auth-lib/  
      └─ package.json  

Is just as much a package as:

- react  
- lodash  
- express  

The only difference is location.

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

- auth-lib is a local package  
- shared-utils is a local package  
- api is also a package (an app)  

All are peers inside the workspace.

---

### 6. How local packages are used

Inside apps/api/package.json:
    
    {
      "dependencies": {
        "@company/auth-lib": "workspace:*"
      }
    }

This tells the package manager:

Do not download this from npm.  
Use the local version inside this repository.

This is what makes the package local in practice.

---

### 7. Important mental shift 

Local packages:

- Feel like npm packages  
- Are imported the same way  
- Follow the same dependency rules  

Example import:

`import { authenticate } from "@company/auth-lib";`

From the code’s perspective, there is no difference.

The only difference is resolution:

- npm package → downloaded from registry  
- local package → linked from workspace  

---

### 8. Why this is powerful 

Because now:

- No publishing required  
- No version bumps  
- No waiting for releases  
- No version skew  
- No duplication  

Change the local package once, and all consumers update immediately.

This is the heart of monorepos.

---

### 9. Are local packages “less safe”? 

No.

Local packages can have:

- Tests  
- Clear APIs  
- Ownership  
- Lint rules  
- Type checking  

They are not less safe.

The only thing removed is coordination overhead.

---

### 10. Local packages vs shared folders

Shared folder (bad):

    utils/  
      helper.ts  

Problems:

- No clear API  
- No dependency tracking  
- Anyone can import anything  
- No ownership  

Local package (good):

    packages/shared-utils/  
     ├─ package.json  
     └─ src/  

Benefits:

- Explicit dependency  
- Clear boundary  
- Trackable usage  
- Enforced imports  

Monorepos prefer packages over folders.

---

### 11. Practical proof that this works

After adding:

`"@company/auth-lib": "workspace:*"`

Run:

pnpm install

pnpm will:

- Detect the local package  
- Link it instead of downloading  
- Keep it in sync automatically  

Verify with:

`pnpm why @company/auth-lib`

You will see it resolved from the workspace, not npm.

---

### 12. Senior-level insight

Local packages are how monorepos keep code shared while boundaries stay explicit.

This is how monorepos avoid becoming spaghetti.

---

### One-sentence takeaway 

A local package is a real package inside a monorepo that can be depended on and imported without publishing or versioning overhead.
