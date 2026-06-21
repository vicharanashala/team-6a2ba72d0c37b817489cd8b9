# 🧠 FAQ Crowdsource — Community-Driven Knowledge Platform

> A modern, AI-augmented crowdsourcing platform where communities collaboratively create, answer, vote on, and curate high-quality FAQs across any topic or domain.
---

## 👥 Team Members

This project was built as a group effort. A big thanks to everyone who contributed!

| Name | Role |
|------|------|
| _Yogya Mittal_ | _Team Lead & Backend Developer_ |
| _Piyush Sonkusare_ | _Frontend Developer_ |
| _Akhila Sai Ambadipudi_ | _UI/UX Engineer_ |
| _Swati Sahoo_ | _AI Lead_ |
| _Sanjib Ghara_ | _NLP Lead_ |
| _Ayush Anand_ | _Dataset Curator_ |
| _Archit Singh_ | _Feature Development Engineer_ |

---

## 📑 Table of Contents

- [Overview](#overview)
- [The Problem We Solve](#the-problem-we-solve)
- [Key Features](#key-features)
- [AI & NLP Capabilities](#ai--nlp-capabilities)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [API Reference](#api-reference)
- [User Roles](#user-roles)
- [Roadmap](#roadmap)
- [License](#license)

---

## Overview

FAQ Crowdsource democratizes knowledge by letting real users surface the questions that actually matter — not the ones companies *think* they should answer. Community voting ensures the best answers rise to the top, while an AI layer continuously improves quality, fills coverage gaps, and summarizes long-form answers into concise takeaways.

The platform is designed to scale across languages, domains, and regions, making it suitable for organizations, developer communities, customer support teams, and any group that benefits from a living, self-maintaining knowledge base.

---

## The Problem We Solve

Traditional FAQ pages suffer from four core issues:

- **Staleness** — rarely updated after initial creation
- **Bias** — written by internal teams who may not know what users actually ask
- **Incompleteness** — miss the edge-case questions real users encounter
- **Poor discoverability** — flat lists with no ranking or search intelligence

This platform solves all four by letting the crowd continuously submit, answer, refine, and vote on FAQs in real time — with AI stepping in to assist, not replace, human contributors.

---

## Key Features

### 🙋 Question & Answer System
- Submit questions with rich-text (Markdown) body, tags, and optional image attachments
- Multiple answers per question with community voting
- "Accepted Answer" marked by the question author or a verified expert
- "Verified Answer" badge for expert-endorsed responses
- Short-form / long-form answer toggle for quick reads or in-depth study

### 🔍 Search & Discovery
- **Semantic search** powered by NLP embeddings — understands intent, not just keywords
- Full-text search across questions, answers, and tags
- Filters by category, tag, status, and date range
- "Related Questions" sidebar and SEO-optimized public FAQ pages

### 🗳️ Voting & Ranking
- Upvote / downvote on questions and answers
- Hot/trending algorithm combining vote score and recency
- Expert-weighted feedback — votes from verified experts carry extra weight in the confidence score
- Anti-gaming measures: rate limits and vote-ring detection

### 👤 Reputation & Gamification
- Reputation point system rewarding quality contributions:
  - Ask a question: **+2** · Answer accepted: **+15** · Answer upvoted: **+10**
  - Question upvoted: **+5** · Answer downvoted: **−2**
- Reputation thresholds gate actions (e.g., 15 rep to upvote, 500 rep to edit others' posts)
- Badges: *First Answer*, *Helpful*, *Expert*, *Top Contributor*, and more
- Weekly / monthly / all-time leaderboards and daily contribution streaks

### 📊 Personalized Dashboard
- Phase-aware content feed adapting to the user's experience level (beginner → advanced)
- Recommended questions based on tags and past activity
- Quick-access shortcuts for unanswered questions in the user's expertise domain
- Weekly progress summary: questions asked, answers given, reputation earned

### 🔔 Notifications
- In-app and email notifications for new answers, upvotes, and accepted answers
- Digest emails (daily/weekly)
- Granular notification preferences per user

### 🛡️ Moderation & Quality
- NLP-based duplicate detection before question submission (similarity threshold > 0.85)
- Community flagging system → moderator review queue
- Confidence Meter beside each answer, calculated from votes, expert endorsements, and AI verification
- Low-quality answer auto-flagging (too short, no formatting)
- Multi-step verification: community vote → expert review → admin sign-off

### 🌐 Embeddable Widget & API
- Lightweight JS SDK (`<script>` tag embed) for any external website
- Full REST API with API key authentication for programmatic access
- Webhook integrations for Slack, Discord, and Microsoft Teams

### 🌍 Multilingual Support
- Auto-detects language of submitted content
- Cross-language search — find answers regardless of the language they were written in
- Machine translation layer with a clear "auto-translated" label
- RTL (right-to-left) layout support

---

## AI & NLP Capabilities

The platform integrates a dedicated AI/NLP layer that works alongside human contributors rather than replacing them.

### Semantic Search
Queries are encoded into dense vector embeddings and matched against indexed question/answer embeddings using kNN cosine similarity in Elasticsearch. Results are fused with standard BM25 text matching (RRF fusion) for the best of both worlds.

### Duplicate Detection
Before a question is saved, its title and body are checked against Elasticsearch via kNN vector search. If the similarity score exceeds **0.85**, the user is prompted to review existing questions instead of creating a duplicate.

### AI-Generated Summaries
Long-form answers are automatically summarized into concise takeaways. All AI-generated summaries are clearly labelled for full transparency.

### Tiered Answer Generation (3-Stage Fallback)
1. **Stage 1 — Automated FAQ**: Surface existing, relevant FAQs and next steps relevant to the user's phase automatically.
2. **Stage 2 — Human Answers**: Route open questions to contributors and verified experts.
3. **Stage 3 — AI Fallback**: If no human answer arrives within 24 hours, an AI-drafted answer (clearly labelled) is generated as a safety net.

### Hybrid Self-Improvement
- AI periodically reviews low-scoring answers (negative vote score) and proposes edits for clarity and formatting.
- AI scans common searches that returned no results and suggests new FAQ entries to fill coverage gaps.
- **All AI suggestions require human reviewer (Moderator/Admin) approval before going live.**

### Key Term Extraction
Named Entity Recognition (NER) auto-extracts key terms from question text and highlights them in search results for faster scanning.

### Voice Input
OpenAI Whisper transcribes spoken questions to text, which the user can review and edit before submitting.

### AI Technology Stack

| Component | Technology |
|-----------|-----------|
| Vector store & kNN search | Elasticsearch 8+ (dense_vector) |
| Sentence embeddings | `all-MiniLM-L6-v2` (self-hosted or HuggingFace) |
| LLM (summaries, answers, self-improvement) | OpenAI GPT-4o / Anthropic Claude |
| Speech-to-text | OpenAI Whisper API |
| Translation | Google Cloud Translate / DeepL API |

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | Next.js 14 (App Router), React, custom CSS design system (glassmorphism) |
| **Backend API** | Fastify, Prisma ORM, TypeScript |
| **Database** | Supabase (PostgreSQL) |
| **Search** | Elasticsearch 8+ (full-text + semantic kNN) |
| **Cache & Sessions** | Redis |
| **Job Queue** | BullMQ |
| **Object Storage** | S3 / MinIO (images and media) |
| **Embeddable Widget** | Vite + React (compiles to a single JS file) |
| **Monorepo Tooling** | Turborepo, npm workspaces |
| **Auth** | Supabase Auth / NextAuth (JWT with 15-min access tokens, 7-day refresh) |
| **Real-time** | Socket.io + Redis Pub/Sub |

---

## Architecture

The platform follows a **modular monolith** design for the MVP (Phases 1–2), with clean service boundaries that allow extraction into a **service-oriented architecture** in later phases.

```
┌──────────────────────────────────────────────────────┐
│                     CLIENT LAYER                     │
│  Web App (Next.js) · Mobile PWA · Widget · Admin     │
└─────────────────────────┬────────────────────────────┘
                          │
┌─────────────────────────▼────────────────────────────┐
│           API GATEWAY (Fastify)                      │
│  Rate Limiting · JWT Auth · CORS · Request Routing   │
│  REST API · WebSocket (Socket.io) · Webhooks         │
└───────────┬──────────────┬───────────────┬───────────┘
            │              │               │
    ┌───────▼───────┐ ┌───▼────────┐ ┌───▼──────────┐
    │ CORE SERVICES │ │ AI LAYER   │ │PLATFORM SVCS │
    │               │ │            │ │              │
    │ Question Svc  │ │ Semantic   │ │ Auth & Users │
    │ Answer Svc    │ │ Search     │ │ Notifications│
    │ Voting Svc    │ │ Duplicate  │ │ Moderation   │
    │ Tag Svc       │ │ Detection  │ │ Analytics    │
    │ Search Svc    │ │ Summary    │ │ Reputation   │
    │ Dashboard Svc │ │ Engine     │ │ Webhooks     │
    └───────┬───────┘ │ Tiered Gen │ │ Translation  │
            │         │ Self-Impr. │ │ Reports      │
            │         └─────┬──────┘ └──────┬───────┘
            └───────────────┼───────────────┘
                            │
┌───────────────────────────▼──────────────────────────┐
│                     DATA LAYER                       │
│  PostgreSQL · Elasticsearch · Redis · S3/MinIO       │
└──────────────────────────────────────────────────────┘
```

**Deployment targets:** Vercel Edge (CDN + SSR cache) → Application Load Balancer → auto-scaling API/WebSocket servers → managed data tier (PostgreSQL with read replicas, 3-node Elasticsearch cluster, Redis cluster).

**Performance targets:**
- Page load: < 2 seconds
- Search results: < 500 ms
- API response (p95): < 200 ms
- Concurrent users: 10,000+

---

## Project Structure

```
faq-crowdsource/
├── apps/
│   ├── web/               # Next.js 14 frontend (user-facing + admin panel)
│   └── widget/            # Embeddable JS widget (Vite + React → single bundle)
├── packages/
│   ├── api/               # Fastify backend REST API + Prisma
│   ├── db/                # Prisma schema & migrations
│   ├── ai/                # AI service (embeddings, LLM calls, STT)
│   ├── queue/             # BullMQ job definitions & workers
│   ├── shared/            # Shared TypeScript types, utils, constants
│   └── config/            # Environment configs
├── turbo.json             # Turborepo task pipeline
├── package.json           # Monorepo root
└── docker-compose.yml     # Local dev services (PG + ES + Redis + MinIO)
```

### Frontend Pages (14+ built)

| Page | Route |
|------|-------|
| Discovery Feed | `/` |
| Question Details | `/question/[id]` |
| Admin Dashboard | `/admin` |
| User Dashboard | `/dashboard` |
| Leaderboard | `/leaderboard` |
| Topics | `/topics` |
| Experts Directory | `/experts` |
| User Profile | `/user/[id]` |
| Settings | `/settings` |
| Notifications | `/notifications` |
| Sign In / Sign Up | `/signin`, `/signup` |
| Privacy Policy & ToS | `/privacy`, `/terms` |

### Backend API Routes

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/questions` | List / search questions |
| `POST` | `/questions` | Submit a new question |
| `GET` | `/questions/:id` | Get question details |
| `POST` | `/questions/:id/answers` | Submit an answer |
| `POST` | `/questions/:id/vote` | Vote on a question |
| `GET` | `/search` | Full-text + semantic search |
| `GET` | `/tags` | List all tags |
| `GET` | `/users/:id` | Public user profile |
| `GET` | `/analytics` | Platform analytics (admin) |
| `POST` | `/answers/:id/summarize` | Trigger AI summary for an answer |
| `POST` | `/jobs/analyze-low-scoring` | AI self-improvement job |
| `POST` | `/jobs/analyze-gaps` | AI gap-fill job |
| `GET` | `/suggestions` | List pending AI suggestions |
| `PUT` | `/suggestions/:id/approve` | Approve an AI suggestion (moderator) |

---

## Getting Started

### Prerequisites

- Node.js 20+
- npm 10.5+
- A [Supabase](https://supabase.com) project (for PostgreSQL)
- (Optional for full AI features) OpenAI API key, Elasticsearch 8+ instance

### Installation

```bash
# Clone the repository
git clone https://github.com/<your-org>/faq-crowdsource.git
cd faq-crowdsource

# Install all dependencies across the monorepo
npm install
```

### Environment Setup

Create a `.env` file inside `packages/api/`:

```env
DATABASE_URL="postgresql://..."        # Supabase connection string (pooled)
DIRECT_URL="postgresql://..."          # Supabase direct connection string
OPENAI_API_KEY="sk-..."               # OpenAI (GPT-4o + Whisper)
ANTHROPIC_API_KEY="sk-ant-..."        # Anthropic Claude (optional fallback)
ELASTICSEARCH_URL="http://..."        # Elasticsearch instance
REDIS_URL="redis://..."               # Redis instance
```

### Running the App

```bash
# Start all apps simultaneously (Next.js + Fastify) via Turborepo
npm run dev
```

- **Frontend:** [http://localhost:3000](http://localhost:3000)
- **Backend API:** [http://127.0.0.1:3001](http://127.0.0.1:3001)

Turborepo handles dependency ordering automatically — the API will be ready before the frontend attempts to connect.

---

## User Roles

| Role | Description | Key Permissions |
|------|-------------|-----------------|
| **Visitor** | Unauthenticated user | Browse & search FAQs |
| **Contributor** | Registered user | Submit questions & answers, vote, comment |
| **Expert** | Verified domain expert | Mark verified answers, edit others' answers |
| **Moderator** | Community moderator | Merge duplicates, flag/remove content, manage tags |
| **Admin** | Platform operator | Full access: user management, analytics, settings |
| **API Consumer** | External system | Submit via API, retrieve curated FAQ sets |

---

## Roadmap

### ✅ Phase 1 — MVP (Complete)
Question & answer CRUD · Voting & ranking · Basic search & tags · User auth & profiles · Basic moderation tools · Full UI overhaul (14+ pages, glassmorphism design system)

### 🔄 Phase 2 — Quality & Engagement
Reputation system & badges · Duplicate detection (NLP) · Review queues · Email + in-app notifications · Admin analytics dashboard

### 🔌 Phase 3 — Scale & Integrate
REST API with API keys · Embeddable JS widget (fully functional) · Webhook integrations (Slack, Discord, Teams) · Bounty system · Advanced search filters

### 🤖 Phase 4 — Intelligence
AI-generated summaries & tiered answer generation · Best answer isolation · Hybrid AI self-improvement · Voice input · Semantic search (NLP/embeddings) · Personalized phase-aware dashboard · Key term & point identification · Confidence meter & expert-weighted feedback · Multilingual support & RTL layouts · Auto-tagging with ML · Sentiment analysis · White-label / multi-tenant mode

---

## Contributing

1. Fork the repo and create a feature branch: `git checkout -b feature/your-feature`
2. Make your changes and commit: `git commit -m 'feat: add your feature'`
3. Push to your fork: `git push origin feature/your-feature`
4. Open a Pull Request describing your changes

Please follow the existing code style and ensure your changes don't break the Turborepo build pipeline (`npm run build`).

---

<p align="center">Built by the FAQ Crowdsource team · © 2026 vicharanashala</p>

---

# team-6a2ba72d0c37b817489cd8b9
FAQ Crowdsourcing project — Yogya Mittal
