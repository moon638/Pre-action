# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Frontend**: React + Vite + TailwindCSS v4 + shadcn/ui

## Artifacts

### prediction-dashboard (React + Vite)
- Path: `/`
- A Real-Time Human Pre-Action Prediction System dashboard
- Dark ops-center UI (navy/electric blue/amber)
- Pages: Live Monitor (`/`), Activity Logs (`/logs`), Alerts Feed (`/alerts`), Analytics (`/analytics`)
- Uses all Orval-generated hooks from `@workspace/api-client-react`

### api-server (Express 5)
- Path: `/api`
- Routes: `/api/predict`, `/api/logs`, `/api/logs/:id/feedback`, `/api/stats/summary`, `/api/stats/recent-alerts`, `/api/stats/top-environments`, `/api/cameras`
- Uses `@workspace/db` (Drizzle + PostgreSQL)
- Contains AI prediction engine in `src/lib/prediction_engine.ts` that simulates environment detection, person detection, and action prediction

## Database Tables

- `prediction_logs` — stores frame processing results (camera_id, environment, persons, predictions, alerts, feedback)
- `cameras` — tracks active cameras and frame counts

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally

## Architecture

The prediction engine (`artifacts/api-server/src/lib/prediction_engine.ts`) simulates the full AI pipeline:
1. Environment detection (from location_tag or random inference)
2. Person detection (random count, bounding boxes, micro-actions)
3. Action prediction (top 3 per person from environment-specific action lists)
4. Alert generation (crowd_detected or low_confidence_behavior)
5. Logging to PostgreSQL

See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details.
