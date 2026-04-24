# The Split Kit - Task Plan

## Mission
Rebuild and stabilize The Split Kit so podcasters can reliably broadcast and
update value-time-split tags during live shows, with a modern UX and maintainable
codebase.

## Phases

### Phase 0 - Discovery and Baseline
- [x] Confirm local run instructions and required services.
- [x] Inventory critical user journeys (auth, event creation, live broadcast, split edits).
- [x] Map external dependencies (companion API, Podcast Index, Alby, others).
- [x] Identify production blockers and high-risk code paths.
- [x] Confirm whether `thesplitbox` is canonical backend and identify deployment owner.

### Phase 1 - Stabilization
- [x] Fix critical runtime errors and broken API/proxy behavior.
- [ ] Remove obvious security risks and unsafe defaults.
- [x] Ensure environment configuration is explicit and documented.
- [x] Define and validate deployment target and minimum runbook.
- [x] Add backend compatibility routes for required `/api/sk/*` endpoints.
- [x] Replace hard-coded domains in backend with env-driven config.

### Phase 2 - Product Core Hardening
- [ ] Refactor high-churn modules with clear boundaries.
- [ ] Add lightweight tests around core flows.
- [x] Standardize data contracts for split payloads and event state.
- [ ] Improve resilience around remote save and reconnect scenarios.

### Phase 3 - UX and Design Overhaul
- [x] Establish design system primitives (spacing, typography, color, components).
- [ ] Rebuild key workflows for clarity and speed. (in progress)
- [ ] Improve accessibility (focus states, keyboard nav, contrast, semantics). (in progress)
- [ ] Add better onboarding and in-app guidance.

### Phase 4 - Launch Readiness
- [ ] Document deploy/rollback/checklist.
- [ ] Run smoke tests on staging/preview.
- [ ] Validate analytics/logging/error monitoring.
- [ ] Cut production release with rollback plan.

## Work Queue (Rolling)
- [x] Audit `src/routes/api/*` for security and reliability issues.
- [ ] Catalog route/component ownership and dead code.
- [x] Draft target architecture for frontend + companion API contracts.
- [ ] Define minimum quality gate (lint + basic test + smoke script).
- [x] Build endpoint mapping matrix (`thesplitkit` expected vs `thesplitbox` current).
- [x] Prioritize first compatibility slice: `refresh`, `checkforuser`, `getblocks`, `saveblocks`.

## Decisions Log
- 2026-04-23: Rewrote agent context from Bowl After Bowl to The Split Kit.
- 2026-04-23: Started structured plan/findings/progress docs for rebuild.
- 2026-04-24: Standardized local API port to 8010 and aligned frontend `remoteServer`.
- 2026-04-24: Added `Podcast Index` proxy/search routes and switched Add Music to server-side search.
- 2026-04-24: Began top-down UI redesign with tokenized light/dark theming and component primitives.
