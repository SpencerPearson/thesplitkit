# The Split Kit - Progress Log

## 2026-04-23

### Completed
- Replaced stale `AGENTS.md` content with Split Kit-specific project guidance.
- Added `task_plan.md` with phased rebuild plan and rolling queue.
- Added `findings.md` with initial codebase/stack/risk observations.
- Performed first-pass technical assessment of app structure and dependencies.
- Added and triaged candidate backend repo (`thesplitbox`) in the same workspace.
- Confirmed backend is related but not contract-compatible with current `thesplitkit` frontend.
- Implemented new `thesplitkit-api` service with V1 compatibility routes and realtime `/event`.
- Added V1 API contract doc, schema doc, parity backlog, and deploy runbook.
- Added contract smoke tests and validated pass against local API runtime.
- Updated root dev workflow to run frontend + `thesplitkit-api` together.
- Updated root dev workflow to target sibling `../thesplitkit-api` path.
- Implemented first real Alby parity slice in `thesplitkit-api`:
  - `/api/alby/auth`
  - `/api/alby/refresh`
  - `/api/alby/logout`
  - `/api/alby/handlePayments` (keysend + LN address attempt)

### In Progress
- Clarifying stack direction (stay on SvelteKit vs migrate).
- Building initial reliability triage list for immediate fixes.
- Defining compatibility-layer approach for required `/api/sk/*` endpoints.
- Verifying Alby production credentials and full OAuth end-to-end callback behavior.

### Next Actions
- Verify local startup path and required environment variables.
- Prioritize and fix high-risk API route issues.
- Establish baseline docs: real README + local/dev/prod runbook.
- Build endpoint mapping doc and implement first backend compatibility endpoints.
- Replace triggerboard/soundboard stubs with persistent event-scoped storage.
- Implement webhook settlement signature flow and payment retry queueing.

## 2026-04-24

### Completed
- Alby OAuth callback/refresh/logout flow validated against local frontend + `thesplitkit-api`.
- Added Podcast Index backend route support and explicit auth failure payloads.
- Added server-side Podcast Index search endpoint (`/api/queryindex/search`) with cache.
- Switched Add Music flow from huge client-side feed preload to debounced server-side search.
- Replaced stuck "Loading Songs" behavior with explicit error/loading states.
- Implemented top-down design system baseline in frontend:
  - tokenized theme variables
  - persistent light/dark mode toggle
  - app shell/header/menu modernization
- Reworked high-traffic pages to consume shared UI primitives:
  - main landing route
  - events list
  - event creator
  - event dashboard shell/header
  - live route and live dark route
- Replaced oversized image-based loading experiences with reusable spinner component.
- Themed modal stack and nested add-block flows (`SelectBlock`, `AddFeed`, `AddMusic`, `AddPlaylist`, `SongSelect`).
- Performed consistency sweep on remaining dashboard/live child components (desktop + mobile variants).
- Replaced frontend README boilerplate with project-specific run, env, and integration documentation.

### In Progress
- Continuing visual consistency pass over remaining deep/niche route components outside core workflows.
- Defining minimum quality gate (lint + basic smoke + contract checks) for routine merges.
- Planning onboarding/help improvements for first-time operators.

### Next Actions
- Finish theming any remaining outlier components in `live/*`, `promotion/*`, and `spotlight` variants.
- Add lightweight UI smoke tests for event create -> block add -> live route view path.
- Harden legacy proxy endpoints and document a security checklist before production rollout.
- Add triggerboard/soundboard persistence (replace stubs) in backend parity backlog.
