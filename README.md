# The Split Kit (Frontend)

The Split Kit frontend for building and switching live value split blocks during
podcast streams.

This app talks to a companion backend (`thesplitkit-api`) for auth, event/block
CRUD, Alby OAuth, Podcast Index search, and realtime broadcast updates.

## What This Repo Contains

- SvelteKit UI application (`src/`)
- Event creation and dashboard editing workflows
- Live presentation routes (`/live/*`, `/promotion/*`, `/video/*`)
- Shared design tokens with light/dark theme toggling

## Prerequisites

- Node.js 20+ recommended
- `thesplitkit-api` checked out as a sibling directory:
  - `../thesplitkit-api`

## Environment

Create `./.env.local` from `./.env.example`:

```bash
VITE_ALBY_CLIENT_ID=your-alby-client-id
VITE_ALBY_CLIENT_ID_PROD=your-production-alby-client-id
```

## Local Development

Install dependencies:

```bash
npm install
```

Start frontend + sibling API together:

```bash
npm run dev
```

This runs:

- frontend on `http://localhost:3000`
- API via `../thesplitkit-api` (default `http://localhost:8010`)

If you only want the frontend process:

```bash
npm run vite
```

## Build / Preview

```bash
npm run build
npm run preview
```

## Key Runtime Contracts

- Auth/session endpoints under `/api/sk/*`
- Alby OAuth/payment endpoints under `/api/alby/*`
- Podcast Index proxy/search endpoints:
  - `/api/queryindex`
  - `/api/queryindex/search`
- Realtime updates on `/event` Socket.IO namespace

## Current Focus

- Compatibility-first reliability for live value block switching
- UI modernization (material-inspired, theme-aware components)
- Incremental refactor of nested dashboard/live child components
