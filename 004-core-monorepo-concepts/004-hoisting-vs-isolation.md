## Hoisting vs Isolation

### 1. First: what problem are we even solving?

In a monorepo you have:

- Many packages  
- Many dependencies  
- Often the same dependency used everywhere  

Examples:

- react  
- lodash  
- zod  

The core question is:

Where should these dependencies be installed?

This question leads to **hoisting vs isolation**.

---

### 2. What “hoisting” means

Hoisting means **moving shared dependencies up to a common place**.

Instead of each package having its own copy:

- One shared copy is placed higher  
- Everyone uses it  

Usually, dependencies are hoisted to:

- the repository root  

---

### 3. Hoisting visualized

Without hoisting:

    api/node_modules/lodash  
    web/node_modules/lodash  
    auth-lib/node_modules/lodash  

With hoisting:

    node_modules/lodash   ← shared  
    api/  
    web/  
    auth-lib/  

Same dependency.  
One physical copy.

---

### 4. Why hoisting exists

Hoisting gives you:

- Smaller disk usage  
- Faster installs  
- Less duplication  
- Faster dependency resolution  

This is why tools like:

- npm  
- yarn (classic)  

defaulted to hoisting.

---

### 5. The hidden problem with hoisting 

Hoisting creates **implicit dependencies**.

A package can:

- Import a dependency  
- Even if it is NOT listed in its own `package.json`  

Because the dependency exists “somewhere above” in the folder tree.

Example:

`import lodash from "lodash";`

But `lodash` is not declared.

What happens:

- It works locally  
- It breaks in CI or production  

This is dangerous and hard to debug.

---

### 6. What “isolation” means

Isolation means **each package only sees what it explicitly declares**.

If a package did not list a dependency:

- It cannot import it  
- The code fails immediately  

Isolation enforces discipline.

---

### 7. Isolation visualized

    api/node_modules/  
      lodash  

    auth-lib/node_modules/  
      zod  

Each package:

- Has its own dependency view  
- Cannot accidentally use another package’s dependencies  

---

### 8. Why isolation is safer 

Isolation ensures:

- Correct dependency declarations  
- No hidden coupling  
- Reproducible builds  
- Fewer “works on my machine” bugs  

At scale, this safety matters more than convenience.

---

### 9. The trade-off 

Hoisting:

- Faster installs  
- Less disk usage  
- More magic  
- Risk of hidden dependencies  

Isolation:

- Safer dependencies  
- Stricter correctness  
- More explicit  
- No hidden dependencies  

Neither approach is always correct.  
Each has costs and benefits.

---

### 10. How modern monorepos handle this

Modern tools try to combine:

- Hoisting efficiency  
- Isolation correctness  

Example: **pnpm**

pnpm:

- Uses a global content-addressable store  
- Hard-links dependencies instead of copying  
- Enforces strict dependency access  

Result:

- Fast installs  
- Low disk usage  
- No hidden dependencies  

You get both speed and safety.

---

### 11. Why this matters for you

If you don’t understand hoisting vs isolation:

- `node_modules` feels random  
- Imports seem to work “by magic”  
- Builds fail unexpectedly in CI  

Understanding this concept explains **most dependency weirdness** in JavaScript projects.

---

### One-sentence takeaway

Hoisting shares dependencies to reduce duplication, while isolation ensures each package can only use what it explicitly declares.
