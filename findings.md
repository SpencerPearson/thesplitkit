# The Split Kit - Findings

## Current Snapshot (2026-04-23)

### Stack and Runtime
- Frontend is SvelteKit v1 with Svelte v4 and Vite.
- Deployment adapter is currently Vercel (`@sveltejs/adapter-vercel`).
- App relies on a companion backend API (`remoteServer`) not contained in this repo.

### Project State
- `AGENTS.md` was a stale copy from an unrelated Django project and has been replaced.
- Frontend `README.md` now documents real local run paths, env variables, and backend contracts.
- The route surface area is large (many feature-specific pages under `src/routes/(main)`).

### Integration Observations
- Production API host appears to be `https://api.thesplitkit.com`.
- Several hard-coded external URLs are embedded directly in route components.
- Dev script depends on sibling directory `../00-curiohoster` for local API startup.
- Candidate backend repo identified and added to workspace: `thesplitbox`.

### Risk Observations
- `src/routes/api/proxy/+server.js` proxies arbitrary URLs with no validation (SSRF risk).
- `src/routes/api/contenttype/+server.js` references `isValidURL` but does not define/import it.
- API route response patterns are inconsistent across files and likely brittle.

## Backend Candidate Triage (`thesplitbox`)

### Why this is likely related
- Domain and metadata references line up with Split Kit usage (`thesplitbox.com`, `api.thesplitkit.com`).
- Purpose lines up: invoice webhooks, split resolution, boost forwarding, Alby integration.
- Contains both decoupled client and backend server, consistent with prior architecture notes.

### Stack and shape
- Node/Express backend under `server/`.
- Mongo-backed metadata/settings store.
- Alby OAuth + payment routes under `/alby`.
- Split routes mounted at root (`/invoice`, `/save-settings`, `/fetch-settings`, etc.).

### Contract mismatch with current frontend
- Current `thesplitkit` frontend expects many `/api/sk/*` endpoints:
  `login`, `register`, `refresh`, `checkforuser`, `getevents`, `getblocks`,
  `saveblocks`, `verifyowner`, `generateguid`, `savetriggers`, `savesounds`,
  `saveremotecreds`.
- `thesplitbox` currently exposes different route names and shape
  (`/invoice`, `/save-settings`, `/fetch-settings`, `/metadata/:id`, etc.).
- Conclusion: this backend is useful, but not drop-in compatible with the current frontend contract.

### High-risk issues in backend candidate
- Broad CORS (`origin: "*"`) on sensitive payment/webhook routes.
- Hard-coded production hosts in server logic (`api.thesplitkit.com`, `curiohoster.com`).
- Session/auth and cookie flows are inconsistent and tightly coupled to Alby tokens.
- Sparse tests and limited operational docs for deploy/recovery.

## Viability Verdict
- Salvageable as the base backend, but requires a compatibility layer and hardening.
- Recommended approach: patch and wrap, not full rewrite first.

## Rebuild Status Update (2026-04-24)

- New compatibility backend scaffolded at `thesplitkit-api`.
- Core `/api/sk/*` V1 contract implemented for auth, events, blocks, settings, owner checks.
- `/event` websocket namespace implemented for live block broadcasts and reconnect snapshots.
- Public lookup endpoint implemented: `/api/sk/event/lookup`.
- Compatibility stubs shipped for non-core routes to keep old UI from hard failing.
- Contract smoke test added and passing via `npm run test:contract`.
- Implemented real Alby parity baseline in `thesplitkit-api`:
  - OAuth auth callback exchange (`/api/alby/auth`)
  - token refresh (`/api/alby/refresh`)
  - logout (`/api/alby/logout`)
  - payment forwarding handler (`/api/alby/handlePayments`)

## UX/Frontend Modernization Update (2026-04-24)

- Added global design token layer with light/dark theme support and persisted user theme preference.
- Reworked core app shell and header/menu hierarchy for cleaner information architecture.
- Added shared UI primitives (page/surface/topbar/input/button/list/motion) and started route adoption.
- Replaced image-based loader patterns in touched flows with a reusable spinner loader.
- Eliminated major dark-mode readability regressions in modal-heavy flows (`SelectBlock`, add-feed/music/playlist paths).
- Large portions of event dashboard and live route child components now consume theme tokens instead of hard-coded light colors.

### Performance/UX Observations

- Previous "load all music feeds then filter in-browser" model was brittle and hid API errors.
- Server-side Podcast Index search with debouncing materially improves reliability and perceived responsiveness.
- Legacy duplicated CSS blocks still exist in some long-lived components and can override intended theme behavior.

### Residual Risks

- Several older component trees still contain duplicated style declarations and ad-hoc custom elements.
- UI consistency still depends on incremental route-level cleanup; not all peripheral routes are fully migrated.
- Proxy-related security hardening is still pending for production posture.

## Open Questions
- Is `thesplitbox` the production backend source of truth, or an older branch?
- What are the non-negotiable workflows for live shows (exact happy path)?
- Which authentication provider(s) are required long term?
- What hosting target do we want for frontend and backend (Vercel/Fly/Render/etc.)?

## Recommendations In Progress
- Keep SvelteKit for fast stabilization unless there is a hard blocker.
- Start with security and reliability fixes in API routes.
- Add real project README and deployment/runbook early in the process.
- Add an adapter layer that implements the current `/api/sk/*` contract over hardened `thesplitbox` internals.
