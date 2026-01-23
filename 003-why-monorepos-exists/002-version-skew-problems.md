## Version skew problems

### 1. First: what “version skew” means

**Version skew** means this:

Different parts of your system are using **different versions of the same shared code** at the same time.

Nothing crashes immediately.  
Nothing looks obviously broken.

But everything is slightly **out of sync**.

That’s what makes version skew dangerous.

---

### 2. A tiny, realistic example

You have a shared library called:

- `shared-utils`

Over time, it gets new versions:

- v1.0.0  
- v1.1.0  
- v1.2.0  

Now look at your services:

- `order-service` → `shared-utils@1.0.0`  
- `user-service` → `shared-utils@1.1.0`  
- `payment-service` → `shared-utils@1.2.0`  

All services use the **same library**,  
but **not the same version**.

👉 This situation is called **version skew**.

---

### 3. Why version skew happens naturally

This is very important:

Version skew does **not** happen because teams are bad.

It happens because:

- Teams work at different speeds  
- Some teams are busy with urgent work  
- Some upgrades feel risky  
- Some services are “stable” and untouched  

No one is doing anything wrong.

👉 The system slowly drifts on its own.

---

### 4. Why version skew is hard to notice

This is the scary part.

Even with version skew:

- Code still builds  
- Services still run  
- CI is green  
- Deployments succeed  

Nothing screams:

“HEY, SOMETHING IS WRONG!”

But underneath:

- Behavior is slightly different  
- Bugs appear only sometimes  
- Logs look different  
- Edge cases behave differently  

This is why version skew is **insidious**  
(it hides quietly and causes pain later).

---

### 5. A very common real-world bug

A bug report comes in:

> “Login fails sometimes”

Engineers investigate and find:

- `user-service` uses `validateEmail()` from v1.0.0  
- `auth-service` uses `validateEmail()` from v1.2.0  

The function changed behavior between versions.

Now:

- Same input  
- Different result  
- Depends on which service handles it  

👉 Debugging becomes a nightmare.

People start asking:

“Why does this work here but not there?”

---

### 6. Why “just upgrade everyone” is not easy

In theory, the solution sounds simple:

“Just upgrade all services to the latest version.”

In reality, this means:

- Each repo needs a pull request  
- Each pull request needs review  
- Each needs CI to pass  
- Each needs deployment  
- Some teams are busy  
- Some teams are blocked  
- Some teams are offline  

Result:

- Upgrades drag on for weeks  
- Some services upgrade, some don’t  
- Version skew continues  

The system never fully catches up.

---

### 7. Version pinning makes it worse

To feel safe, teams start doing this:

    "shared-utils": "1.1.3"

This is called **version pinning**.

It creates new problems:

- Teams fear upgrading  
- Dependencies stay frozen  
- Old bugs never get fixed  

Over time:

- Shared code stops being truly shared  
- Every service lives in its own past  

The system fragments.

---

### 8. Why semantic versioning doesn’t fully fix this

Semantic versioning helps, but it’s not magic.

Even with perfect versioning:

- Humans fear breaking changes  
- Small changes still affect logic  
- APIs evolve faster than expectations  

Semver helps manage change,  
but it **does not remove coordination cost**.

People still need to talk, plan, and upgrade.

---

### 9. The human behavior consequence

Version skew causes teams to slowly change behavior:

- Avoid shared libraries  
- Copy-paste code  
- Fork internal utilities  
- Say “don’t touch that code”  

This is not technical debt.

This is **organizational debt**.

The system becomes harder to improve over time.

---

### 10. Why monorepos eliminate version skew

In a monorepo:

- There is only **one version**  
- Shared code lives locally  
- Changes update all users together  
- No publishing  
- No version pinning  

You literally **cannot** have version skew.

Everyone always uses the same code.

This massively simplifies everything.

---

### One-sentence takeaway

Version skew happens when different services depend on different versions of shared code, leading to inconsistent behavior and extremely painful debugging.
