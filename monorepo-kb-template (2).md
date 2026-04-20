# 🤖 Monorepo Knowledge Base — AI Agent Generation Template

---

## ⚙️ AGENT INSTRUCTIONS (Read This First)

You are an AI agent with read access to a frontend monorepo codebase. Your task is to **generate a complete Knowledge Base markdown document** by analysing the codebase and populating every section below.

This Knowledge Base will be used by an AI assistant to answer questions from developers, QA engineers, analysts, and non-technical stakeholders. The assistant must be able to answer questions like:

- *"What is the monorepo manager for this project?"*
- *"Give me the route for the user profile page in app X."*
- *"What port does the dashboard app run on locally?"*
- *"What shared hooks are available?"*
- *"How do I create a new library package?"*
- *"What TypeScript types are shared across apps?"*
- *"Which packages depend on the UI component library?"*
- *"What's the test strategy for QA?"*
- *"Who owns the payments app?"*
- *"What external services does this project depend on?"*

**Every section must be detailed enough to answer that category of question without the AI needing to look at the actual code.**

---

### How to Use This Template

- Every section has a `<!-- AGENT: -->` block describing **exactly what to inspect, read, or infer** from the codebase.
- After gathering data, **replace the `<!-- AGENT: -->` block and placeholder text** with real, accurate information.
- If a section does not apply, write `> Not applicable — [brief reason].` Do not leave sections blank.
- If a value cannot be determined confidently, write `> ⚠️ Could not be determined automatically. Manual review recommended.`
- Write **for a mixed audience** — technical sections can be precise, but overview/glossary sections must be readable by non-technical users.
- When output is complete, **remove this entire `⚙️ AGENT INSTRUCTIONS` section** from the final document.

---

### Codebase Discovery — Run First

Before writing any section, perform these discovery steps and keep results in working memory:

```
STRUCTURE
  1. List root directory (depth 3, exclude node_modules, .git, dist/, .next/, out/, .turbo/, .nx/)
  2. Read: package.json (root)
  3. Read: README.md (root)
  4. Read: CONTRIBUTING.md (if present)
  5. Read: CHANGELOG.md (if present)

MONOREPO TOOL
  6. Check for: nx.json → Nx
  7. Check for: lerna.json → Lerna
  8. Check for: turbo.json → Turborepo
  9. Check for: pnpm-workspace.yaml → pnpm Workspaces
  10. Check for: workspaces field in root package.json → Yarn/npm Workspaces

PACKAGES
  11. List all dirs under apps/, packages/, libs/ (or equivalent)
  12. Read package.json for every app and package/lib: name, description, version, dependencies, devDependencies, scripts, private, publishConfig

ROUTING
  13. For each app, search for router config files:
      React Router: src/routes.*, src/router.*, src/App.tsx (look for <Route> / createBrowserRouter)
      Next.js: list app/ or pages/ directory structure recursively
      TanStack Router: routeTree.gen.ts, src/routes/ directory
      Remix: app/routes/ directory
  14. For each app, list all route paths, their components, and any guard/auth wrapper

CONFIG & ENV
  15. Read tsconfig.base.json or tsconfig.json (root) — compilerOptions, paths aliases
  16. Read .eslintrc.* or eslint.config.* (root)
  17. Read jest.config.* or vitest.config.* (root and per-package)
  18. Read playwright.config.* or cypress.config.* (if present)
  19. Read .env.example / .env.template (root and per-app)
  20. Read .nvmrc / .node-version / .tool-versions

CI/CD
  21. Read all files in .github/workflows/ or .gitlab-ci.yml or Jenkinsfile or .circleci/config.yml or azure-pipelines.yml

SHARED CODE
  22. For the UI component library package: list all exported components (read its index file or barrel exports)
  23. For any shared hooks/utils package: list exported hooks and utilities
  24. For any types/models package: list exported types and interfaces
  25. Scan for shared constants, i18n config, feature flag definitions

TOOLING
  26. Check for .storybook/ or storybook/ directory — read main.ts/main.js for addons and config
  27. Check for code generators: tools/generators/, plopfile.js, _templates/, nx generate
  28. Check for .husky/ directory → list hooks
  29. Check for commitlint.config.* or .czrc
  30. Check for Docker: Dockerfile, docker-compose.yml, docker-compose.dev.yml

EXTERNAL INTEGRATIONS
  31. Scan all package.json files for known third-party SDKs (analytics, monitoring, payments, auth, feature flags, etc.)
  32. Scan .env.example for service URLs and API keys (document variable names only, not values)
```

---

## 📌 Quick Reference

<!-- AGENT:
Populate from discovery steps 2, 6-10, 20.
- Project Name: root package.json → name or description, or README title
- Monorepo Tool: whichever config file was found in steps 6-10 (include version from devDependencies)
- Package Manager: detect from lockfile (yarn.lock → Yarn, pnpm-lock.yaml → pnpm, package-lock.json → npm) and packageManager field in package.json
- Primary Language: TypeScript if tsconfig.json or .ts files present, else JavaScript
- React Version: find "react" in root or shared package dependencies
- Node Version: from .nvmrc / .node-version / engines field / .tool-versions
- Total Apps: count of directories in apps/ (or equivalent)
- Total Packages: count of directories in packages/ or libs/ (or equivalent)
- Repository URL: package.json → repository field, or git remote origin
- CI/CD: from step 21
- Environments: infer from CI deploy jobs or .env.example comments
-->

| Property | Value |
|---|---|
| **Project Name** | |
| **Monorepo Tool** | |
| **Package Manager** | |
| **Primary Language** | |
| **Component Library** | React |
| **Node Version** | |
| **Total Apps** | |
| **Total Shared Packages** | |
| **Repository URL** | |
| **CI/CD Platform** | |
| **Environments** | |

---

## 1. 🧭 Project Overview

### 1.1 What Is This Project?

<!-- AGENT:
Synthesise a plain-English 3–5 sentence summary from: README, root package.json description, and app/package names.
Answer: What does this monorepo do? What products does it power? Who uses those products?
Write for a non-technical reader (product manager, analyst, business stakeholder). Avoid jargon.
-->

### 1.2 Business Context

<!-- AGENT:
- Products Served: list each app and its apparent purpose (infer from name + README + package.json description)
- Primary Users: infer from app names (e.g. "customer-portal" → external customers, "admin-dashboard" → internal staff, "partner-hub" → B2B partners)
- Business Domain: infer from dependencies and naming (Stripe → payments, @auth0 → identity, health-related names → healthcare, etc.)
-->

- **Products Served:**
- **Primary Users / Consumers:**
- **Business Domain:**

### 1.3 Key Capabilities

<!-- AGENT:
List 4–6 high-level things this codebase enables, written for a non-technical reader.
e.g. "Shared UI components used across all products", "Centralised authentication for all apps", "Coordinated deployments with a single command"
Base these on what the apps and packages actually do.
-->

---

## 2. 🗂️ Repository Structure

### 2.1 Top-Level Layout

<!-- AGENT:
Produce an annotated ASCII tree of the repo root (depth 2–3).
Exclude: node_modules, .git, dist/, .next/, out/, .turbo/, .nx/, .cache/
Add a brief comment after each directory/file explaining its purpose.
Base comments on directory conventions, package.json descriptions, and README.
-->

```
[Generated tree]
```

### 2.2 Apps

<!-- AGENT:
For each app (discovery step 11-12):
- Name: package.json → name
- Description: what the app does (README or package.json description, or infer from name)
- Framework: detect from dependencies (Next.js, Vite+React, Remix, CRA, etc.)
- Local Port: check scripts.dev / vite.config (server.port) / next.config (port in dev script) / .env.example (PORT variable)
- Local Dev URL: http://localhost:[port] or from README
- Environments: infer from CI deploy jobs (which branch deploys where)
One row per app.
-->

| App (`package.json` name) | Description | Framework | Local Port | Dev URL | Environments |
|---|---|---|---|---|---|

### 2.3 Shared Packages & Libraries

<!-- AGENT:
For each package/lib (discovery step 11-12):
- Name: package.json → name
- Type: UI components / hooks / utilities / types / tokens / config / api-client / etc. (infer from name and exports)
- Description: what it provides (README or package.json description)
- Consumed By: search for this package name in the dependencies of all other packages/apps
- Published: private:true in package.json → No; publishConfig present → Yes (to which registry)
One row per package.
-->

| Package Name | Type | Description | Consumed By | Published |
|---|---|---|---|---|

### 2.4 Project Dependency Graph

<!-- AGENT:
Map out which packages depend on which — this is the internal dependency graph.
Read each package/app's package.json and list its internal dependencies (packages from this same monorepo, identified by the shared @scope prefix or workspace:* version).
Present as a structured list:

[app-name]
  depends on → @scope/pkg-a, @scope/pkg-b

@scope/pkg-a
  depends on → @scope/pkg-c

Then write 1–2 sentences explaining what this means for build order and affected commands.
If the monorepo tool has a graph command (nx graph, turbo run build --dry), note how to visualise it.
-->

```
[Dependency graph]
```

### 2.5 Package Naming Convention

<!-- AGENT:
From all package.json names, identify:
1. The npm scope (e.g. @acme)
2. The naming pattern (e.g. @scope/domain-type)
3. Show real examples from the codebase
-->

---

## 3. ⚙️ Monorepo Tooling

### 3.1 What Monorepo Tool Is Used?

<!-- AGENT:
State clearly and directly which tool is used, the version, and the config file.
This section must be answerable at a glance — it is one of the most common questions asked.

Format:
**Tool:** [Nx / Lerna / Turborepo / pnpm Workspaces / Yarn Workspaces / npm Workspaces]
**Version:** [exact version from devDependencies]
**Config File:** [filename at repo root]
**Why Chosen:** [from README/CONTRIBUTING/ADR docs; if not documented, infer from project scale and tool strengths]

Then include the relevant <details> block below and delete the others.
-->

**Tool:**
**Version:**
**Config File:**
**Why Chosen:**

---

<details>
<summary>🔷 Nx — Full Configuration</summary>

<!-- AGENT:
1. Paste the full contents of nx.json
2. Identify and list all defined targets (build, test, lint, serve, e2e, etc.)
3. Note cacheableOperations or cacheableTargets — which tasks are cached
4. Note namedInputs — any custom input groups
5. Note defaultBase — the base branch for `nx affected`
6. Note any plugins registered
7. For 2–3 representative projects, read their project.json and summarise their targets
8. Note whether Nx Cloud is configured (nxCloudAccessToken in nx.json or nx-cloud.env file)

Then generate the real commands for this repo based on actual target names found.
-->

**nx.json:**
```json
[Full nx.json contents]
```

**Defined Targets:**

| Target | Cacheable | Depends On | Notes |
|---|---|---|---|

**Affected Base Branch:**
**Nx Cloud:** [Enabled / Not configured]

**Key Commands:**
```bash
# View project graph
nx graph

# Run a target for one project
nx run [project]:[target]

# Run a target for all projects
nx run-many --target=[target] --all

# Run only affected projects (since last commit to base branch)
nx affected --target=[target]

# Clear local cache
nx reset

# List available generators
nx list
```

</details>

<details>
<summary>🔶 Lerna — Full Configuration</summary>

<!-- AGENT:
1. Paste the full contents of lerna.json
2. Note the versioning mode (independent or fixed)
3. Note packages glob
4. Note npmClient and useWorkspaces
5. Note command.version and command.publish blocks
6. Note whether conventional commits are enabled

Generate real commands based on config.
-->

**lerna.json:**
```json
[Full lerna.json contents]
```

**Versioning Mode:** [Fixed / Independent]
**Conventional Commits:** [Yes / No]

**Key Commands:**
```bash
[Real lerna commands derived from config]
```

</details>

<details>
<summary>🟡 Turborepo — Full Configuration</summary>

<!-- AGENT:
1. Paste the full contents of turbo.json
2. List all pipeline tasks with their dependsOn, outputs, and cache setting
3. Note any globalDependencies
4. Note whether remote caching is configured (TURBO_TOKEN / TURBO_TEAM in .env.example or CI)

Generate real commands based on pipeline task names.
-->

**turbo.json:**
```json
[Full turbo.json contents]
```

**Pipeline Tasks:**

| Task | Depends On | Cached | Outputs |
|---|---|---|---|

**Remote Cache:** [Configured / Not configured]

**Key Commands:**
```bash
[Real turbo commands derived from pipeline]
```

</details>

<details>
<summary>🟢 pnpm / Yarn / npm Workspaces — Full Configuration</summary>

<!-- AGENT:
1. Paste workspace config (pnpm-workspace.yaml or package.json[workspaces])
2. Note .npmrc or .yarnrc.yml hoisting/linker settings
3. Note root-level scripts that orchestrate cross-package work
4. Note any postinstall or prepare scripts

Generate real cross-package commands.
-->

**Workspace Config:**
```yaml
[Workspace config contents]
```

**Hoisting / Linker Settings:**

**Key Commands:**
```bash
[Real workspace commands]
```

</details>

---

### 3.2 Root-Level Workspace Scripts

<!-- AGENT:
Read root package.json → scripts.
List every script defined at the root level with its command and what it does.
These are the commands developers run most often.
-->

| Script | Command | What It Does |
|---|---|---|

### 3.3 Code Generators & Scaffolding

<!-- AGENT:
Search for code generation tools:
1. Nx generators: run `nx list` or check tools/generators/ for custom generators; also check for @nx/react, @nx/js, @nx/node generators
2. Plop: check for plopfile.js, plopfile.ts, .plop/ directory
3. Hygen: check for _templates/ directory
4. Custom scripts: any script named generate, scaffold, create in root or tools/

For each generator/template found, document:
- What it generates (component, library, app, page, hook, etc.)
- The command to run it
- What files/folders it creates
- Any prompts it asks

This is important for developers asking "how do I create a new X?"
-->

| Generator | What It Creates | Command | Files Created |
|---|---|---|---|

### 3.4 Git Hooks

<!-- AGENT:
Check for .husky/ directory and list each hook file with what it runs.
Common hooks: pre-commit (lint-staged, typecheck), commit-msg (commitlint), pre-push (tests).
Also check for lint-staged.config.* or lint-staged field in package.json — list what it runs on which file patterns.
If no hooks are configured, state that explicitly.
-->

| Hook | Tool | What It Runs |
|---|---|---|

---

## 4. 🧱 Technology Stack

### 4.1 Core Technologies

<!-- AGENT:
Scan all package.json files (root + apps + packages) to build this table.
Use the most common version for shared deps. Document per-app differences in 4.2.

Detection map:
- Build tool: vite → Vite | webpack / next → Next.js bundler | @rspack → Rspack | rollup → Rollup | parcel → Parcel | esbuild → esbuild
- Styling: styled-components / @emotion → CSS-in-JS | tailwindcss → Tailwind | @vanilla-extract → Vanilla Extract | sass → Sass | *.module.css pattern → CSS Modules | no package → plain CSS
- State: @reduxjs/toolkit / redux → Redux Toolkit | zustand → Zustand | jotai → Jotai | recoil → Recoil | mobx → MobX | xstate → XState | @tanstack/store → TanStack Store
- Routing: react-router-dom → React Router | @tanstack/router → TanStack Router | next → Next.js App/Pages Router | @remix-run → Remix
- Data fetching: @tanstack/react-query / react-query → TanStack Query | swr → SWR | @apollo/client → Apollo | @trpc/client → tRPC | urql → urql
- Forms: react-hook-form → RHF | formik → Formik | @tanstack/react-form → TanStack Form
- Unit test: jest → Jest | vitest → Vitest
- Component test: @testing-library/react → RTL
- E2E: @playwright/test / playwright → Playwright | cypress → Cypress
- Linting: eslint; read extends/plugins for the rule set (airbnb, standard, recommended)
- Formatting: prettier
- API type: graphql → GraphQL | @trpc/server → tRPC | openapi/swagger spec present → REST+OpenAPI | axios/fetch without schema → REST
-->

| Layer | Technology | Version | Notes |
|---|---|---|---|
| **Component Library** | React | | |
| **Language** | | | |
| **Build Tool** | | | |
| **Styling** | | | |
| **State Management** | | | |
| **Routing** | | | |
| **Data Fetching** | | | |
| **Form Handling** | | | |
| **Testing — Unit** | | | |
| **Testing — Component** | | | |
| **Testing — E2E** | | | |
| **Linting** | | | |
| **Formatting** | | | |
| **API Layer** | | | |

### 4.2 Per-App Technology Differences

<!-- AGENT:
Compare each app's package.json against the core stack.
List ONLY genuine differences — a different tool, a significantly different major version, or an additional tool not used elsewhere.
If all apps share the same stack: write "> All apps use the same core technology stack."
-->

| App | What Differs | Reason (if documented) |
|---|---|---|

### 4.3 TypeScript Configuration

<!-- AGENT:
Read tsconfig.base.json or tsconfig.json (root):
1. List all path aliases defined in compilerOptions.paths (e.g. @components/* → packages/ui/src/*)
   These aliases are essential for developers — show the full mapping as a code block
2. Note strict mode setting
3. Note target and lib
4. Note moduleResolution
5. Note any project references

Then for 1–2 apps, show how their tsconfig.json extends the base.
-->

**Path Aliases:**
```json
{
  "paths": {
    // [Real aliases from tsconfig]
  }
}
```

**Key Settings:**

| Setting | Value | Notes |
|---|---|---|
| strict | | |
| target | | |
| moduleResolution | | |
| baseUrl | | |

### 4.4 Shared Configuration Files

<!-- AGENT:
List every shared config file found at root or in a config/ folder.
For each: path, what it's for, and which packages extend/use it.
-->

| File | Path | Purpose | Extended By |
|---|---|---|---|

---

## 5. 🛣️ Application Routes

<!-- AGENT:
This section is CRITICAL. Developers and QA ask about routes constantly.
For EVERY app, map out ALL routes completely.

Detection strategy:
- Next.js (App Router): recursively list app/ directory. Each folder = a route segment. Files: page.tsx = rendered page, layout.tsx = layout, loading.tsx, error.tsx, not-found.tsx. Dynamic segments: [param] → :param. Route groups: (group) → no URL segment.
- Next.js (Pages Router): recursively list pages/ directory. Each .tsx/.jsx file = a route. _app.tsx and _document.tsx are not routes. Dynamic: [param].tsx → :param. Catch-all: [...slug].tsx
- React Router (v6): read the router config file (createBrowserRouter / <Routes> in App.tsx or routes.tsx). Extract every <Route path="..."> element or route object, its component, and any loader/action.
- TanStack Router: read routeTree.gen.ts or the routes/ directory structure.
- Remix: read app/routes/ directory structure. Each file = a route. Nested: parent.child.tsx → /parent/child.
- Vue Router / others: adapt accordingly.

For each route, document:
- Path: the URL pattern (e.g. /users/:id)
- Description: what this page/view does in plain English
- Auth Required: does the route require login? (look for auth guards, ProtectedRoute wrapper, middleware)
- Roles: any role restrictions (e.g. admin only)
- Component: the main component file rendered

Organise by app. If an app has >15 routes, group them by domain (auth, users, settings, etc.).
-->

### 5.1 [App Name] Routes

| Path | Description | Auth Required | Roles | Component |
|---|---|---|---|---|
| `/` | | | | |

### 5.2 [App Name] Routes

| Path | Description | Auth Required | Roles | Component |
|---|---|---|---|---|

<!-- AGENT: Repeat for each app found. -->

### 5.3 Route Guards & Navigation Patterns

<!-- AGENT:
Document how route protection is implemented:
1. How is authentication enforced on protected routes? (ProtectedRoute wrapper, middleware.ts in Next.js, loader redirect, etc.)
2. What happens when an unauthenticated user hits a protected route? (redirect to /login, 401 page, etc.)
3. Are there role-based route guards? How are they implemented?
4. Is there a 404 page? Where is it?
5. Are there any redirects defined? (e.g. / → /dashboard when logged in)

Show real code snippets from the route guard implementation (the guard component/middleware, not the full file).
-->

---

## 6. 🚀 Getting Started

### 6.1 Prerequisites

<!-- AGENT:
From discovery steps 2, 9, 20:
- Node version: .nvmrc or engines field
- Package manager + version: packageManager field in package.json or lockfile
- Global CLIs needed: check if nx, turbo, etc. are used as globals vs npx
- Docker: Dockerfile or docker-compose.yml present
- Any other tools: from README prerequisites section or setup scripts
-->

| Requirement | Version | Install |
|---|---|---|

### 6.2 First-Time Setup

<!-- AGENT:
Read in order: README → CONTRIBUTING → root scripts (setup, bootstrap, prepare, postinstall) → any setup.sh or Makefile.
Generate the complete, runnable sequence of commands a new developer must run to get from a fresh clone to a working local environment.
Include: clone, install, env file setup, any code generation / build step required before running.
Use the ACTUAL package manager and monorepo tool commands.
-->

```bash
# [Real setup commands]
```

### 6.3 Running Each App Locally

<!-- AGENT:
For EACH app, extract the dev command from:
1. The app's package.json → scripts.dev or scripts.start
2. The monorepo tool command (nx serve [app], turbo run dev --filter=[app-name])
3. README

Show: the exact command, the local URL, and any required env vars that must be set first.
-->

| App | Command | Local URL | Requires |
|---|---|---|---|

### 6.4 Daily Developer Commands

<!-- AGENT:
Produce the commands a developer uses every single day.
Get these from: root package.json scripts, monorepo tool conventions, CI config (to find what QA / CI runs).
All commands must be real — taken from actual scripts or tool conventions, not invented.
-->

| Task | Command | Notes |
|---|---|---|
| Start dev server (single app) | | |
| Start all apps | | |
| Run unit tests (affected only) | | |
| Run unit tests (all) | | |
| Run E2E tests | | |
| Build for production | | |
| Lint all packages | | |
| Format code | | |
| Type-check | | |
| View dependency graph | | |
| Clear cache | | |
| Create new component | | |
| Create new library | | |
| Create new app | | |

---

## 7. 🏠 Local Development Environment

### 7.1 Port Assignments

<!-- AGENT:
For each app, find the local development port from:
1. The dev script in package.json (e.g. vite --port 3001, next dev -p 3001)
2. vite.config.ts/js → server.port
3. next.config.* → (port in dev script)
4. .env.example → PORT variable
5. README

Also check for any other locally-running services: Storybook port, mock server port, proxy port.
-->

| Service | Port | URL | Notes |
|---|---|---|---|

### 7.2 Environment Variables

<!-- AGENT:
Read .env.example / .env.template at root and per-app.
For EVERY variable found:
- Name: exact variable name
- Scope: which app(s) need it (root-level = all, or specific app)
- Required: if it has a non-empty placeholder value, treat as required
- Description: infer from name, surrounding comments, or usage in code
- Set In: CI secrets / .env.local / hardcoded default

Group by app or by scope.
NEVER document actual secret values. Document only variable names and descriptions.
-->

**Root / Shared:**

| Variable | Required | Description | Set In |
|---|---|---|---|

**[App Name]:**

| Variable | Required | Description | Set In |
|---|---|---|---|

<!-- AGENT: Repeat per-app table for each app that has its own .env.example -->

### 7.3 Storybook

<!-- AGENT:
Check for .storybook/ directory.
If present:
1. Read .storybook/main.ts or main.js → list addons configured
2. Find the Storybook start command (root package.json scripts or per-package)
3. Find the Storybook version from devDependencies
4. Note whether Chromatic or Percy is configured for visual regression (check CI config and devDependencies)
5. Find the Storybook URL (local port and any deployed URL)

If not present: "> Not configured."
-->

**Version:**
**Start Command:**
**Local URL:**
**Deployed URL:**
**Addons:**
**Visual Regression:**

### 7.4 API Mocking

<!-- AGENT:
Check for Mock Service Worker (msw package):
1. If present: find the handlers file (src/mocks/handlers.ts or similar)
2. List the mocked endpoints found in handlers (method + path + brief description)
3. Find how MSW is started (in browser: setupWorker; in tests: setupServer)
4. Note any fixture/seed data files

Check for other mocking approaches:
- json-server or similar mock API servers (check package.json scripts for "mock" or "api:mock")
- WireMock configs
- Custom Express mock servers

If no mocking: "> No API mocking layer configured."
-->

**Tool:**
**Handlers File:**
**Mocked Endpoints:**

| Method | Path | Description |
|---|---|---|

### 7.5 Docker & Local Infrastructure

<!-- AGENT:
Check for Dockerfile, docker-compose.yml, docker-compose.dev.yml at root or per-app.
If present:
1. List the services defined in docker-compose
2. Note which services are required for local development
3. Provide the command to start Docker services
4. Note any database seeding or initialisation steps

If not present: "> Docker not used for local development."
-->

---

## 8. 📁 Project Conventions

### 8.1 Folder Structure Convention

<!-- AGENT:
Examine 2–3 apps and 1–2 shared packages to identify the internal folder structure pattern.
Document the standard folder layout inside a typical app and a typical package.
Common patterns:
- Feature-based: src/features/[feature]/{components, hooks, utils, types, api}
- Layer-based: src/components/, src/hooks/, src/pages/, src/utils/, src/types/
- Domain-based: src/[domain]/ containing everything for that domain

Show the real structure of one app as an example tree.
-->

**App Structure:**
```
src/
├── [real folder structure]
```

**Package Structure:**
```
src/
├── [real folder structure]
```

### 8.2 File Naming Conventions

<!-- AGENT:
Observe naming patterns from actual files in the repo:
- Components: PascalCase.tsx? kebab-case.tsx? Component.module.css?
- Hooks: useHookName.ts? use-hook-name.ts?
- Test files: Component.test.tsx? Component.spec.tsx? __tests__/Component.tsx?
- Story files: Component.stories.tsx? Component.stories.ts?
- Type files: types.ts? Component.types.ts? interfaces.ts?
- Utility files: format.ts? formatDate.ts? utils.ts?
- Index files: barrel exports? direct imports only?

Show real file path examples for each convention.
-->

| File Type | Convention | Example |
|---|---|---|
| Component | | |
| Hook | | |
| Test | | |
| Story | | |
| Types/Interface | | |
| Utility | | |
| Constants | | |
| Index/Barrel | | |

### 8.3 Import Conventions

<!-- AGENT:
Check:
1. tsconfig.json paths aliases → what can be imported as @components/... or @/... etc.
2. ESLint config → eslint-plugin-import rules (e.g. no-relative-parent-imports, order rules)
3. Observe actual import patterns in source files
4. Any import sorting tool (eslint-plugin-simple-import-sort, prettier-plugin-sort-imports)

Document the expected import order and which aliases to use for internal imports.
-->

```tsx
// [Real import ordering example based on codebase conventions]
import [external packages]
import [internal monorepo packages via @scope/...]
import [app-relative imports via alias e.g. @components/...]
import [relative imports]
```

### 8.4 Component Conventions

<!-- AGENT:
From examining existing components in the codebase, document:
1. Are components always default exports or named exports?
2. Do components have a co-located types file or are props typed inline?
3. Is there a standard component file template? (check generators or CONTRIBUTING)
4. Are props interfaces named ComponentNameProps?
5. Is there a forwardRef pattern used?
6. Is there a compound component pattern used for complex components?

Show a minimal real component as an example if a simple one exists.
-->

### 8.5 Commit Convention

<!-- AGENT:
From commitlint.config.*, .czrc, CONTRIBUTING.md, or git log:
1. What convention is used? (Conventional Commits, Angular, custom, none enforced)
2. What commit types are allowed?
3. What scope values are valid (if enforced)?
4. Is the commit message length restricted?

Show 3 real example commits based on actual package/scope names in this repo.
-->

**Convention:**

```
[type]([scope]): [description]

Allowed types: [list from commitlint config]
Allowed scopes: [list if defined, or "any"]

Real examples:
  [example 1 using real app/package name]
  [example 2]
  [example 3]
```

---

## 9. 🧩 Shared Code Reference

### 9.1 UI Component Library

<!-- AGENT:
For the shared UI component package (from discovery step 22):
1. Find the package name and version
2. Read the index file / barrel exports to get a full list of exported components
3. For each component, note: name, brief description of what it does, key props (look at the TypeScript interface or JSDoc)
4. Note the import path pattern

Group components by category if the exports are already grouped, otherwise by type.
This is critical — developers ask "does we have a [component]?" constantly.
-->

**Package:** `@[scope]/[package-name]`
**Import Pattern:** `import { ComponentName } from '@[scope]/[package-name]'`

| Component | Description | Key Props |
|---|---|---|

### 9.2 Shared Hooks

<!-- AGENT:
For any hooks package or hooks directory in shared packages (discovery step 23):
1. List every exported hook
2. For each: name, what it does, parameters, return value
3. Note the import path

Also search app source files for hooks that are defined locally but could be relevant (e.g. in a hooks/ shared directory).
Developers ask "do we have a hook for X?" — be thorough.
-->

**Package:** `@[scope]/[hooks-package]` (or note if hooks are part of another package)

| Hook | Description | Parameters | Returns |
|---|---|---|---|

### 9.3 Shared Utilities

<!-- AGENT:
For any utils/helpers package or shared utilities (discovery step 23):
1. List every exported utility function
2. For each: name, what it does, key parameters
3. Note the import path

Group by domain if they are already grouped (e.g. date utilities, string utilities, API utilities).
-->

| Utility | Description | Usage |
|---|---|---|

### 9.4 Shared Types & Interfaces

<!-- AGENT:
For any types/models package (discovery step 24):
1. List all exported TypeScript types, interfaces, and enums
2. For each: name, what it represents, key fields (for interfaces)
3. Note the import path

These are critical — developers and QA ask about data structures constantly.
Group by domain (e.g. User types, Product types, API response types).
-->

**Package:** `@[scope]/[types-package]` (or note where types live)

| Type / Interface | Description | Key Fields |
|---|---|---|

### 9.5 Shared Constants & Enums

<!-- AGENT:
Search for shared constants files (constants.ts, enums.ts, config.ts in shared packages).
List all exported constants and enums that would be useful to developers:
- Route constants
- API endpoint constants
- Enum values (user roles, status codes, event names)
- Configuration constants
-->

| Constant / Enum | Value(s) | Description |
|---|---|---|

### 9.6 Design Tokens & Theming

<!-- AGENT:
From discovery step 25 and section 10.3 investigation:
1. Identify the theming system (Tailwind config, CSS variables, styled-components theme, vanilla-extract)
2. List the real token categories and values

For Tailwind: read tailwind.config.js → theme.extend → list custom colors, spacing, typography, breakpoints
For CSS variables: search for :root { --variable patterns → list all custom properties
For theme objects: find the theme.ts or tokens.ts file → list all token values

This lets developers ask "what's the primary colour?" or "what are the breakpoints?"
-->

**System:**

| Category | Tokens / Values |
|---|---|
| Colors | |
| Typography | |
| Spacing | |
| Breakpoints | |
| Shadows | |
| Border Radius | |

---

## 10. 🔐 Authentication & Authorisation

### 10.1 Auth Approach

<!-- AGENT:
From discovery step 31 and auth-related source files:
- Auth provider: @auth0/auth0-react → Auth0 | amazon-cognito-identity-js / @aws-amplify/auth → Cognito | @okta/okta-auth-js → Okta | next-auth → NextAuth.js | @azure/msal-react → Azure AD / MSAL | custom implementation
- Auth method: OAuth2+PKCE, JWT, session cookie, SAML, OpenID Connect
- Token storage: read auth context or useAuth files for localStorage, sessionStorage, memory, httpOnly cookie
- Shared auth package: is there a @scope/auth package? What does it export?
-->

| Property | Value |
|---|---|
| **Auth Provider** | |
| **Auth Method** | |
| **Token Storage** | |
| **Shared Auth Package** | |

### 10.2 Login Flow (Plain Language)

<!-- AGENT:
Describe what happens when a user logs in, in 4–6 plain-English steps.
Base on: auth provider documentation + actual implementation in auth context/callback files.
Focus on user experience, not implementation detail.
-->

### 10.3 Roles & Permissions

<!-- AGENT:
Search for role/permission definitions:
1. Enum or const with role names (USER, ADMIN, EDITOR, VIEWER, etc.)
2. Route guards that check roles (<RequireRole>, <ProtectedRoute roles={[...]}>)
3. Permission utility functions (hasPermission, canAccess, etc.)
4. JWT claims that include roles
5. RBAC config files

List real roles and what they can/cannot do.
If no RBAC: "> All authenticated users have the same permissions level."
-->

| Role | Description | What They Can Do |
|---|---|---|

### 10.4 Session Management

<!-- AGENT:
Document:
1. How long sessions last (token expiry — look for expiresIn in auth config or JWT inspection)
2. How token refresh works (silent refresh, refresh token rotation, re-login prompt)
3. What happens on logout (token clearing, redirect)
4. Is there SSO across apps in this monorepo?
-->

---

## 11. 🌐 API Integration

### 11.1 API Endpoints & Services

<!-- AGENT:
From discovery steps 31, 32:
- List every external API or backend service this project calls
- For each: name, type (REST/GraphQL/tRPC), base URL (from .env.example variable name), auth requirement, which apps use it
-->

| API / Service | Type | Base URL Variable | Auth | Used By |
|---|---|---|---|---|

### 11.2 API Client Configuration

<!-- AGENT:
Find the API client setup (lib/api.ts, utils/api.ts, apollo.ts, trpc.ts, or in a shared package).
Show the real configuration code (anonymised — replace any hardcoded values with env var references).
Explain in plain language what it configures (base URL, auth headers, interceptors, error handling, retry logic).
-->

```tsx
// [Real API client setup]
```

### 11.3 Data Fetching Patterns

<!-- AGENT:
Based on the data fetching library in use, document the standard patterns used in this codebase:
- TanStack Query: standard query key conventions, how queries/mutations are structured, where query definitions live
- Apollo: query structure, where GQL files live, code generation setup if any
- tRPC: router structure, how procedures are called from the client
- SWR: standard usage pattern
- Custom fetch: wrapper utility pattern

Show a real example query/mutation from the codebase.
-->

### 11.4 API Error Handling

<!-- AGENT:
Search for error handling patterns:
1. Global error boundary (ErrorBoundary.tsx)
2. Query error handling (onError callbacks, error state in UI)
3. HTTP interceptors for 401/403/500 responses
4. Toast/notification on API errors
5. Retry logic configuration

Describe each layer of error handling in plain language.
-->

---

## 12. 🗃️ State Management

### 12.1 State Architecture

<!-- AGENT:
Based on the detected state management library, document how state is structured in this codebase.

For Redux Toolkit:
1. Find the store definition (store.ts or store/index.ts)
2. List all slices/reducers registered in the store
3. List any middleware configured (thunk, saga, RTK Query)
4. Document the selector convention

For Zustand:
1. Find all store files (*.store.ts or stores/*.ts)
2. For each store: name, what state it holds, what actions it exposes

For React Context:
1. List all Context providers
2. For each: what state it provides, where it's positioned in the tree

For Jotai/Recoil:
1. List atom definitions
2. Group by domain

Show the store/state structure as a list or tree, not prose.
-->

### 12.2 State Scope Guidelines

<!-- AGENT:
From CONTRIBUTING docs, code comments, or observation of how state is used:
1. What goes in global state vs local component state?
2. What goes in URL state (search params)?
3. What goes in server state (react-query cache) vs client state?
4. Are there documented rules about state placement?

If not documented: observe patterns in the code and describe what you see.
-->

---

## 13. 🧪 Testing

### 13.1 Testing Strategy Overview

<!-- AGENT:
From test config files and CI, document the full testing picture.
For each test type: tool, file location pattern, when it runs, how many tests approximately exist (count test files).
-->

| Test Type | Tool | File Pattern | Runs When | Approx. Count |
|---|---|---|---|---|
| Unit / Integration | | | | |
| Component | | | | |
| Visual Regression | | | | |
| E2E | | | | |
| Accessibility | | | | |

### 13.2 Running Tests

<!-- AGENT:
Real commands only, derived from package.json scripts and monorepo tool conventions.
All command variants: single package, all affected, all packages, E2E, coverage, watch mode.
-->

```bash
# Unit tests — single package
[command]

# Unit tests — all affected packages (fastest for PRs)
[command]

# Unit tests — all packages
[command]

# Watch mode
[command]

# With coverage
[command]

# E2E — all
[command]

# E2E — single app
[command]

# E2E — headed (visible browser, useful for debugging)
[command]
```

### 13.3 Coverage Requirements

<!-- AGENT:
From jest/vitest config (coverageThreshold / thresholds) and CI coverage gate steps.
If enforced, show the exact configured thresholds.
-->

| Scope | Statements | Branches | Functions | Lines |
|---|---|---|---|---|

### 13.4 Testing Conventions

<!-- AGENT:
From code observation and docs:
1. Co-located tests or __tests__ folder?
2. What do test file names look like?
3. Are tests written with describe/it blocks? What's the naming convention?
4. E2E selector strategy (data-testid? aria roles? CSS classes?) — look at actual spec files
5. Is MSW used for mocking API calls in tests?
6. Are there shared test utilities (render wrappers, custom matchers)?
7. Are snapshots used? (search for toMatchSnapshot)
8. Is there a test data/fixture strategy?
-->

### 13.5 E2E Test Coverage

<!-- AGENT:
For each E2E spec file found (playwright/tests/, cypress/e2e/):
1. List the file name and the user flows it tests
2. Note which app it tests

This helps QA understand what is and isn't covered.
-->

| Spec File | App | User Flows Tested |
|---|---|---|

---

## 14. 🏗️ Build & CI/CD

### 14.1 Build Pipeline

<!-- AGENT:
From CI config files, reconstruct the full pipeline.
Show as an annotated flow:
- What triggers each stage
- What jobs run (and which run in parallel)
- Dependencies between jobs
- Where each stage deploys to (if applicable)

Present as a clear text diagram.
-->

```
[Reconstructed pipeline from CI config]
```

### 14.2 CI Workflows

<!-- AGENT:
From CI config, list every workflow/job with its real name, trigger, and what it does.
Use the actual job/workflow names from the config files.
-->

| Workflow / Job | Trigger | Steps | Duration (if known) |
|---|---|---|---|

### 14.3 Build Outputs

<!-- AGENT:
For each app, document what the production build produces:
1. The build command
2. The output directory (dist/, .next/, build/, out/)
3. What type of output (SPA bundle, SSR server, static files, Docker image)
4. Approximate bundle size if determinable (from bundlesize config or CI logs)
-->

| App | Build Command | Output Dir | Output Type |
|---|---|---|---|

### 14.4 Caching

<!-- AGENT:
Document all caching layers:
1. Monorepo task cache (Nx / Turbo / none) — what is cached, what invalidates it
2. CI dependency cache (actions/cache) — the cache key strategy
3. Remote cache (Nx Cloud, Vercel Remote Cache) — configured or not
4. CDN/browser cache (if documented)
-->

| Cache Layer | Tool | What Is Cached | Invalidated By |
|---|---|---|---|

---

## 15. 🚢 Deployment

### 15.1 Environments

<!-- AGENT:
From CI config, .env.example, README, and deployment config files (vercel.json, netlify.toml, serverless.yml):
List each environment with its URL (or URL variable name), source branch, deploy trigger, and who can approve.
-->

| Environment | URL | Deployed From | Trigger | Approval |
|---|---|---|---|---|
| Development | | | | |
| Staging | | | | |
| Production | | | | |

### 15.2 Deployment Platform & Steps

<!-- AGENT:
From CI config and any deploy scripts:
1. What platform is used (Vercel, Netlify, AWS S3+CloudFront, Docker+K8s, Azure Static Web Apps, etc.)?
2. What is the exact sequence of steps from code merge to production?
3. Are there manual approval gates?
4. Is there a rollback procedure documented?
-->

**Platform:**

```bash
# [Real production build and deploy commands]
```

### 15.3 Feature Flags

<!-- AGENT:
From discovery step 31:
- launchdarkly-react-client-sdk → LaunchDarkly
- @unleash/proxy-client-react → Unleash
- @configcat/react → ConfigCat
- @growthbook/growthbook-react → GrowthBook
- Custom: featureFlags.ts, flags.ts, useFlag.ts

If found:
1. Package name and version
2. How flags are defined (SDK dashboard, local config file, env var)
3. How flags are used in code (real usage example)
4. List any flags that are currently defined in config or constants files

If not found: "> No feature flag system detected."
-->

| Flag Name | Default | Description |
|---|---|---|

---

## 16. 📦 Package Management

### 16.1 Adding Dependencies

<!-- AGENT:
Based on detected package manager and monorepo tool, generate the real commands.
Use an actual package name from the repo as the target example.
Show: runtime dep, dev dep, root-level dep, and how to add to multiple packages.
-->

```bash
# Add runtime dependency to a specific package
[command]

# Add dev dependency to a specific package
[command]

# Add shared dev dependency at root level
[command]

# Remove a dependency
[command]

# Update a dependency
[command]
```

### 16.2 Versioning & Publishing

<!-- AGENT:
From lerna.json / .changeset/ / .releaserc / semantic-release config:
1. Versioning tool: Lerna / Changesets / semantic-release / manual
2. Versioning mode: fixed (all packages share a version) or independent (each package has its own)
3. Changelog: auto-generated from commits? Which format?
4. Publish target: npm public / private registry / internal registry

Show the real release workflow commands.
-->

**Tool:**
**Mode:** [Fixed / Independent]
**Registry:**

```bash
# [Real version bump and publish commands]
```

### 16.3 Internal Package Imports

<!-- AGENT:
Using a real shared package name from the repo, show:
1. How to declare the dependency in package.json (workspace:* or *)
2. How to import from it in source code
3. Whether it needs to be built before it can be imported (check if it uses TypeScript project references or pre-built dist)
-->

```json
// package.json — declaring dependency on an internal package
{
  "dependencies": {
    "@[real-scope]/[real-package]": "workspace:*"
  }
}
```

```tsx
// Importing from an internal package
import { [RealExport] } from '@[real-scope]/[real-package]';
```

---

## 17. ♿ Accessibility & Performance

### 17.1 Accessibility Standards

<!-- AGENT:
From axe-core config, Storybook a11y addon, README, CONTRIBUTING:
- Target WCAG level (2.1 AA, 2.2 AA, etc.)
- Automated tools and how to run them
- ESLint jsx-a11y rules (check .eslintrc for the rule set used)
- Browser support for assistive technologies (if documented)
-->

**Target Standard:**

| Tool | What It Checks | How to Run |
|---|---|---|

### 17.2 Performance Budgets

<!-- AGENT:
From lighthouse CI config, webpack performance config, vite chunkSizeWarningLimit, bundlesize:
List the real configured thresholds.
If none: "> No formal performance budgets configured."
-->

| Metric | Budget | Tool |
|---|---|---|

### 17.3 Bundle Analysis

<!-- AGENT:
Real commands to analyse bundle size for each app.
From webpack-bundle-analyzer, vite-bundle-visualizer, @next/bundle-analyzer, rollup-plugin-visualizer.
-->

```bash
# [Real bundle analysis commands per app]
```

---

## 18. 🔌 Third-Party Integrations & Services

<!-- AGENT:
From discovery step 31 (dependency scan) and step 32 (env var scan):
List EVERY third-party service this project integrates with.
This is important for analysts asking "what services do we depend on?" and for developers setting up local environments.

Common patterns to detect:
- Analytics: @segment/analytics-next, mixpanel-browser, react-ga4, @amplitude/analytics-browser
- Monitoring/Errors: @sentry/react, @datadog/browser-rum, logrocket, @newrelic/browser-agent
- Auth: @auth0/auth0-react, @okta/okta-react, @azure/msal-react, amazon-cognito-identity-js, next-auth
- Payments: @stripe/stripe-js, @stripe/react-stripe-js, braintree-web
- Feature flags: launchdarkly-react-client-sdk, @unleash/proxy-client-react
- CMS: contentful, @sanity/client, @prismicio/client
- Support/Chat: @intercom/messenger-js-sdk, zendesk
- Maps: @googlemaps/js-api-loader, mapbox-gl, leaflet
- Email: @sendgrid/mail, @mailchimp/mailchimp_marketing
- Search: algoliasearch, @elastic/elasticsearch
- CDN/Media: cloudinary, imgix, @cloudflare/workers-sdk
- Testing/Quality: chromatic, percy, datadog

For each integration: package name, purpose, which apps use it, required env variables.
-->

| Service | Purpose | Package | Used By Apps | Required Env Vars |
|---|---|---|---|---|

---

## 19. 🐛 Debugging & Troubleshooting

### 19.1 Common Issues & Fixes

<!-- AGENT:
From TROUBLESHOOTING.md, FAQ.md, README troubleshooting section, pinned GitHub Issues, and inferred from tools:

Always include tool-specific issues:
- Nx: `Cannot find module` after pulling → rebuild internal packages; stale cache → nx reset; project graph errors → check project.json
- Lerna: bootstrap errors, symlink issues, publishing errors
- Turborepo: cache hit on broken build → clear .turbo; pipeline ordering issues
- pnpm: phantom dependencies, shamefully-hoist issues
- TypeScript monorepo: path aliases not resolving, project references not building
- Next.js in monorepo: transpilePackages missing for local packages
- Vite in monorepo: pre-bundling issues with local packages, HMR not working

Also add any project-specific issues found in docs.
-->

| Symptom | Root Cause | Fix Command / Steps |
|---|---|---|

### 19.2 Debug Commands

<!-- AGENT:
Real commands for debugging this specific project, based on the detected tools.
-->

```bash
# View project dependency graph
[command]

# Check which projects are affected by current changes
[command]

# Run a build with verbose output
[command]

# Clear all caches (monorepo tool + node_modules/.cache)
[command]

# Check why a package is installed
[command]

# Inspect the final resolved config for a package
[command]

# Run a single test file directly
[command]

# Debug E2E tests with browser open
[command]
```

### 19.3 Logging & Monitoring Access

<!-- AGENT:
From detected monitoring packages (Sentry, Datadog, LogRocket, etc.) and .env.example:
List each tool, its purpose, and how to access it.
URLs can only be included if found in docs or .env.example.
Mark access as "[Requires manual input]" if not documented in code.
-->

| Tool | Purpose | Access |
|---|---|---|

---

## 20. 👥 Team & Ownership

### 20.1 Package & App Ownership

<!-- AGENT:
From CODEOWNERS, package.json author/maintainers fields, per-package README, or any ownership docs:
List the owning team for each app and package.
If a CODEOWNERS file exists, extract ownership for each major path.
For Slack channels and on-call: mark as "[Requires manual input]" — cannot be inferred from code.
-->

| App / Package | Owning Team | Contact |
|---|---|---|

### 20.2 CODEOWNERS

<!-- AGENT:
If .github/CODEOWNERS (or root CODEOWNERS) exists, paste its full contents here.
This is a reference developers check when creating PRs.
If it doesn't exist: "> No CODEOWNERS file configured."
-->

```
[CODEOWNERS contents]
```

---

## 21. 🔄 Development Workflow

### 21.1 Branching Strategy

<!-- AGENT:
From CI config triggers, CONTRIBUTING, and README:
-->

| Branch Pattern | Purpose | Auto Deploy To |
|---|---|---|

### 21.2 Pull Request Process

<!-- AGENT:
From .github/PULL_REQUEST_TEMPLATE.md, CONTRIBUTING, and CI config (what must pass before merge):
List as a numbered checklist.
-->

### 21.3 Release Process

<!-- AGENT:
How does code go from merged PR to a production release?
From CI config release jobs, versioning tool config, and README/CONTRIBUTING release section.
Write as a step-by-step process.
-->

---

## 22. 🌍 Internationalisation (i18n)

<!-- AGENT:
Check for i18n libraries:
- react-i18next / i18next
- react-intl / @formatjs/intl
- next-i18next
- lingui / @lingui/react
- Custom solution

If found:
1. Package name and version
2. Where translation files live (public/locales/, src/i18n/, etc.)
3. Which languages are supported (list locale codes found in translation file directories)
4. How to add a new translation key
5. How to add a new language
6. Whether translations are managed externally (Crowdin, Lokalise, Phrase)

If not found: "> Internationalisation is not implemented in this project."
-->

**Library:**
**Supported Locales:**
**Translation Files:**

| Locale | File Path | Completeness |
|---|---|---|

---

## 23. 🌐 Browser & Device Support

<!-- AGENT:
Check for:
1. .browserslistrc or browserslist in package.json → list the target browsers
2. Babel config → @babel/preset-env targets
3. Vite build.target or esbuild target
4. README → supported browsers section
5. CSS autoprefixer config

Also note:
- Mobile/responsive: is there evidence of responsive design (tailwind sm/md/lg, CSS media queries, react-responsive)?
- Minimum screen width if documented

If nothing is documented: "> Browser support targets are not formally documented."
-->

**Target Browsers:**
```
[browserslist config or equivalent]
```

| Browser | Minimum Version | Notes |
|---|---|---|

---

## 24. ⚠️ Known Limitations & Tech Debt

<!-- AGENT:
Search for:
1. TODO / FIXME / HACK / TECH-DEBT comments in source files — collect the most significant ones
2. Any tech-debt.md, KNOWN_ISSUES.md, or similar file
3. GitHub Issues labelled "tech-debt", "known-issue", or "limitation" (if accessible)
4. CHANGELOG entries marked as "known issues"
5. README warnings or caveats section

List only significant items that affect developers' work, not trivial TODOs.
This helps the AI assistant answer "is there a known issue with X?" accurately.
-->

| Area | Issue | Impact | Tracking |
|---|---|---|---|

---

## 25. 📚 Glossary

<!-- AGENT:
Two types of entries:

1. STANDARD TERMS — always include, defined in context of this specific project:
   - Monorepo: what it means for THIS project
   - [Monorepo tool name]: what it does in this project
   - Affected: if the tool supports it, explain how it works here
   - Package / Library: what these mean in this codebase
   - Workspace: the workspace concept for the detected package manager
   - Task Graph / Pipeline: if Nx or Turborepo is used
   - Design Token: if tokens are present
   - Storybook: if present, what it contains for this project

2. PROJECT-SPECIFIC TERMS — extract from:
   - App and package names (what does "ledger", "portal", "hub", "atlas" mean in this business context?)
   - Domain language in component names and route paths
   - Business terms in README or CONTRIBUTING
   - Enum values that represent business concepts (order statuses, user roles, etc.)

Add 5–10 project-specific terms. Write definitions for a non-technical reader.
-->

| Term | Definition |
|---|---|
| **Monorepo** | |
| **[Monorepo tool]** | |
| **Package** | |
| **Workspace** | |
| **[Detected term]** | |

---

## 26. 🔗 Resources & Links

<!-- AGENT:
From README, package.json (homepage, repository, bugs), docs/, .env.example URL variables, CI deploy URLs:
List all useful links. Mark any that cannot be found in code as "[Requires manual input]".
Remove rows for resources that clearly don't exist for this project.
-->

| Resource | URL | Notes |
|---|---|---|
| Repository | | |
| Storybook (deployed) | | |
| Figma / Design Files | | |
| API Documentation | | |
| Issue Tracker | | |
| Extended Docs / Confluence | | |
| Monitoring / Datadog | | |
| Error Tracking / Sentry | | |
| CI/CD Dashboard | | |
| npm / Package Registry | | |

---

## 27. 📝 Knowledge Base Change Log

<!-- AGENT:
First entry: today's date, "AI Agent (automated generation)", brief summary of what was generated and any sections that required manual input.
-->

| Date | Author | Change |
|---|---|---|
| | AI Agent (automated generation) | Initial KB generated. Sections requiring manual input: [list them] |

---

*This knowledge base is a living document. If you find information that is incorrect or outdated, update it or raise it with the owning team.*

*Template: monorepo-kb-template.md — AI Agent Edition v3.0.0*
