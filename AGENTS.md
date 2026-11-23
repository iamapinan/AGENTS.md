# AGENTS.md

Universal guidelines and engineering standards for AI coding agents. Explicit user instructions override this file; mention any conflict before proceeding.

---

## 1. PRIORITIES (Conflict Resolution)
1. **Correctness & Security:** Zero regressions, safe boundary validation, no secrets leaked.
2. **Accessibility & Contrast:** WCAG AA compliance, dark/light theme parity, keyboard navigation.
3. **Repository Conventions:** Follow existing project patterns, directory layout, and naming style.
4. **Architecture & Clean Code:** Separation of concerns, minimal complexity, strict typing.
5. **UI Polish & Micro-interactions:** Only after functionality is verified and passing tests.

---

## 2. WORKFLOW & DISCIPLINE
- **Karpathy Coding Discipline (Mandatory):**
  1. **Think Before Coding:** Inspect existing code, tests, docs, and configs first. Explicitly state assumptions. Push back on speculative flexibility or unneeded abstractions. Ask only when ambiguity makes proceeding risky.
  2. **Simplicity First:** Implement the minimal code that fully solves the problem. No single-use helper frameworks or speculative boilerplate.
  3. **Surgical Changes:** Touch only what the task requires. Never reformat, rename, or reorder unrelated code. Clean up newly obsolete code.
  4. **Goal-Driven Execution:** Define observable success criteria. Reproduce bugs with focused tests before fixing. Verify with concrete checks (typecheck, tests, build) before asserting completion.
- **Reuse Before Creating:** Check existing shared utilities, components, services, and tokens first.
- **Architectural Planning:** Write a brief plan if a change touches >3 files, database schemas, or public API contracts.
- **Dependencies:** Do not add third-party libraries without explicit justification or user consent.
- **Memory & Notes:** Keep project notes/context updated in `[PROJECT_MEMORY_PATH]` (e.g., `docs/MEMORY.md` or local workspace notes).

---

## 3. ARCHITECTURE & CODE STANDARDS
- **Clean Architecture & Layering:**
  - **Presentation:** Views, UI components, pages, CLI commands, HTTP handlers.
  - **Application/Domain:** Business logic, use cases, workflows, custom hooks, state management.
  - **Infrastructure:** DB models/queries, network clients, filesystem access, external SDKs.
  - *Strict Rule:* Never call infrastructure/database queries directly from presentation views.
- **File Sizing:** One primary entity/component/service per file. Target <300 lines (hard limit: 800 lines). Proactively extract subcomponents or helpers.
- **Strict Typing:** No escape hatches (`any`, untyped `dict`, empty `interface{}`). Validate all external inputs at the boundary using schema validators (e.g., Zod, Pydantic, JSON Schema).
- **Language:** English for identifiers, commit messages, code comments, and technical documentation. Comments must explain *why*, not *what*.
- **Git Commits:** Conventional Commits (`feat:`, `fix:`, `refactor:`, `test:`, `docs:`, `chore:`).
- **Automated Tests:** Add regression tests for bug fixes and unit/integration coverage for new domain logic. Never mute, disable, or delete failing tests to make a build pass.

---

## 4. FRONTEND, LAYOUT & UX (For Projects with UI)
- **Contrast & Theme Parity:**
  - Strict WCAG AA contrast (minimum **4.5:1** for text, **3:1** for icons, borders, and large text).
  - Both Light and Dark modes must have explicitly paired surface and foreground tokens.
  - Form placeholders and disabled states must remain legible (`>= 3:1` contrast).
- **Semantic Status Hygiene:**
  - Reserve warning (`amber/yellow`) and danger (`red`) exclusively for real errors, hazards, or abnormal states.
  - Never style normal, empty, or safe states (e.g., "0 issues", "None", "Active") with warning/danger colors. Use neutral or success tokens.
- **Anti-Slop UI & Geometry:**
  - **No Thick Side-Border Accents:** Avoid `border-l-4` or vertical color stripes on card sides. Use clean, uniform, symmetrical borders with soft background tinting or subtle elevation.
  - **No Emoji as UI Icons:** Use a single, cohesive SVG icon set across the product.
  - **Ban Generic AI Tells:** Never use `Sparkles`, magic wands, or star-burst icons for routine features. Use semantic icons representing the actual action (e.g., `Search`, `Scan`, `Eye`, `Settings`).
- **Button & Action UX:**
  - **No Wordwrap:** Action triggers and buttons must enforce `white-space: nowrap; flex-shrink: 0;`.
  - **Short Single-Language Labels:** Use concise action verbs (e.g., `Save`, `Export`, `Print`, `Cancel`). Never combine dual languages or append redundant parentheses like `(PDF)` or `(Test)`.
  - **Touch Targets:** Minimum `44x44px` clickable area for all interactive elements on touch screens.
- **Asynchronous Loading UX:**
  - **Skeletons over Spinners:** Use geometry-matched skeleton placeholders for initial loads, lists, tables, and page transitions (CLS = 0).
  - Keep static layout shells (navigation, headers, filters) visible; never unmount them during background fetching.
  - Reserve circular spinners strictly for inline micro-actions (e.g., inside an active submit button).
- **Data Tables & Grids:**
  - Always wrap wide tables in horizontal scroll containers with sticky headers (`<thead>`).
  - Right-align numbers with tabular/monospace digits (`font-variant-numeric: tabular-nums`). Left-align text.
  - Pin primary identifier and action columns on wide multi-column datasets.
- **Forms & Inputs:**
  - Persistent visible `<label>` above every input (never placeholder-as-label).
  - Single-column layout by default (`max-width: 36rem–42rem`); multi-column only for short paired fields.
  - Inline error text below inputs linked via accessibility attributes (`aria-describedby`).
  - Disable submit buttons with loading indicators during in-flight requests to prevent double-submits.

---

## 5. API, BACKEND & SECURITY
- **REST / API Standards:** Plural nouns (`/api/v1/resources/:id`), standard HTTP verbs, and consistent JSON envelope:
  - Success: `{ "success": true, "data": ... }`
  - Error: `{ "success": false, "error": { "code": "...", "message": "...", "details": ... } }`
- **Sanitized Errors:** Never leak stack traces, database schema details, or raw SQL errors to clients (return sanitized `500 Internal Error`).
- **Input Validation:** Schema-validate and sanitize all payloads (body, query, headers, params) before business execution. Validate environment variables on startup.
- **Security Hardening:**
  - Enforce authentication, role authorization, and tenant/object-level ownership checks (IDOR prevention) on all protected routes.
  - Parameterized queries / ORM bindings only (strictly no string concatenation into queries).
  - Verify cryptographic signatures on external webhooks. Rate-limit exposed endpoints.
  - Zero hardcoded secrets or API tokens. Never expose server secrets in public/client bundle configs.

---

## 6. POLISH & MICRO-INTERACTIONS (Optional/Finishing Pass)
- **Concentric Radius:** `outer-radius = inner-radius + padding (+ border-width)` for nested surfaces.
- **Controlled Transitions:** Never use `transition: all` or `will-change: all`. Specify exact properties (`transform`, `opacity`, `background-color`).
- **Interactive Feedback:** Micro-scale on active/pressed state (`scale(0.97)` over `120ms`) only when enabled.
- **Accessibility & Motion:**
  - Gate motion animations behind `@media (prefers-reduced-motion: no-preference)`.
  - Guard hover effects with `@media (hover: hover)` to avoid sticky hover states on mobile touchscreens.

---

## 7. PRE-FINISH VERIFICATION CHECKLIST
Before claiming any task complete, confirm:
- [ ] Linting, typechecking, tests, and build command all pass with 0 errors.
- [ ] New/edited code adheres to Clean Architecture and touches only relevant files.
- [ ] No hardcoded secrets, untyped `any`, unhandled exceptions, or unvalidated boundaries.
- [ ] Contrast meets WCAG AA in both light and dark themes (if UI).
- [ ] Buttons and labels enforce `nowrap` without awkward text breaks (if UI).
- [ ] Asynchronous states use skeleton loaders without layout shift (if UI).
- [ ] All newly introduced logic is covered by automated unit/integration tests.