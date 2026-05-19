# Daksh — Freelance Python / Django Portfolio

> Backend & full-stack developer · 3+ years building production Django applications · Rajkot, Gujarat

I build Django web applications end-to-end: data modelling, REST APIs, real-time features, payment integrations, computer vision, web scraping, and the polished admin UIs that hold them together.

This portfolio walks through six self-contained projects that demonstrate the work I do for freelance clients. **Source code is in private repos** — I share access on request after a brief NDA / scope conversation, or I rebuild the same patterns inside your codebase if you'd rather start clean.

**Available for:** part-time freelance contracts (15–25 hrs/week), short fixed-scope builds, code reviews, and architecture consults.

📧 **gmail.com** · 📍 Rajkot, Gujarat · ⏱️ Available immediately

---

## The seven projects

Each project has its own deep-dive write-up under [`projects/`](./projects/). The titles below link to the detailed page.

| # | Project | What it solves | Tech demonstrated |
|---|---|---|---|
| 1 | [**SaaS Starter**](./projects/01-saas-starter.md) | Multi-tenant SaaS boilerplate with billing | Django · DRF · JWT · Stripe · Docker |
| 2 | [**Face Recognition Attendance**](./projects/02-face-attendance.md) | Office / school attendance via webcam | Django · OpenCV · face_recognition · NumPy |
| 3 | [**Scraping Dashboard**](./projects/03-scraping-dashboard.md) | Scheduled web scraping with analytics | Django · Celery · Redis · BeautifulSoup |
| 4 | [**Booking Platform**](./projects/04-booking-system.md) | Online appointment booking SaaS | Django · iCal · async scheduling logic |
| 5 | [**Invoice & Billing**](./projects/05-invoice-system.md) | Freelancer invoicing with PDF + email | Django · ReportLab · SMTP |
| 6 | [**Real-Time Chat**](./projects/06-realtime-chat.md) | Multi-room chat over WebSockets | Django Channels · Daphne · Redis |
| 7 | [**Signal System**](./projects/07-signal-system.md) | AI trading signal generator + market scanner | React · Vite · Tailwind · Anthropic / OpenAI / Perplexity / Gemini |

Combined: ~136 Python files plus ~35 KB of React/JSX, ~3,400 lines of Python, ~2,400 lines of templates, 30+ models, 95+ views, 70+ HTML pages, 7 GitHub Actions CI workflows, 7 Dockerized environments, MIT-licensed.

---

## What I'm good at

See the [skills matrix](./SKILLS.md) for the full list, but in short:

**Backend** — Django (5+ years exposure, 3+ pro), Django REST Framework, FastAPI, Flask, custom user models, JWT auth, multi-tenancy, role-based permissions, complex form/formset workflows, signal-based side effects, query optimization, database design (PostgreSQL, MySQL, MongoDB).

**Async & real-time** — Celery + Redis for background jobs and scheduled tasks, Django Channels + Daphne for WebSockets and group broadcasts, async consumers, message queues.

**Integrations** — Stripe (checkout sessions + webhooks + subscription lifecycle), email delivery (SMTP, Mailgun-ready), SMS hooks (Twilio-ready), OAuth, third-party REST APIs, calendar (.ics), PDF generation.

**Computer vision & ML** — OpenCV, face recognition (`face_recognition` / dlib), webcam frame processing, image encoding and matching, basic object detection.

**AI / LLM integration** — Multi-provider LLM workflows (Anthropic Claude, OpenAI Responses API, Perplexity Sonar, Google Gemini) with built-in web search, prompt engineering as code, structured output parsing, server-side key handling so secrets never reach the browser. See [Signal System](./projects/07-signal-system.md).

**Web scraping & automation** — BeautifulSoup, Scrapy, Selenium, [Make.com](https://make.com) for no-code automations, JSON-driven selector engines, anti-detection patterns.

**Frontend** — Bootstrap 5, vanilla JS, Chart.js, HTMX-friendly markup, React 18 + Vite + Tailwind CSS, responsive design, accessibility basics. Not a designer, but I ship UIs that are clean and don't embarrass the work.

**Infrastructure** — Docker + docker-compose, GitHub Actions CI, environment-based config, Postgres tuning, deployment to Railway / Render / Fly.io / VPS, Nginx reverse-proxy, Let's Encrypt TLS.

---

## Recent freelance experience

**Python Developer — Digipie Technologies** (Surat) · Mar 2025 – Present
Backend services with FastAPI, REST API design with validation and performance tuning, frontend integration, production debugging, deployments.

**Python Developer — Coretus Technologies** (Rajkot) · Sep 2023 – Aug 2024
Real-time face recognition system with Flask + OpenCV, integration-heavy backend APIs, database optimization.

**Python Developer — Vivansh Infotech LLP** (Ahmedabad) · Jan 2023 – Aug 2023
Backend with Flask + Django, REST APIs, structured data processing, database systems, frontend integration.

---

## How I work with clients

**Discovery (free, ~30 min).** We talk through what you actually need versus what you think you need. I ask about deadlines, budget constraints, integrations, and who else is on the team.

**Quote.** Fixed-price for well-defined scopes (e.g. "build me a booking site"); hourly for ongoing or research-heavy work. Indian-market rates for India-based clients, USD-market rates for international clients — happy to share specifics on request.

**Build.** I work in 1–2 week iterations with a working demo at the end of each. You see progress weekly, not just at the deadline. I use feature branches and PR-style review even on solo projects.

**Handoff.** Source code (private repo, transferred to your org / GitHub), `.env.example`, README with setup steps, deploy guide, and a 30-day support window for bug fixes baked in.

---

## Why my work stands out from the crowd on Upwork / Fiverr

- **Tests on every project** — most freelancers skip them. CI badges on every repo prove they run green.
- **Docker on every project** — clients can `docker compose up` and see the work without installing Python or fighting versions.
- **Seed data** — every project has `python manage.py seed` so the demo populates immediately. No empty-database confusion.
- **Real READMEs** — sitemap of routes, demo logins, configuration, deployment notes. Not just "install requirements and run".
- **Clean commit history** — small, descriptive commits, not "WIP" and "fixed stuff".
- **Production-grade defaults** — env-based config, password validation, CORS where needed, custom user model, no hard-coded secrets.

---

## Engagement options

| Option | Best for | What you get |
|---|---|---|
| **Fixed-scope build** | "I need a booking site / invoice tool / chat app" | Full build, code, tests, deploy. Fixed price. |
| **Part-time retainer** | Ongoing dev work or maintenance on existing Django app | 15–25 hrs/week, predictable hourly rate, monthly invoicing |
| **Code review** | "Audit my Django app before I ship" | Detailed PR-style review with security + performance + maintainability flags |
| **Architecture consult** | Pre-build planning | 1–3 hours of design thinking, deliverable is a written plan + tradeoffs doc |

---

## Get in touch

📧 **gmail.com**
📱 Available on WhatsApp
💻 Source repos shared on request after a brief intro call
🧠 Free 30-minute discovery call if you want to talk through your project

> *"The best time to start a project was last week. The second-best time is right now."*
