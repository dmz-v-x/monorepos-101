## Version ranges in pnpm workspaces (`workspace:*`)

### 1. Why versioning becomes confusing in monorepos

### 1.1 How versioning works in normal projects

In a normal (non-monorepo) world:

- Each package lives in its own repository
- Packages are published to npm
- Versions are how consumers coordinate

Example:

- auth-lib@1.2.3
- auth-lib@1.3.0

Consumers must decide:

- Which version to install
- When to upgrade
- Whether breaking changes exist

Here, versions are coordination tools between different repositories.

---

### 1.2 What changes in a monorepo

In a monorepo:

- Packages live together
- Code changes together
- Refactors happen in one commit
- Publishing is optional or delayed

Now ask this:

If `apps/api` and `packages/auth-lib` live in the same repo  
and change in the same commit…

What does “1.0.0 vs 1.1.0” even mean internally?

This is where traditional version thinking breaks down.

---

### 2. The naive (wrong) approach beginners try

Many beginners write this inside a monorepo:

    {
      "dependencies": {
        "@company/auth-lib": "1.0.0"
      }
    }

Why this is wrong for internal packages:

- The version number is meaningless locally
- pnpm will still link the local package
- It implies publishing semantics that do not apply
- It recreates version-skew thinking
- It brings back polyrepo problems

You gain nothing and reintroduce pain.

---

### 3. The most important mental shift

Local workspace packages are NOT downloaded.

They are:

- Not fetched from npm
- Not version-coordinated
- Not installed from the internet

They are linked directly from the same repository.

So the real question is:

“How do I tell pnpm to always use the local package?”

That is exactly what `workspace:*` exists for.

---

### 4. What `workspace:*` means 

`workspace:*` means:

Use the package that exists in this workspace repository.

Nothing more. Nothing less.

It does NOT mean:

- Any npm version
- Latest registry version
- A wildcard from the internet

It strictly means:

Local workspace package only.

---

### 5. Plain-English definition

`workspace:*` tells pnpm to always link to the local workspace package instead of downloading anything from a registry.

---

### 6. Why the `*` exists

The `*` means:

- “I don’t care about the version number”
- “As long as it’s the local workspace package, use it”

In monorepos:

- Code evolves together
- Changes are atomic
- Exact internal version numbers are irrelevant

Structure replaces version coordination.

---

### 7. How pnpm processes `workspace:*` step by step

When pnpm sees:

    "@company/auth-lib": "workspace:*"

pnpm does the following:

1. Reads pnpm-workspace.yaml
2. Finds all workspace packages
3. Locates @company/auth-lib
4. Confirms it is a local package
5. Creates a local link
6. Skips npm registry entirely
7. Enforces dependency boundaries

No network.
No publishing.
No ambiguity.

---

### 8. Why this completely eliminates version skew

Recall the classic problems:

- Some services use v1.0.0
- Others use v1.2.0
- Bugs behave differently
- Debugging becomes impossible

With `workspace:*`:

- There is only one version
- All consumers use it
- Changes apply everywhere instantly
- Drift is impossible

Version skew cannot exist internally anymore.

---

### 9. Are version numbers still useful at all?

Yes — but for different reasons.

Inside a monorepo, versions are used for:

- Eventual publishing
- Changelogs
- Release notes
- External consumers

They are NOT used for:

- Internal coordination
- Dependency correctness
- Local compatibility

Structure replaces versions internally.

---

### 10. Other workspace ranges

pnpm also supports:

- workspace:^
- workspace:~

These exist mainly for publish-oriented workflows.

In practice:

- workspace:* is simplest
- workspace:* is safest
- workspace:* is most common

Most mature teams default to `workspace:*`.

---

### 11. Common mistakes to avoid

Do NOT:

- Mix workspace:* and real versions randomly
- Use exact versions for internal-only packages
- Expect workspace:* to fetch from npm
- Treat internal packages like published ones

Rule of thumb:

If the package is internal → use workspace:*.

---

### 12. Senior-level insight

`workspace:*` replaces human version coordination with structural guarantees.

This is not just syntax.
It is a philosophical shift.

Monorepos work because structure replaces coordination.

---

### 13. One-sentence takeaway

`workspace:*` forces pnpm to always use the local workspace package, eliminating internal version coordination and version skew entirely.
