## What is a Build

### 1. Start with the simplest idea

When you write code, computers cannot run it directly (most of the time).

You usually need to:

- Prepare it  
- Transform it  
- Bundle it  
- Optimize it  

That preparation process is called a build.

---

### 2. What is a build?

👉 A build is:

The process of turning your source code into something that can be run, deployed, or shipped.

---

### 3. Source code vs build output

**Source code**

- What you write  
- Human-friendly  

Examples:

- TypeScript  
- Modern JavaScript  
- Multiple small files  

**Build output**

- What machines run  
- Optimized  
- Combined  

Examples:

- Compiled JavaScript  
- Bundled files  
- Minified code  

👉 Build = source → output

---

### 4. Very simple example

You write:

    // src/index.ts
    const add = (a: number, b: number) => a + b;

Build process:

- Convert TypeScript → JavaScript  

Output:

    // dist/index.js
    const add = (a, b) => a + b;

👉 That conversion step is part of the build.

---

### 5. Builds are NOT just compilation

Many beginners think:

“Build = compile”

❌ That’s incomplete.

A build may include:

- Compiling (TS → JS)  
- Bundling (many files → one file)  
- Minifying (smaller size)  
- Tree-shaking (remove unused code)  
- Copying assets  
- Generating types  
- Validating configs  

👉 Build = pipeline, not one step.

---

### 6. Builds exist even when you don’t notice them

Examples:

- `npm run build`  
- `pnpm build`  
- `tsc`  
- `vite build`  
- `webpack`  
- `esbuild`  
- `bun build`  

All of these:

- Create build output  

---

### 7. Why builds exist at all

Builds solve real problems:

**1. Compatibility**

- Browsers / runtimes don’t support all syntax  

**2. Performance**

- Smaller files load faster  
- Bundled code runs faster  

**3. Safety**

- Catch errors early  
- Validate configs  

**4. Deployment**

- Servers expect predictable output  

---

### 8. Builds in backend projects

In backend systems, builds often:

- Compile TypeScript  
- Bundle code for Lambda  
- Generate OpenAPI clients  
- Prepare Docker images  

Even if you run Node directly, a build is still happening.

---

### 9. Builds + dependency graphs

Remember dependency graphs?

Example:

    shared-utils → auth → api

Build order:

- Build shared-utils  
- Build auth  
- Build api  

👉 Dependency graph decides build order.

---

### 10. Why builds become slow in monorepos

In large repos:

- Many packages  
- Many builds  
- Many dependencies  

Naive approach:

“Rebuild everything every time”

This causes:

- Slow CI  
- Slow feedback  
- Developer frustration  

👉 This is why incremental builds exist.

---

### 11. Incremental builds 

Instead of:

“Build everything”

We want:

“Build only what changed + what depends on it”

This is impossible without dependency graphs.

---

### 12. One-sentence definition

A build is the process of transforming source code into runnable or deployable output.
