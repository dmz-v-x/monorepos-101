## Costs of Monorepo

### 1. Costs of monorepos

This answers an uncomfortable but necessary question:

If monorepos are so good, why doesn’t everyone use them?

Because monorepos introduce **real costs**.

---

### 2. First: an important mindset reset

Monorepos are not free wins.

They **shift complexity**, not remove it.

- Polyrepo → complexity in coordination  
- Monorepo → complexity in tooling and discipline  

You are choosing **where the pain lives**.

---

### 3. Cost #1 — Tooling complexity

This is the biggest cost.

A monorepo cannot survive with basic tools alone.

You usually need:

- An advanced package manager like pnpm  
- Task orchestration tools like Nx or Turborepo  
- Smarter CI configuration  
- Caching strategies  

This leads to:

- More setup  
- More things to learn  
- More ongoing maintenance  

For small teams, this can feel heavy and overwhelming.

---

### 4. Cost #2 — CI/CD becomes harder at first

In a polyrepo:

- One repo → one pipeline  
- Simple and easy to reason about  

In a monorepo:

- Many projects → one repo  

You must:

- Detect what changed  
- Run only affected builds and tests  
- Avoid rebuilding everything  

If done incorrectly:

- CI becomes slow  
- Costs increase  
- Developers get frustrated  

Monorepos require **smarter CI**, not just more CI.

---

### 5. Cost #3 — Accidental coupling risk

In a monorepo:

- All code is visible  
- All code is technically importable  

This creates temptation:

- “I’ll just import that internal function”  
- “I’ll reuse this private utility”  

Without strong rules:

- Boundaries slowly disappear  
- Tight coupling grows silently  

This is not only a monorepo problem,  
but monorepos make bad decisions easier.

---

### 6. Cost #4 — Repository size and performance

As monorepos grow:

- Repository size increases  
- Git operations can slow down  
- Cloning can take longer  

Modern tools help a lot, but:

- Very large repos still need tuning  
- Poor Git habits can hurt productivity  

This cost is manageable, but it never goes away completely.

---

### 7. Cost #5 — Ownership and access control

In a polyrepo:

- Ownership is clear  
- Permissions are simple  

In a monorepo:

- Many teams share one repo  
- Different areas have different owners  
- Review rules become more complex  

You need:

- Clear code ownership  
- Strong review processes  
- Team discipline  

Without these:

- Confusion grows  
- Pull requests conflict  
- Teams step on each other  

---

### 8. Cost #6 — Learning curve for new engineers

New engineers may feel:

- “This repo is huge”  
- “Where do I even start?”  
- “What am I allowed to change?”  

Without good:

- Folder structure  
- Documentation  
- Tooling  

Onboarding can feel intimidating at first.

---

### 9. Cost #7 — One bad decision affects many teams

In a monorepo:

- Tooling changes affect everyone  
- Dependency upgrades affect everyone  
- CI failures block everyone  

This means:

- Changes must be more careful  
- Decisions must be well thought out  

This is power — and also responsibility.

---

### 10. A very important truth

Monorepos **amplify everything**.

- Good discipline → huge productivity gains  
- Bad discipline → huge productivity loss  

Polyrepos limit the blast radius.  
Monorepos expand it.

---

### 11. One-sentence summary

Monorepos reduce coordination cost but increase tooling, discipline, and operational complexity.
