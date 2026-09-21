# Udalitatoo — laser removal brand & studio operations

**Udalitatoo** is the public web presence and back-office stack for **Maxim Buketov** ([udalitatoo.ru](https://udalitatoo.ru)): a marketing landing and SEO journal for laser tattoo and permanent-makeup removal in Samara, a multi-role **Buketov** CRM for the studio, and a **Learn** PWA for internal training and student access. Modules share one product story but live in separate private repos for deploy boundaries. Built and owned by Maxim Buketov; engineering and delivery by [asydneysummer](https://github.com/asydneysummer).

**Production:** [udalitatoo.ru](https://udalitatoo.ru) (landing + journal), [app.udalitatoo.ru](https://app.udalitatoo.ru) (Buketov CRM), [learn.udalitatoo.ru](https://learn.udalitatoo.ru) (Learn / blog CMS admin & student cabinet).

This repository (**[udalitatoo-ecosystem](https://github.com/asydneysummer/udalitatoo-ecosystem)**) is the public portfolio index for the Udalitatoo module family. Application source lives in the linked private repos below.

## Features

- **Public landing** — hero video, services, FAQ, carbon peeling section, contacts, PWA manifest, and local SEO for Samara
- **Journal** — static article hub at `/journal/` with RSS (`feed.xml`), Open Graph, canonical URLs, and Yandex full-text feeds for long-form care and procedure content
- **Buketov CRM** — installable PWA for beauty-studio operations: role-based dashboards, booking, clients, portfolio, blog tooling, finance, and Telegram hooks (see production app)
- **Learn** — separate host for admin learning management (modules, students, invites, analytics) and a student cabinet; login routing sends students to `learn.udalitatoo.ru`

## Screenshots gallery

| Landing (production) | Journal index |
| --- | --- |
| ![Landing home](docs/screenshots/landing-home.png) | ![Journal index](docs/screenshots/journal-index.png) |

| Journal article — carbon peeling | Journal article — procedure |
| --- | --- |
| ![Journal article carbon](docs/screenshots/journal-article-karbon.png) | ![Journal article bolno](docs/screenshots/journal-article-bolno.png) |

## Roles & capabilities

Evidence from production PWAs (`app.udalitatoo.ru`, `learn.udalitatoo.ru`); full auth rules live in private module repos.

| Role | Product | Can do |
|------|---------|--------|
| **Public visitor** | [udalitatoo.ru](https://udalitatoo.ru) | Browse landing sections, read journal articles and RSS, follow service/contact CTAs |
| **Admin** | [app.udalitatoo.ru](https://app.udalitatoo.ru) | Dashboard; users; masters; site & journal analytics; finance; payments; expenses; blog; Telegram integration |
| **Master** | [app.udalitatoo.ru](https://app.udalitatoo.ru) | Dashboard; calendar; schedule; services; site & journal analytics; clients; portfolio; blog |
| **Client** | [app.udalitatoo.ru](https://app.udalitatoo.ru) | Dashboard; browse masters; book appointments; view appointments and photo history; read blog |
| **Admin (Learn)** | [learn.udalitatoo.ru](https://learn.udalitatoo.ru) | Learning content admin; students; invites; analytics (admin routes) |
| **Student** | [learn.udalitatoo.ru](https://learn.udalitatoo.ru) | Student cabinet on the Learn host (separate from main CRM origin after login) |

Users with multiple CRM roles can switch between **admin**, **master**, and **client** inside the Buketov app (role switcher in the production PWA).

## Architecture / tech map

```
┌──────────────────────────────────────────────────────────────────┐
│ Udalitatoo ecosystem (portfolio index — this repo)               │
└───────────────────────────────┬──────────────────────────────────┘
                                │
        ┌───────────────────────┼───────────────────────┐
        ▼                       ▼                       ▼
┌───────────────┐     ┌─────────────────┐     ┌─────────────────┐
│ udalitatoo    │     │ buketov-app     │     │ blog-cms        │
│ static site   │     │ React/Vite PWA  │     │ React/Vite PWA  │
│ landing+      │     │ app.udalitatoo  │     │ learn.udalitatoo│
│ journal       │     │ .ru             │     │ .ru             │
└───────┬───────┘     └────────┬────────┘     └────────┬────────┘
        │                      │                       │
        │  journal HTML/RSS    │  REST + uploads       │  learning API
        │  deployed with site  │  (private API repo)   │  (private repo)
        └──────────────────────┴───────────────────────┘
                          nginx · Ubuntu production hosts
```

| Surface | Host | Responsibility |
|---------|------|----------------|
| Marketing + journal | `udalitatoo.ru` | Public content, SEO, RSS |
| Studio CRM | `app.udalitatoo.ru` | Admin / master / client operations |
| Learn | `learn.udalitatoo.ru` | Training admin + student experience |

## Key engineering work

- **Split public site and apps** — static marketing and journal on the apex domain; CRM and Learn on dedicated subdomains with their own PWA manifests and theme tokens
- **Journal SEO pipeline** — article pages with canonical URLs, structured metadata, RSS including Yandex full-text payloads, and cross-links from the main landing nav
- **Multi-role CRM** — `admin` / `master` / `client` route trees, optional multi-role users with in-app role switching, and separate dashboards per role
- **Operational depth in CRM** — scheduling, client records, portfolio, payments/expenses, site analytics, blog management, and Telegram integration surfaces (from production navigation)
- **Learn host isolation** — post-login redirect sends students to `learn.udalitatoo.ru` while admins manage learning, students, and invites on the Learn origin
- **Installable PWAs** — standalone display, icons, and theme colors for Buketov and Learn production builds
- **Portfolio index repo** — this public GitHub index documents the ecosystem for employers without exposing private application source

## Tech stack

| Layer | Technologies (evidenced in production) |
|-------|----------------------------------------|
| **Public site** | Static HTML/CSS, video hero, web manifest, journal static pages + RSS |
| **Buketov CRM** | React, Vite, PWA; TanStack Query; Axios; role-based SPA routing |
| **Learn** | React, Vite, `vite-plugin-pwa`, admin vs student route guards |
| **Edge / deploy** | nginx on Ubuntu (production response headers) |
| **API & data** | Private backend repos (request access); not shipped in this index |

## Repository layout

| Path | Role |
|------|------|
| `README.md` | Public employer-facing ecosystem index (this file) |
| `docs/screenshots/` | Portfolio captures of production landing and journal |

Runtime apps and services live in the module repositories listed below—not in this index repo.

## Local setup

There is no application code in **udalitatoo-ecosystem**. Clone the private module repos and follow each README / `.env.example`:

| Module | Repo |
|--------|------|
| Landing + journal | [udalitatoo](https://github.com/asydneysummer/udalitatoo) *(private — request access)* |
| CRM PWA | [buketov-app](https://github.com/asydneysummer/buketov-app) *(private — request access)* |
| Learn / blog CMS | [blog-cms](https://github.com/asydneysummer/blog-cms) *(private — request access)* |

Typical pattern: install dependencies and run the Vite dev server in **buketov-app** or **blog-cms**; serve or build static assets from **udalitatoo** per that repo’s docs. Do not commit real API keys, Telegram tokens, or payment credentials.

## Related repositories

### Udalitatoo modules

| Module | Repository | Production |
|--------|------------|------------|
| **Site** — landing, journal, RSS | [udalitatoo](https://github.com/asydneysummer/udalitatoo) *(private — request access)* | [udalitatoo.ru](https://udalitatoo.ru), [udalitatoo.ru/journal/](https://udalitatoo.ru/journal/) |
| **Buketov CRM** — studio PWA (admin / master / client) | [buketov-app](https://github.com/asydneysummer/buketov-app) *(private — request access)* | [app.udalitatoo.ru](https://app.udalitatoo.ru) |
| **Learn** — training admin & student cabinet | [blog-cms](https://github.com/asydneysummer/blog-cms) *(private — request access)* | [learn.udalitatoo.ru](https://learn.udalitatoo.ru) |

*Private portfolio index · request access to module source repos · client: Maxim Buketov / udalitatoo.ru · author: [asydneysummer](https://github.com/asydneysummer)*
