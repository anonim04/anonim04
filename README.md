# Akbar

**Backend Engineer** · Jakarta, Indonesia

I build backend services and the tooling that keeps them honest — lately the architecture behind them too. Currently at **Wondr by BNI**, working on
event-driven microservices in digital banking (Java / Spring Boot, Python / FastAPI, Golang, Apache Kafka,
Red Hat OpenShift). Before that, two years leading mobile test automation at the same bank — which is where
most of my instinct for testable, observable systems comes from.

> **A note on this profile.** Most of my substantial work lives in private repositories — employer code and
> personal systems that run in production with real configuration in them. The two projects below are mine,
> built solo, and I'm happy to walk through the architecture, the trade-offs, or the code itself in a
> conversation.

---

## Kasier POS — multi-tenant point-of-sale platform

`private` · Jan 2026 – Aug 2026 · solo · 637 commits · 181k lines of production code · 1,748 automated tests

A full point-of-sale system for small multi-branch businesses: a Laravel API, a Vue 3 back office and web POS,
and a Flutter cashier app that keeps selling when the network is gone.

- **368-endpoint REST API** on Laravel 11 / PHP 8.2 over PostgreSQL — 95 tables, 146 migrations.
- **Three-level data isolation** (platform → tenant → branch) enforced through Eloquent global scopes rather
  than hand-written `where` clauses, so a single forgotten filter can't leak across tenants. Permission naming
  is kept consistent between routes, seeders, and the frontend router by automated tests — a typo fails the
  build instead of silently evaluating to `false`. 128 permissions, 368 gated routes.
- **Offline-first mobile POS** on Drift/SQLite with a sync queue. Sales, held orders, discounts, barcode
  scanning, and shift open/close all work without connectivity — which required replicating the server's
  promotion engine on-device, and two layers of idempotency (cache lock plus a persistent `Idempotency-Key`)
  so a retried transaction is never counted twice.
- **Cut authenticated-request overhead from ~11 table reads to 1** by serving user, role, and permission data
  from Redis with pre-loaded relations and four-path version-based invalidation — so revoking access takes
  effect immediately instead of waiting out a TTL. Covers both session-based web clients and bearer-token
  mobile clients.
- **1,748 automated tests** (940 PHPUnit, 361 Vitest, 447 Flutter) behind a CI/CD pipeline that refuses to
  build an image when a single test fails, and a documentation gate that verifies 370 endpoints and 95 tables
  against the real code on every build.

`Laravel 11` `PHP 8.2` `Vue 3` `TypeScript` `Pinia` `TailwindCSS` `Flutter` `Bloc` `Drift`
`PostgreSQL` `Redis` `PgBouncer` `Docker` `FrankenPHP` `Caddy` `GitHub Actions` `Harbor`

---

## Foundation Server — self-hosted infrastructure platform

`private` · Aug 2025 – Aug 2026 · solo · 666 commits · 23.3k lines · 24 services

The infrastructure the project above runs on. Ten components on Docker Swarm on a single VPS — the
"build it yourself" answer to managed database, registry, secret, and monitoring services, taken far enough
that it actually holds up in production.

- **24 services across 6 segmented overlay networks**, each with resource limits, health checks, backups,
  alerts, and its own dashboard, all deployed by a single command with dependency ordering handled in script.
- **Continuous deployment built from scratch.** Pushing an image to Harbor promotes it to production: Harbor
  emits a webhook over a private overlay network to an agent inside the Swarm — no public endpoint at all —
  and the agent deploys *by digest*, not by tag. The live version can always be named exactly, and a rollback
  is one digest away. Designed as a written spec first, then verified with a canary image built to fail
  deterministically.
- **Observability across every layer**: 34 alert rules, 40 recording rules, and 7 custom Grafana dashboards
  covering node, container, PostgreSQL, PgBouncer, ingress, backup failures, WAL archiving, error rates, and
  TLS certificate expiry.
- **Layered backups with a tested restore path** — pgBackRest with 4 full / 7 differential retention plus WAL
  archiving for point-in-time recovery, and a separate daily document-volume backup with a minimum-keep floor
  as a brake against silent backup failure.
- **Self-audited the entire repository** — 15 findings, 3 of them critical. The worst: three monitoring
  services were publishing ports straight to the internet through Swarm ingress mode, bypassing the host
  firewall because Docker manages its own iptables rules.

`Docker Swarm` `Traefik v3` `PostgreSQL` `PgBouncer` `pgBackRest` `Harbor` `Trivy` `Infisical`
`Prometheus` `Grafana` `Loki` `Alertmanager` `Portainer` `Bash` `GitHub Actions`

---

## Also

At work I build product services and the internal tooling around them. Right now that means owning the
backend architecture of a new multi-tenant CMS platform — the design documents, the platform standards every
service follows, and the services themselves — and acting as architect and technical reviewer for a
server-driven UI framework built by another squad, where my job is the security model, the task breakdown,
and reviewing what the team ships.

Earlier I built an API regression and smoke-testing platform with a custom IntelliJ plugin, and a single-node
analytics engine for a personal finance feature.

All of it lives in my employer's repositories, so the details belong in a conversation rather than on this
page.

## Certifications

| Certificate Name | Platform | Verification |
| :--- | :--- | :--- |
| **Problem Solving (Intermediate)** | HackerRank | [![Badge](https://img.shields.io/badge/View_Certificate-2EC866?style=flat-square&logo=hackerrank&logoColor=white)](https://www.hackerrank.com/certificates/eaf168886a02) |
| **REST API (Intermediate)** | HackerRank | [![Badge](https://img.shields.io/badge/View_Certificate-2EC866?style=flat-square&logo=hackerrank&logoColor=white)](https://www.hackerrank.com/certificates/ebc9192a29d0) |
| **SQL (Advanced)** | HackerRank | [![Badge](https://img.shields.io/badge/View_Certificate-2EC866?style=flat-square&logo=hackerrank&logoColor=white)](https://www.hackerrank.com/certificates/37901c9c9c99) |
| **Javascript (Intermediate)** | HackerRank | [![Badge](https://img.shields.io/badge/View_Certificate-2EC866?style=flat-square&logo=hackerrank&logoColor=white)](https://www.hackerrank.com/certificates/a6449e826767) |

## Elsewhere

[LinkedIn](https://www.linkedin.com/in/akbar-akbar-425a08137) · akbar.tahir89@gmail.com
