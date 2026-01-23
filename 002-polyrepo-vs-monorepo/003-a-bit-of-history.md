## A Bit of History

### 1. Historical context: Google and Facebook

This topic answers one big question:

Why did monorepos even exist if polyrepos already worked?

---

### 2. Early days of software

In the early days (1990s to early 2000s):

- Software projects were small  
- Teams were small  
- Apps were often one big codebase  
- CI/CD barely existed  
- Cloud did not exist  
- Microservices did not exist  

So naturally:

- One project → one repository  
- Sharing code across repositories was rare  

👉 Polyrepo was not a conscious choice — it was just the default.

---

### 3. The scale problem appears

As companies grew:

- Teams grew to hundreds  
- Products grew to thousands of features  
- Codebases became massive  
- Many systems depended on each other  

New problems started showing up:

- Shared code was duplicated everywhere  
- Refactors required changing many repositories  
- Version mismatches became common  
- Coordination overhead exploded  

Polyrepo started to hurt at scale.

---

### 4. Google’s situation

Google faced a unique challenge:

- Tens of thousands of engineers  
- Thousands of services and libraries  
- Massive shared infrastructure  
- Frequent large refactors  

If Google used polyrepos:

- A single refactor could take months  
- Version management would become impossible  
- Code consistency would slowly collapse  

So Google asked a new question:

“What if all code lived in one place?”

---

### 5. Google’s monorepo decision

Google chose:

- One massive repository  
- All code in one place  
- Very strict tooling  
- Strong ownership rules  

This enabled:

- Large-scale refactors in days instead of months  
- Consistent APIs across teams  
- Centralized tooling  
- One single source of truth  

Google still uses a monorepo today.

---

### 6. Facebook faced the same pain

Facebook had:

- A huge frontend codebase  
- Shared UI components  
- Very fast product iteration  
- Many teams touching the same code  

With polyrepos, they faced:

- Breaking changes  
- Sync issues between teams  
- Slow development speed  

So Facebook:

- Adopted a monorepo  
- Built strong internal tools  
- Optimized build systems  

Many popular tools today were created in this environment.

---

### 7. Important insight

👉 Monorepos were not created because polyrepos were bad.

They were created because:

- Polyrepos do not scale well past a certain size  
- Coordination cost becomes the real bottleneck  
- Existing tooling could not handle the complexity  

Monorepos are a **scaling solution**, not a trend.

---

### 8. Critical takeaway

Monorepos are a response to organizational scale, not a replacement for polyrepos.

This is why:

- Startups often do not need monorepos  
- Large enterprises often do  
- Choosing the wrong model causes real pain  

There is no universally correct choice.

---

### 9. One-sentence summary

Monorepos emerged when large companies outgrew polyrepos and needed a way to manage massive, fast-changing shared codebases.
