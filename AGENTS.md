# AGENTS.md - The Split Kit

Project context for AI coding agents working in this repository.

For Cursor-native behavior, see `.cursor/rules/`.
For GitHub Copilot-specific guidance, see `.github/copilot-instructions.md`.

---

## What This Project Is

**The Split Kit** is a podcasting tool for creating, broadcasting, and updating
Podcasting 2.0 value-time-split metadata during live shows.

This repository is the front-end application. It currently contains legacy
behavior and rough UX, and is being rebuilt into a modern, reliable product
that podcasters actually enjoy using.

Primary near-term objective:
- Restore production reliability fast.
- Preserve mission-critical features (live split updates, event workflow).
- Improve UI/UX and maintainability with incremental, testable changes.

---

## Current Stack Snapshot

| Layer | Detail |
|-------|--------|
| Framework | SvelteKit (`@sveltejs/kit` v1) |
| UI | Svelte v4 components in `src/` |
| Build tool | Vite |
| Deployment adapter | `@sveltejs/adapter-vercel` |
| Formatting | Prettier + `prettier-plugin-svelte` |
| Runtime API routes | SvelteKit `src/routes/api/*/+server.js` |
| External companion API | `remoteServer` in `src/stores.js` (dev: `localhost:8000`, prod: `api.thesplitkit.com`) |

Note: This app depends on external services and APIs for significant behavior.
Treat this as a frontend + integration surface, not a standalone monolith.

---

## Key Files

| File | Purpose |
|------|---------|
| `src/stores.js` | Runtime endpoints and shared app state stores |
| `src/routes/(main)/` | Main user-facing app routes |
| `src/routes/api/` | Server routes/proxy helpers |
| `src/lib/functions/` | Shared helper functions used by pages/components |
| `svelte.config.js` | SvelteKit config and deployment adapter |
| `vite.config.js` | Build config and path aliases |
| `task_plan.md` | Phase-based implementation plan |
| `findings.md` | Research and technical findings |
| `progress.md` | Session-by-session execution log |

---

## Critical Rules

### 1) Operational boundaries (mandatory)
Agents are restricted to local workspace edits, local analysis, and local
command execution only.

Never perform remote or infrastructure actions directly, including:
- SSH/SCP/SFTP or direct host access
- remote service/container control on non-local hosts
- direct remote secret/env mutation
- deploy/release/cutover/DNS actions
- pushing branches or creating PRs unless explicitly requested

When remote verification is needed, provide copy/paste commands for the human
operator and continue from the returned output.

### 2) Never commit secrets
- Never commit `.env` files, API keys, tokens, or credentials.
- If credentials appear in code/config, flag them and recommend rotation.

### 3) Preserve existing behavior unless task explicitly changes it
This is a live tool with brittle workflows. Do not refactor critical paths
blindly. Prefer targeted, reversible changes with clear verification steps.

### 4) Keep changes incremental and testable
- Ship in small slices.
- Add or update docs when behavior changes.
- Include manual verification steps for UI-heavy changes.

### 5) No automatic pushing or PR creation
Do not push branches or create pull requests without explicit user consent.

---

## Engineering Principles For This Rebuild

- Prioritize reliability over novelty.
- Isolate external integration points behind small abstractions.
- Reduce hidden side effects in components and stores.
- Prefer typed contracts for payloads as we modernize.
- Improve UX and accessibility as first-class requirements.

---

## Known Risks To Track

- Heavy dependence on external APIs and hard-coded third-party URLs.
- Legacy route/component sprawl with inconsistent architecture patterns.
- Potentially unsafe proxy patterns in current API endpoints.
- Sparse baseline tests and minimal runbook documentation.

Treat these as active modernization targets, not blockers.

