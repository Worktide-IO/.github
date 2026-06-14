<div align="center">

<a href="https://worktide-io.github.io/">
  <img src="https://raw.githubusercontent.com/Worktide-IO/worktide-web/main/public/brand/logo/worktide-lockup.svg" alt="Worktide" height="72" />
</a>

### Open-source project, time and CRM management.

A self-hostable hybrid of [awork](https://www.awork.com/) and [Redmine](https://www.redmine.org/) — multi-tenant from day one, with polymorphic comments, HMAC-signed webhooks, a granular permission matrix, real-time updates over Mercure, and a built-in CRM.

[**Landing page**](https://worktide-io.github.io/) · [**Backend**](https://github.com/Worktide-IO/worktide) · [**Frontend**](https://github.com/Worktide-IO/worktide-web)

</div>

---

## What's inside

| Repo | What |
|---|---|
| [`worktide`](https://github.com/Worktide-IO/worktide) | Symfony 8 + API Platform 4 + Doctrine 3 backend. 60+ entities, JWT + Personal-Access-Token auth, HMAC-signed outbound webhooks, granular per-role permission matrix, Doctrine `onFlush` domain event log, Mercure publishing on every CRUD. |
| [`worktide-web`](https://github.com/Worktide-IO/worktide-web) | React 19 + Vite + Refine 5 + Tailwind v4 + shadcn/ui SPA. Custom Hydra/JSON-LD data provider, OpenAPI codegen via kubb, real-time list updates over Mercure with per-user JWT. |
| `worktide-mobile` | Flutter app — planned, focus on the active timer + quick task capture. |

## Architecture in one paragraph

The backend is a workspace-scoped, multi-tenant API where every entity stacks the same handful of reusable Doctrine traits (UUIDv7 + timestampable + soft-delete + workspace-scope + versioning + audit + external-reference). API Platform exposes the contract; the resource voters delegate up the aggregate chain via the `AccessDecisionManager`; a granular `Capability` × `Role` permission matrix can be overridden per workspace. Every tracked mutation lands in a single immutable `domain_events` log that drives the activity feed, the outbound HMAC-signed webhook dispatcher, and the Mercure live-update publisher. The frontend speaks the same OpenAPI contract through a typed kubb-generated client.

## Highlights

- **Multi-tenancy** as a primitive — JWT + Personal-Access-Tokens, per-workspace permission matrix, voter-based authorization.
- **First-class CRM** — Customer → Contact → CustomerSystem → ServiceSubscription with cents-precision billing and auto-computed next-billing dates. Departs from awork's project-scoped contacts model.
- **Outbound webhooks** with HMAC-SHA256 signatures, retries, delivery log, and auto-deactivation after consecutive failures.
- **Live updates** — `mercure: true` on an entity is enough; the SPA's `useMercureTopic` hook turns SSE frames into TanStack-Query invalidations.
- **Granular per-role permissions** — Capability enum × Role baseline matrix + per-workspace overrides. The owner role is intentionally immutable so no workspace can lock itself out.
- **awork importer** ships in `worktide` — pull a representative slice via Bearer token, replicate idempotently via `(externalSource, externalId)`.

## License

MIT — pick a repo above and look at its `LICENSE`.

## Maintained by

[WapplerSystems](https://wappler.systems) — the Worktide-IO collective of contributors. Worktide started as the internal toolkit for the agency and is being open-sourced as both a working PM tool and a reference build for modern Symfony + React stacks.
