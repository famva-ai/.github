<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="profile/assets/logo-light.png">
  <source media="(prefers-color-scheme: light)" srcset="profile/assets/logo-dark.png">
  <img alt="Famva" src="profile/assets/logo-dark.png" width="260">
</picture>

<h3>Wellbeing Beyond Borders</h3>

<p>
AI-powered wellness intelligence for families separated by distance, not love.<br/>
We help the diaspora look after aging parents back home — before a small problem becomes a crisis.
</p>

<p>
  <a href="https://famva.co.uk"><img alt="Website" src="https://img.shields.io/badge/website-famva.co.uk-FF2E83?style=flat-square"></a>
  <img alt="Status" src="https://img.shields.io/badge/status-private%20beta-2B0F4F?style=flat-square">
  <img alt="License" src="https://img.shields.io/badge/license-proprietary-lightgrey?style=flat-square">
  <a href="mailto:hello@famva.com"><img alt="Contact" src="https://img.shields.io/badge/contact-hello%40famva.com-2B0F4F?style=flat-square"></a>
</p>

</div>

---

## About Famva

Famva is a remote elderly-care platform built for the diaspora, starting with UK-based families who have parents in Nigeria. A **Sponsor** (the adult child abroad) links to their parent's profile and gets AI-generated care plans, meal plans, and exercise routines tailored to the elderly person's health conditions, mobility, and local context, plus early-warning alerts when day-to-day patterns start to slip.

The goal isn't to replace visits home. It's to close the gap in between — so a decline in health or mood gets noticed in days, not months.

**What the platform does:**

- **AI care plans** — daily task lists (medication, hydration, exercise, wellness) generated from the elderly person's profile, conditions, and routine.
- **AI meal plans** — 7-day Nigerian-cuisine meal plans that respect medical conditions, food preferences, and intolerances, with automatic nutritional flagging (e.g. low-sodium guidance for hypertension).
- **AI exercise routines** — daily, mobility-appropriate exercise sets, from standing routines to fully seated ones.
- **Silent Decline Alerts** — a statistical monitoring engine that watches task completion, mood, step count, and sleep against a rolling baseline and proactively alerts the sponsor when something looks off.
- **Sponsor + admin tooling** — subscription billing, notifications, and an internal dashboard for operating the platform.

## Our Repositories

| Repository | Description | Primary Stack |
|---|---|---|
| [`backend`](https://github.com/famva-ai/backend) | Core REST API — auth, profiles, AI orchestration, health metrics, alerts, billing, notifications | Node.js · TypeScript · Express · PostgreSQL · Redis |
| [`web`](https://github.com/famva-ai/web) | Marketing site and admin dashboard | Next.js · React · TypeScript · Tailwind CSS |
| [`mobile-app`](https://github.com/famva-ai/mobile-app) | Cross-platform app for sponsors and elderly users | Flutter · Dart |
| [`.github`](https://github.com/famva-ai/.github) | Organisation-wide health files and this profile | — |

> These repositories are private. If you need access — investors, partners, contractors — reach out at [hello@famva.com](mailto:hello@famva.com).

## How It Fits Together

```mermaid
flowchart LR
    subgraph Clients
        MOB[Mobile App<br/>Flutter]
        WEB[Web<br/>Next.js — marketing + admin]
    end

    API[Backend API<br/>Node.js / Express]
    WORKER[Background Workers<br/>Bull queues]

    subgraph Data
        PG[(PostgreSQL)]
        REDIS[(Redis)]
    end

    subgraph AI["AI Orchestration"]
        AI1[DeepSeek]
        AI2[Gemini]
        AI3[OpenAI]
    end

    subgraph THIRDPARTY["Third-Party"]
        STRIPE[Stripe]
        TWILIO[Twilio SMS]
        FCM[Firebase Push]
        S3[AWS S3]
        CMS[Contentful]
    end

    MOB --> API
    WEB --> API
    API --> PG
    API --> REDIS
    API --> WORKER
    WORKER --> REDIS
    API --> AI
    API --> STRIPE
    WORKER --> TWILIO
    WORKER --> FCM
    API --> S3
    WEB --> CMS
```

The backend runs a **multi-provider AI fallback chain** (DeepSeek → Gemini → OpenAI) so care plan, meal plan, and exercise generation keeps working even if a provider is degraded — with hardcoded safe defaults as a last resort. Health alerting itself is a deterministic, rule-based engine rather than an LLM, so it stays predictable and explainable.

## Tech Stack

<div align="center">

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=flat-square&logo=express&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)
![Flutter](https://img.shields.io/badge/Flutter-02569B?style=flat-square&logo=flutter&logoColor=white)
![Dart](https://img.shields.io/badge/Dart-0175C2?style=flat-square&logo=dart&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-635BFF?style=flat-square&logo=stripe&logoColor=white)

</div>

| Layer | Highlights |
|---|---|
| **Backend** | Node.js, TypeScript, Express, Sequelize/PostgreSQL, Redis + Bull job queues, JWT auth, Swagger/OpenAPI docs |
| **Web** | Next.js, React, Tailwind CSS, shadcn/ui, Contentful (blog/CMS), Vercel Analytics |
| **Mobile** | Flutter, MVVM architecture (Provider + get_it + auto_route) |
| **AI** | DeepSeek, Google Gemini, OpenAI — orchestrated with provider fallback and schema-enforced structured output |
| **Infrastructure** | Docker Compose, Nginx + Let's Encrypt, GitHub Actions |
| **Integrations** | Stripe (billing), Twilio (SMS/OTP), Firebase (push), AWS S3 (file storage) |

## Security & Privacy

Elderly users' health data is sensitive by nature, so it's treated that way throughout the stack:

- Health-sensitive fields (diagnoses, medications, sleep patterns, etc.) are **encrypted at rest** and never stored in plaintext.
- All traffic is served over **HTTPS/TLS**.
- Access is **role-based** (sponsor / elderly / admin), with JWT-based auth and full **audit logging** on admin actions.
- Every AI-generated recommendation has a **deterministic fallback**, so the product degrades gracefully rather than failing silently.

Found a security concern? Please report it privately to [hello@famva.com](mailto:hello@famva.com) rather than opening a public issue.

## Get in Touch

- **Website:** [famva.co.uk](https://famva.co.uk)
- **Email:** [hello@famva.com](mailto:hello@famva.com)
- **Company:** CareBridgeAI LTD

<div align="center">
<sub>Built with care for families everywhere.</sub>
</div>
