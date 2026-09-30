# Mayson Veyre — Digital Atelier

<p align="center">
  <strong>A private commercial and operational platform built for Mayson Veyre Digital Atelier.</strong>
</p>

<p align="center">
  <em>Precision in the interface. Structure in the operation.</em>
</p>

<p align="center">
  <img alt="Next.js" src="https://img.shields.io/badge/Next.js-16.3.5-black?logo=next.js" />
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white" />
  <img alt="Prisma" src="https://img.shields.io/badge/Prisma-7.10.0-2D3748?logo=prisma&logoColor=white" />
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-Supabase-4169E1?logo=postgresql&logoColor=white" />
  <img alt="Stripe" src="https://img.shields.io/badge/Stripe-payments-635BFF?logo=stripe&logoColor=white" />
</p>

---

## Overview

**Mayson Veyre** is a full-stack internal platform designed to support the commercial and operational workflow of the **Mayson Veyre Digital Atelier Paris**.

Instead of treating the product as a generic admin dashboard, the system was designed around the idea of a **digital private office**: focused, restrained, highly structured and responsible for the complete journey from lead acquisition to proposal, payment and project execution.

```text
LEAD
  ↓
CLIENT
  ↓
MEETING
  ↓
PROPOSAL
  ↓
ACCEPTANCE / PAYMENT
  ↓
PROJECT
  ↓
DELIVERY
```

The application brings these stages together in one authenticated environment.

---

## Product

The platform is organized around the operational needs of a digital atelier.

| Area | Purpose |
|---|---|
| **Office / Dashboard** | Operational overview and quick access to the workspace |
| **Leads** | Capture, qualification and commercial pipeline management |
| **Clients** | Centralized relationship and customer information |
| **Meetings** | Commercial and client meeting management |
| **Proposals** | Proposal creation, delivery and lifecycle tracking |
| **Projects** | Execution tracking, statuses, dates, tasks and ownership |
| **Notifications** | Internal operational alerts and preferences |
| **Users** | Controlled team access and role administration |
| **Settings** | Account and application preferences |
| **Audit** | Administrative activity tracking |

---

## Proposal workflow

One of the core features is the proposal lifecycle.

```text
DRAFT
  ↓
SENT
  ↓
VIEWED
  ↓
ACCEPTED
```

A proposal can also move through decline or expiration paths depending on the state of the negotiation.

The workflow supports:

- proposal creation and editing
- client and lead relationships
- public proposal access
- proposal view tracking
- acceptance / decline actions
- payment integration
- connection between commercial and project workflows

### Public proposal

Customers can access a proposal through a public tokenized route:

```text
/proposal/[token]
```

This keeps the client-facing experience separated from the authenticated Private Office.

---

## Payments

The platform integrates with **Stripe** for payment processing and webhook-driven updates.

The intended flow is:

```text
PROPOSAL
   ↓
ACCEPTANCE
   ↓
CHECKOUT
   ↓
STRIPE
   ↓
WEBHOOK
   ↓
DATABASE / OFFICE
```

Stripe events are handled by backend webhook routes so payment state can be synchronized with the application.

---

## Access control

The current internal access model is intentionally small and explicit.

```text
ADMIN
COMMERCIAL
```

### `ADMIN`

Administrative access to the private workspace, user management and protected operational resources.

### `COMMERCIAL`

Commercial access focused on leads, clients, meetings, proposals and related workflows according to application permissions.

Authorization is enforced at the API layer as well as in the interface, so protected operations do not depend only on frontend visibility.

---

## Security architecture

Security is treated as part of the application architecture rather than as a frontend-only concern.

The project includes mechanisms such as:

- session-based authentication
- permission-aware authorization
- CSRF protection for protected operations
- rate limiting
- audit logging
- protected API routes
- separation between private workspace and public proposal routes
- environment-based secret management

The internal session cookie is handled by the application as `mayson_session`.

Sensitive credentials are excluded from version control and are expected to be supplied through deployment environment variables.

---

## Technology stack

### Frontend

- **Next.js 16.3.5**
- **React**
- **TypeScript**
- **Tailwind CSS**
- **Framer Motion**
- **Lucide React**
- **shadcn-style / Radix-based UI patterns**

### Backend

- **Next.js App Router**
- **Route Handlers**
- REST-style internal APIs
- Custom authentication and authorization layers
- Webhook processing

### Data

- **PostgreSQL**
- **Supabase**
- **Prisma ORM 7.10.0**

### Payments

- **Stripe**

### Engineering

- TypeScript strict mode
- ESLint
- Vitest
- Production build validation
- Git / GitHub
- Vercel-ready deployment architecture

---

## Architecture

At a high level, the application follows this structure:

```text
                         ┌─────────────────────┐
                         │      Next.js App    │
                         │   App Router / UI   │
                         └──────────┬──────────┘
                                    │
                 ┌──────────────────┼──────────────────┐
                 │                  │                  │
                 ▼                  ▼                  ▼
           React Interface      Route Handlers   Auth / Security
                 │                  │                  │
                 └──────────────────┼──────────────────┘
                                    │
                                    ▼
                              Prisma ORM
                                    │
                                    ▼
                           PostgreSQL / Supabase
                                    │
                                    ├───────────────┐
                                    │               │
                                    ▼               ▼
                                 Stripe         Webhooks
```

The goal is to keep the business workflow, interface, persistence and security model connected instead of building isolated screens.

---

## Project structure

```text
my-app/
├── app/
│   ├── api/
│   │   ├── auth/
│   │   ├── clients/
│   │   ├── leads/
│   │   ├── meetings/
│   │   ├── notifications/
│   │   ├── projects/
│   │   ├── proposals/
│   │   ├── settings/
│   │   ├── stripe/
│   │   └── users/
│   │
│   ├── login/
│   ├── office/
│   │   ├── audit/
│   │   ├── clients/
│   │   ├── leads/
│   │   ├── meetings/
│   │   ├── projects/
│   │   ├── proposals/
│   │   ├── settings/
│   │   └── users/
│   │
│   └── proposal/
│       └── [token]/
│
├── components/
│   └── office/
│
├── lib/
│   ├── auth/
│   └── security/
│
├── prisma/
│   └── schema.prisma
│
├── public/
├── scripts/
├── prisma.config.ts
├── next.config.ts
├── package.json
└── tsconfig.json
```

---

## Local development

Clone the repository and install dependencies:

```bash
git clone https://github.com/saantanadeev/mayson-veyre-digital-atelier.git
cd mayson-veyre-digital-atelier
npm install
```

Create a local `.env` file with the required application variables.

Generate the Prisma client:

```bash
npx prisma generate
```

Start the development server:

```bash
npm run dev
```

Then open:

```text
http://localhost:3000
```

---

## Environment variables

Production secrets should never be committed to Git.

The project expects configuration to be supplied through environment variables. Typical production variables include:

```env
DATABASE_URL=
DIRECT_URL=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
NEXT_PUBLIC_APP_URL=
```

Use real values only in your local environment or deployment provider settings.

**Never publish secret values in GitHub issues, README files, screenshots or documentation.**

---

## Production deployment

The deployment architecture is designed around:

```text
GitHub
   ↓
Vercel
   ↓
Next.js
   ↓
Prisma
   ↓
Supabase PostgreSQL
```

The project has been validated with a production build using:

```bash
npm run build
```

The build must complete successfully before a release is deployed.

---

## Design direction

The visual language follows the identity of **Mayson Veyre Digital Atelier**:

- deep black surfaces
- restrained gold accents
- editorial typography
- minimal borders
- generous spacing
- glass and transparency effects
- subtle motion
- micro-interactions
- quiet, premium visual hierarchy

The product intentionally avoids the standard appearance of a conventional SaaS dashboard.

The interface is meant to feel closer to a **private French atelier** than to a generic business admin panel.

> **The technology stays behind the scenes. Precision appears in the experience.**

---

## Engineering principles

The project is built around a few practical principles:

**Product over screens** — a feature is only complete when its interface, business rules, API and persistence work together.

**Security at the boundary** — protected behavior is enforced on the server, not only hidden in the UI.

**Consistency across the workflow** — leads, clients, meetings, proposals, payments and projects should form one operational system.

**Deliberate interface design** — motion, spacing, typography and interaction states are part of the product rather than decoration.

**Production awareness** — the application is structured for real deployment, environment isolation and external integrations.

---

## Repository status

This repository contains the public source and technical documentation for the Mayson Veyre project.

The platform is an evolving product and may change as new operational requirements are introduced.

---

## License

Copyright © 2026 Caique Santana / Mayson Veyre Digital Atelier.

All rights reserved.

The repository is publicly accessible for inspection, portfolio and educational review. Public visibility does **not** grant permission to copy, redistribute, resell or publish modified versions of the software without prior written authorization.

---

## Mayson Veyre

**Mayson Veyre Digital Atelier Paris**

Private systems. Commercial precision. Digital craftsmanship.

<p align="center">
  <sub>Built with Next.js, TypeScript, Prisma, Supabase and Stripe.</sub>
</p>
