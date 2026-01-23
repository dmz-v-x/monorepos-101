## What is CI/CD

### 1. First: the problem CI/CD solves

Imagine this situation:

You write code on your laptop  
It works for you  
You push it to GitHub  
Someone else pulls it  
It breaks  
Production breaks  
Panic

Why does this happen?

Because:

- Code is not tested consistently  
- Builds are manual  
- Deployment is manual  
- Humans make mistakes  

👉 CI/CD exists to remove humans from repetitive, risky steps

---

### 2. What does CI/CD stand for?

CI/CD has two parts:

- CI = Continuous Integration  
- CD = Continuous Delivery (or Continuous Deployment)  

---

### 3. What is Continuous Integration (CI)?

👉 Continuous Integration means:

Automatically checking and building your code every time it changes.

Whenever you:

- Push code  
- Open a pull request  
- Merge a branch  

CI automatically:

- Installs dependencies  
- Builds the code  
- Runs tests  
- Reports success or failure  

No human clicks.  
No “it works on my machine”.

---

### 4. Simple CI example

You push code.

CI does this automatically:

- Get latest code  
- Install packages  
- Run build  
- Run tests  
- Say ✅ or ❌  

If something fails:

- Code is rejected  
- Bug is caught early  

👉 CI = early error detection

---

### 5. What is Continuous Delivery (CD)?

👉 Continuous Delivery means:

Automatically preparing your code so it is always ready to be deployed.

After CI passes:

- Code is packaged  
- Artifacts are created  
- Deployment-ready output exists  

But:

- A human may still click “deploy”

---

### 6. What is Continuous Deployment?

👉 Continuous Deployment means:

Automatically deploying code to production when CI passes.

- No button  
- No approval  
- Fully automated  

Not every company does this — but many do.

---

### 7. CI vs CD

| Part            | What it does       |
| --------------- | ------------------ |
| CI              | Checks correctness |
| CD (Delivery)   | Prepares release   |
| CD (Deployment) | Ships to users     |

- CI is mandatory  
- CD is optional but powerful  

---

### 8. Where CI/CD runs

CI/CD runs on servers, not your laptop.

Common platforms:

- GitHub Actions  
- GitLab CI  
- Jenkins  
- CircleCI  
- Azure DevOps  

They:

- Watch your repository  
- React to changes  
- Run workflows  

---

### 9. CI/CD is just automation

CI/CD is not magic.

It’s:

- Scripts  
- Commands  
- Run in the right order  
- On every change  

Example commands inside CI:

    pnpm install
    pnpm build
    pnpm test

Same commands you run locally — just automated.

---

### 10. How CI/CD connects to builds

Remember builds?

CI always includes:

- Builds  
- Tests  

If build fails:

- CI fails  
- Code is blocked  

👉 Builds are a core part of CI

---

### 11. Why CI/CD is critical for monorepos

In monorepos:

- One repo  
- Many packages  
- Many teams  
- Many dependencies  

Without CI/CD:

- One bad change breaks everyone  
- No one knows where the break came from  

With CI/CD:

- Changes are validated automatically  
- Only affected packages are built  
- Teams can move fast safely  

---

### 12. Why monorepo CI/CD is hard

Naive CI:

“Build and test everything”

Problems:

- Slow  
- Expensive  
- Frustrating  

Modern CI:

“Build and test only what changed”

This requires:

- Dependency graphs  
- Affected detection  
- Caching  

👉 This is why tools like Nx and Turborepo exist.

---

### 13. One-sentence definition

CI/CD is an automated process that builds, tests, and prepares or deploys code every time it changes.
