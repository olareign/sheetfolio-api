# Sheetfolio API

The NestJS backend for [Sheetfolio](https://github.com/olareign/sheetfolio), where new server-side work lives.

**Status: not connected to the web app.** The Sheetfolio web app keeps its own sign-in, onboarding and profile flow
(Next.js server actions on Upstash Redis), so this service is not deployed and costs nothing to run while the product
is market tested. Nothing in the web app calls it. It was split out of `apps/api` in the Sheetfolio monorepo, with its
history, to be developed on its own.

## What is here

| Area      | What                                                                                                |
| --------- | --------------------------------------------------------------------------------------------------- |
| `config/` | Environment validated with Zod at boot; production refuses to start without its required settings.  |
| `email/`  | `EmailService`: sends through Resend's REST API, or prints to the log in development without a key. |
| `health/` | `GET /health` returns `{ status, email }`, where `email` is `resend` or `console`.                  |

## Run it

Needs Node 20.19+ (or 22.12+) and pnpm. NestJS 12 ships as ESM.

```bash
pnpm install
cp .env.example .env
pnpm dev            # http://localhost:4000
```

| Variable                        | Notes                                                                                |
| ------------------------------- | ------------------------------------------------------------------------------------ |
| `PORT`                          | Listen port (default 4000).                                                          |
| `WEB_ORIGIN`                    | The one browser origin allowed to call the API (CORS). Required in production.       |
| `RESEND_API_KEY`, `RESEND_FROM` | Email. Required in production. Without a key in development, mail is logged instead. |

## Scripts

| Script                         | Does                                     |
| ------------------------------ | ---------------------------------------- |
| `pnpm dev`                     | Start with file watching                 |
| `pnpm build` / `pnpm start`    | Compile to `dist/` / run the build       |
| `pnpm test`                    | Unit and end-to-end tests (Vitest + SWC) |
| `pnpm typecheck` / `pnpm lint` | `tsc --noEmit` / ESLint                  |
| `pnpm format` / `format:check` | Prettier                                 |

Tests compile with SWC rather than esbuild because Nest's dependency injection needs emitted decorator metadata.

## Plan

Backend work comes here as the product grows, starting with the tracked links and the weekly job digest from the
founder plan. Wiring the web app to this API is a deliberate later step, taken when there is revenue to host it.
