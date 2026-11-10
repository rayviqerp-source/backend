# RAYVIQ Backend

API and services for the RAYVIQ solar-inverter service platform.

The platform connects customers, OEM service managers, service partners and
certified field engineers across one workflow: **request → assignment →
visit → digital service report → OEM review → closure**.

| Repo | Stack | Purpose |
| --- | --- | --- |
| [`rayviqerp-source/frontend`](https://github.com/rayviqerp-source/frontend) | Next.js 16, React 19, Tailwind v4 | Web app, marketplace and role portals (currently a fully interactive client-side demo with a seeded dataset) |
| `rayviqerp-source/backend` (this repo) | TBD | REST/JSON API, persistence, auth, dispatch engine, integrations |

## Status

This repository is the landing place for the backend service. The frontend
currently runs in demo mode: all state lives in a client-side store
(`frontend/lib/store.tsx` + `frontend/lib/seed.ts`) persisted to
`localStorage`. The API contracts below are the target that the backend must
implement so the demo store can be swapped for real endpoints.

## Planned API surface

| Area | Endpoints |
| --- | --- |
| Auth | `POST /auth/login`, `POST /auth/register`, `POST /auth/refresh`, `POST /auth/logout` |
| Engineers | `GET /engineers`, `GET /engineers/:id`, `PATCH /engineers/:id`, `GET /engineers/:id/availability` |
| Sites & equipment | `GET /sites`, `GET /sites/:id`, `GET /equipment`, `GET /equipment/:id` |
| Tickets | `GET /tickets`, `POST /tickets`, `GET /tickets/:id`, `PATCH /tickets/:id/status` |
| Assignments | `GET /assignments`, `POST /assignments/:id/offer`, `POST /assignments/:id/accept`, `POST /assignments/:id/reject` |
| Visits | `GET /visits`, `POST /visits/:id/check-in`, `POST /visits/:id/report` |
| AMC | `GET /amc/contracts`, `POST /amc/contracts`, `GET /amc/visits` |
| OEM | `GET /oem/products`, `GET /oem/customers`, `GET /oem/reports` |
| Notifications | `GET /notifications`, `POST /notifications/:id/read` |
| Audit | `GET /audit`, `GET /timeline/:entityType/:entityId` |

### Conventions

- **Auth** — JWT access token + refresh token; role claims mirror the demo
  roles (`customer`, `engineer`, `vendor`, `oem`, `admin`).
- **Status machine** — ticket and assignment transitions are enforced exactly
  as in `frontend/lib/constants.ts` (`TICKET_TRANSITIONS`); invalid
  transitions must be rejected by the server.
- **Audit trail** — every state change writes an append-only audit entry and a
  timeline event, matching the demo behaviour in `frontend/lib/store.tsx`.
- **Errors** — RFC 7807 style problem details, `4xx` for validation,
  `409` for illegal transitions, `403` for permission failures.

## Conventions for this repo

- Keep secrets out of git — `.env` files are ignored.
- One service per folder; migrations live next to the code they belong to.
- Open a pull request for every change; `main` stays deployable.

## Getting started

The service stack is still being finalised. Once the runtime is picked, this
section will be replaced with the actual bootstrap commands.
