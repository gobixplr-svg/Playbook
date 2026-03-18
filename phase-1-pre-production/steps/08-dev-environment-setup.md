# Step 8: Dev Environment Setup

## Purpose

Set up the development environment so Phase 2 starts with momentum, not configuration. By the end of this step, the project compiles, a test suite runs, external services respond, and an AI workspace loads your project context — so the first Sprint 1 task is building a feature, not debugging build settings.

## Why This Matters

Setting up infrastructure last means your first Sprint 1 story carries hidden setup tax — you'll spend days on build config, dependency resolution, CI debugging, and service authentication instead of building features. Every hour of setup friction in Sprint 1 is an hour stolen from the walking skeleton.

This step formalizes three things:
1. **Build environment** — the project compiles with the correct structure and dependencies
2. **Testing pipeline** — at least one test passes end-to-end (target → framework → assertion → green)
3. **AI workspace** — a new AI session can understand the project without manual briefing

## Platform Legend

This step covers three project types. Read the shared guidance, then follow the subsection for your platform:

| Platform | Icon | Examples |
| --- | --- | --- |
| iOS / macOS | **[iOS]** | SwiftUI apps, UIKit apps |
| Web | **[Web]** | React, Next.js, Astro, static sites |
| Game Engine | **[Engine]** | Unity, Godot, Unreal |

Sections 8.6 (AI Workspace) and 8.8 (Validation) are universal. All other sections have platform-specific subsections — skip the ones that don't apply.

## Prerequisites

- Phase 0 Step 6 (App Naming) completed — finalized app name for project name and identifiers
- Step 1 (Requirements) completed — test plan scope and acceptance criteria count
- Step 3 (Architecture) completed — module structure, patterns, project layout
- Step 5 (Design System) completed — design tokens for asset setup
- Step 6 (API & Data) completed — data schema, external service list (if applicable)
- Step 7 (Project Plan) completed — sprint plan, current sprint stories

## Process

### 8.1 Project Setup

**Principle: Create the project shell manually, not via AI.** AI-generated project scaffolds frequently produce misconfigured build settings, wrong project types, or framework-specific issues that are hard to debug. The correct workflow:

1. **You create the project** using the platform's standard tooling
2. **Verify the project compiles** with default settings
3. **AI builds out the folder structure and code files** inside the working project

#### [iOS] Xcode Project

**Important:** AI-generated `.xcodeproj` files frequently produce "grey folder" issues — folder references instead of groups — which prevents the AI from managing files correctly in later sprints.

1. **Create in Xcode** with the correct template, bundle ID, deployment target, and test targets
2. **Verify file-system-synchronized groups** — Xcode 15+ uses this by default. Right-click the top-level app folder: if you see "Convert to Group" it's already synced (correct)
3. **AI builds out the folder structure** — with synced groups, the AI can create directories and Swift files via the filesystem and they'll compile automatically

**Xcode project creation checklist:**

| Setting | Value |
| --- | --- |
| Template | App (iOS) |
| Product Name | [Finalized app name from Phase 0 Step 6] |
| Team | Your Apple Developer account |
| Organization Identifier | [com.yourapp] |
| Interface | SwiftUI |
| Language | Swift |
| Storage | None (unless using SwiftData/Core Data) |
| Include Tests | Yes (both Unit and UI) |

**After project creation, configure:**
- Deployment target from architecture doc
- Swift language version from architecture doc
- Strict concurrency checking (if using Swift 6)
- Code signing

#### [Web] Node.js Project

Use the framework's official scaffold command:

| Framework | Scaffold Command |
| --- | --- |
| React Router v7 | `npx create-react-router@latest` |
| Next.js | `npx create-next-app@latest` |
| Astro | `npm create astro@latest` |
| Vite (plain React) | `npm create vite@latest` |

**After scaffold, configure:**
- Rename `package.json` name to match your project
- TypeScript strict mode (`"strict": true` in tsconfig)
- Path aliases (`~/` → `./app/` or `./src/`)
- SSG/pre-rendering if the architecture calls for static output
- Framework-specific routing setup (file-based or config-based per architecture doc)

**Web project creation checklist:**

| Setting | Value |
| --- | --- |
| Package name | [project-slug from Phase 0 Step 6] |
| Framework | [From architecture doc] |
| Language | TypeScript |
| Styling | [Tailwind CSS / CSS Modules / etc. from architecture doc] |
| SSR/SSG | [Per architecture doc — static, SSR, or hybrid] |

#### [Engine] Game Engine Project

**Create the project manually in the engine's project manager** (Unity Hub, Godot Project Manager, Unreal Launcher). AI-generated engine project files miss renderer assignments, quality profiles, and platform-specific settings.

| Setting | Unity | Godot | Unreal |
| --- | --- | --- | --- |
| Create via | Unity Hub | Godot Project Manager | Unreal Launcher |
| Template | 3D/2D Core (URP/HDRP) | Default | Third Person / Blank |
| Renderer | Set in Project Settings | Set in Project Settings | Set at creation |
| Platform target | iOS / Android / PC | Export presets | Target platform |

**After project creation, configure:**
- Renderer settings (quality profiles, resolution scaling)
- Input system (new Input System for Unity, Input Map for Godot)
- Build settings for target platform
- Editor preferences (script editor, formatting)

#### All Platforms — AI Creates After Setup

Once the project shell exists and compiles:
- Folder hierarchy from the architecture document — every folder in the spec should exist, even if empty
- `.gitignore` for the platform ([gitignore.io](https://gitignore.io) is a good starting point)
- Initial commit of the empty project structure

**Folder structure verification:** Walk through the architecture document's module list and confirm each folder exists. Missing folders mean missing homes for Sprint 1 code — which leads to "where does this go?" decisions that slow development.

### 8.2 Dependency Management

Add all third-party dependencies identified across Steps 2–6.

**Universal rules:**
- **Pin versions** — "latest" today is "broken build in 3 months" when a breaking change ships
- **Commit the lockfile** — `package-lock.json`, `Packages.resolved`, `*.lock` files ensure reproducible builds
- **Document the source** — record which spec identified each dependency

**Dependency inventory format:**

| Dependency | Manager | Version | Purpose | Source Spec |
| --- | --- | --- | --- | --- |
| [name] | [SPM / npm / UPM] | [pinned version] | [what it does] | [Step 3 / Step 6] |

#### [iOS] Swift Package Manager

SPM is built into Xcode and requires no extra tooling. Add packages via File → Add Package Dependencies. Use exact version or "Up to Next Minor" for stability.

#### [Web] npm / pnpm

```bash
npm install [dependencies]
npm install -D [devDependencies]
```

Verify `package-lock.json` is committed. For monorepos or strict reproducibility, consider `pnpm` with its strict peer dependency resolution.

**Watch for:** Dependencies that ship client-side code vs. build-only. MDX plugins, syntax highlighters, and remark/rehype plugins run at build time only — verify they don't bloat the client bundle.

#### [Engine] Engine Package Managers

- **Unity:** Unity Package Manager (UPM) for official packages, OpenUPM for community packages. Git-based packages via URL.
- **Godot:** AssetLib for official/community addons, git submodules for custom libraries.
- **Unreal:** Marketplace plugins, git submodules, or manual source inclusion.

**After adding all dependencies:** Build the project. A clean build with all dependencies resolved is the baseline.

### 8.3 Testing Infrastructure

Set up the testing pipeline so TDD begins immediately in Sprint 1.

#### [iOS] Swift Testing + XCUITest

**Create test targets:**
- Unit test target (Swift Testing framework preferred for new code)
- UI test target (XCUITest — Swift Testing doesn't support UI tests yet)

**Create mock protocols:**
For every service protocol defined in Step 3, create a corresponding mock class:

```swift
class MockRouteService: RouteServiceProtocol {
    var routesToReturn: [Route] = []
    var fetchRoutesCalled = false
    func fetchRoutes() async throws -> [Route] {
        fetchRoutesCalled = true
        return routesToReturn
    }
}
```

#### [Web] Vitest + Testing Library + Playwright

**Install test dependencies:**

```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom jsdom
```

**Create test config** (`vitest.config.ts`):

```typescript
import { defineConfig } from "vitest/config";
import tsconfigPaths from "vite-tsconfig-paths";

export default defineConfig({
  plugins: [tsconfigPaths()],
  test: {
    environment: "jsdom",
    include: ["app/**/*.test.{ts,tsx}", "src/**/*.test.{ts,tsx}"],
    setupFiles: ["./app/test/setup.ts"],
  },
});
```

**Create test setup** (`app/test/setup.ts`):

```typescript
import "@testing-library/jest-dom/vitest";
```

**Add test scripts to package.json:**

```json
"test": "vitest run",
"test:watch": "vitest"
```

For E2E testing (Sprint 2+), plan for Playwright but don't set it up now — unit tests are the Sprint 1 priority.

#### [Engine] Engine Test Frameworks

| Engine | Test Framework | Test Types |
| --- | --- | --- |
| Unity | Unity Test Framework (NUnit) | Edit Mode + Play Mode |
| Godot | GdUnit4 or GUT | Unit + Scene tests |
| Unreal | Automation Framework | Unit + Functional |

Set up the test runner and create the appropriate test assembly/directory structure.

#### All Platforms — Hello World Test

Write the most boring test imaginable. The value is in the infrastructure, not the assertion:
- The test target compiles
- It can import the main module
- Assertions execute
- The test runner reports results

For web projects, test a real utility function (like a `cn()` class name merger) — it proves the test config, path aliases, and imports all work.

### 8.4 External Service Configuration

Set up development instances for all external services from the architecture and API specs.

**For each service, document:**

| Service | Dev Instance | Key Location | Verification |
| --- | --- | --- | --- |
| [name] | [URL/project] | [see key placement below] | [test request] |

**Key placement by platform:**

| Key Type | iOS | Web | Engine |
| --- | --- | --- | --- |
| Public keys (read-only, restricted) | Info.plist / build config | `.env` (server-only) | Build config / ScriptableObject |
| User tokens (OAuth, sessions) | Keychain — never UserDefaults | httpOnly cookies / server session | Keychain / encrypted file |
| Service role keys (admin) | Server-side only — never in client | Server-side only — never in client | Server-side only — never in client |

**[Web] Environment variable safety:** Framework-specific prefixes (`VITE_`, `NEXT_PUBLIC_`, `PUBLIC_`) expose variables to the client bundle. Only prefix variables that are safe to be public. Server-only secrets should have no prefix and only be read in server-side code (loaders, API routes, Edge Functions).

**Static sites with no backend:** If your architecture has no external services (static site, no database, no auth), this section is N/A. Document "No external services — static site" and move on.

Verify connectivity by running a simple request against each service.

#### Infrastructure Pre-flight Checklist

Before committing to any hosted backend or third-party service, verify the following. Discovering cost or account issues mid-sprint is expensive — 5 minutes of pre-flight prevents a session-derailing pivot.

| Check | What to Verify |
| --- | --- |
| **Account limits** | Free tier project count, bandwidth caps, storage limits. Will you hit a wall during development? |
| **Hosting costs** | Pricing at all tiers (free, dev, production). Can you justify the cost at each phase? |
| **Self-hosting viability** | Is there a Docker/self-hosted option as fallback? What's the setup cost? |
| **Required secrets** | List every API key, private key, and credential the service needs. Where will each one live? |
| **Service coupling** | Which app services depend on this backend? (e.g., RLS-protected routes require real auth) |
| **Local development** | Can you run the service locally without internet? Is there a CLI or emulator? |

### 8.5 CI/CD Pipeline

Configure automated builds so broken code is caught before it merges.

**Minimum CI pipeline:**
- Trigger on push to main/develop and on PRs
- Build the project
- Run all tests
- Report status (pass/fail)

#### [Web] Static Hosting Setup

If your architecture calls for static hosting (Cloudflare Pages, Vercel, Netlify), configure it now:

| Provider | Build Command | Output Directory | Config |
| --- | --- | --- | --- |
| Cloudflare Pages | `npm run build` | `build/client` or `dist` | Dashboard or `wrangler.toml` |
| Vercel | `npm run build` | Auto-detected | `vercel.json` (optional) |
| Netlify | `npm run build` | `dist` or `build` | `netlify.toml` |

**Set environment variables:** `NODE_VERSION` (use LTS, e.g., `20`), plus any build-time secrets.

**Create a `_headers` file** (Cloudflare/Netlify) or middleware (Vercel) for security headers:
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`
- `Referrer-Policy: strict-origin-when-cross-origin`
- Cache-Control for immutable assets (`/assets/*`)

For git-push deploys, the hosting provider's GitHub integration is your CI — every push builds and deploys.

#### Code Quality

| Platform | Linter | Formatter |
| --- | --- | --- |
| iOS | SwiftLint | swift-format or Xcode defaults |
| Web | ESLint | Prettier |
| Unity | Rider inspections / .editorconfig | Rider / VS Code |
| Godot | GDScript linter (built-in) | gdformat |
| Unreal | UnrealHeaderTool + clang-tidy | clang-format |

Match linter rules to coding standards from the AI workspace. Zero violations on the empty project.

Start minimal. A CI pipeline that builds and tests is 90% of the value. Deployment, code coverage, and artifact archiving can be added incrementally.

### 8.6 AI Workspace Setup

Configure the AI coding assistant to understand the project without manual briefing every session. This section is universal — all platforms use the same AI tooling.

**Project instructions (`.claude/CLAUDE.md` or equivalent):**
- What the app is (one paragraph)
- Current phase and status
- Key decisions (tech stack, architecture, patterns)
- Project structure reference
- Build/run/test commands
- Pointers to pre-production specs

**Coding rules (`.claude/rules/` or equivalent):**
Create focused rules files (30–80 lines each) for:
- **Language standards** — naming, architecture patterns, error handling, testing framework
- **UI framework rules** — design system enforcement, animation standards, prohibited patterns
- **Security rules** — input validation, credential storage, data privacy, rate limiting
- **Git workflow** — commit format, branching strategy, PR conventions

Each rules file should be imperative ("Do X", "Never Y") — concise rules are followed; verbose guidelines are skipped.

**Memory seeding (`.claude/memory/MEMORY.md` or equivalent):**
Summarize key decisions from Steps 1–7 that would be expensive to re-derive:
- Tech stack and versions
- Architecture pattern and key structural decisions
- Data model summary (names, key relationships)
- Design tokens (colors, typography, spacing)
- Sprint plan summary (current sprint, total sprints)
- Testing approach

MEMORY.md is the cheat sheet an AI reads at session start. Summarize and link — don't duplicate full specs.

**Session hooks (optional but valuable):**
- Session start: restore previous context (branch, recent files, active task)
- Session end: persist current state
- Pre-compact: snapshot before context compression
- Decision logger: persist architectural decisions

**Verification:** Start a fresh AI session. Without any manual context, the AI should answer: What is this project? What tech stack does it use? What architecture pattern? What am I working on right now?

### 8.7 Security Configuration

Create security rules and configure platform-specific security settings.

**Universal security rules (all platforms):**
- Input validation (string lengths, format validation, HTML stripping)
- Error message sanitization (generic user messages, private detailed logs)
- Network security (HTTPS enforcement, logging restrictions)
- Credential storage rules (what's safe where)

**Platform-specific security:**

| Category | iOS | Web | Engine |
| --- | --- | --- | --- |
| Privacy manifest | PrivacyInfo.xcprivacy | N/A | N/A |
| Transport security | App Transport Security | CSP headers, HSTS | Platform TLS settings |
| Credential storage | Keychain — never UserDefaults | httpOnly cookies, server sessions | Keychain / encrypted prefs |
| Bundle safety | No secrets in Info.plist | No secrets with `VITE_`/`NEXT_PUBLIC_` prefix | No secrets in build configs |
| OWASP reference | Mobile Top 10 | Web Top 10 | Mobile Top 10 (if mobile target) |

**Static sites with no user data:** If your architecture has no authentication, no user input, and no API keys, security configuration is minimal — CSP headers and cache policies are sufficient. Document this decision and move on.

### 8.8 Validation

Final verification that the environment is ready for Phase 2:

- [ ] Project builds successfully (correct folder structure from architecture doc)
- [ ] At least one unit test passes
- [ ] CI pipeline triggers on push and reports green (or static hosting builds on push)
- [ ] Each external service responds to a test request (if applicable)
- [ ] Linter runs with zero violations (if configured)
- [ ] AI workspace loads correctly in a new session
- [ ] All pre-production specs are accessible from the project
- [ ] Security configuration documented and verified
- [ ] **[Web]** Static site pre-renders all routes to HTML
- [ ] **[Web]** Client JS bundle is within performance budget
- [ ] **[Engine]** Project runs in the editor on target platform
- [ ] **[Engine]** Build pipeline produces a runnable artifact for target platform

## Deliverable

Complete the [Dev Environment Setup Template](../templates/dev-environment-setup.md) and save it to your project's `docs/pre-production/dev-environment-setup.md`.

## Definition of Done

- [ ] Project compiles with the folder structure from the architecture document
- [ ] All dependencies from the architecture and API specs resolve and build
- [ ] Test targets exist with at least one passing unit test
- [ ] External services configured with development credentials and verified (if applicable)
- [ ] CI/CD pipeline or static hosting runs on push (build + test/deploy)
- [ ] AI workspace configured (project instructions, coding rules, memory)
- [ ] Security rules documented and platform security configured
- [ ] Validation checklist passes (Section 8.8)

## Tips

- **Build the folder structure from the architecture doc, not from a tutorial.** Step 3 already defined where everything goes — don't reinvent it.
- **Pin dependency versions.** "Latest" today is "broken build in 3 months" when a breaking change ships.
- **One passing test proves the pipeline works.** Write the most boring test imaginable. The value is in the infrastructure, not the assertion.
- **Don't skip the AI workspace for "later."** The first Sprint 1 session without it costs more time than setting it up now.
- **Security rules prevent vulnerabilities at generation time.** It's cheaper to never write insecure code than to find and fix it in code review.
- **Separate dev from production early.** Use a separate backend instance for development. Accidentally deleting production data during Sprint 1 is a bad day.
- **The validation checklist is your exit gate.** If any item fails, the environment isn't ready. Fix it before starting Sprint 1.

### [Web] Web-Specific Tips

- **Use the framework's official scaffold.** `create-react-router`, `create-next-app`, `create astro` — these set up the correct build config, TypeScript paths, and development server. Don't build from scratch.
- **Verify SSG output early.** If your architecture calls for static pre-rendering, run the build and check the output directory. Missing routes in the pre-render list mean missing pages in production.
- **Check client bundle size after adding dependencies.** Framer Motion, syntax highlighters, and component libraries add up. Set a budget (e.g., <150KB gzipped) and measure after each dependency.
- **`_headers` files are free security.** Cloudflare Pages and Netlify support static `_headers` files for security headers and cache policies. Add them during setup, not after launch.

### [Engine] Game Engine Tips

- **Create the project manually in the engine's project manager** (Unity Hub, Godot Project Manager, Unreal Launcher). AI-generated engine project files miss renderer assignments, quality profiles, and platform-specific settings. The AI can create the folder structure and code files after the project exists.
- **Clearly separate "AI creates" from "developer does manually."** The AI produces: documentation, coding rules, memory files, folder structure on the filesystem, assembly/module definitions, and starter scripts. The developer does: engine project creation, Editor settings, renderer configuration, build pipeline setup, and first device build.
- **Assembly definitions (Unity) / modules (Godot/Unreal) enforce architecture boundaries.** Create one per module from the architecture document. This is the most valuable thing the AI can set up — it prevents dependency violations from day one.
- **Test framework setup varies by engine.** Unity uses Unity Test Framework (NUnit-based) with Edit Mode and Play Mode test assemblies. Godot uses GdUnit or GUT. Unreal uses the Automation Framework. Set up the test runner and verify one passing test before Sprint 1.
