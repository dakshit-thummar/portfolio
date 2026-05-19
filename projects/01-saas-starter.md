# SaaS Starter — Multi-Tenant Django Boilerplate

> A production-ready foundation for any subscription-based SaaS product. Multi-tenant organizations, role-based memberships, JWT auth, Stripe billing — all wired up.

🔒 *Source code is in a private repository. Available on request.*

---

## The problem

Most early-stage SaaS founders waste 3–4 weeks rebuilding the same boilerplate before they get to actual product work: user signup, organizations / teams, invitations, billing, webhooks, settings pages. This project is that 3–4 weeks of work, done once, packaged up, and ready to extend with whatever your product actually does.

## What it does

- **Email-based signup and login** with a custom Django user model, password validation, and JWT tokens (access + refresh)
- **Multi-tenant organizations** — every user belongs to one or more orgs; data is scoped per-org via querysets
- **Role-based permissions** — Owner, Admin, Member; only Owners can subscribe / invite Admins
- **Member invitations** by email with role assignment
- **Stripe checkout sessions** for subscription start; **webhook handler** for subscription lifecycle events (active, past_due, cancelled)
- **Branded landing page**, signup form, login form, dashboard with org stats
- **Browsable REST API** with DRF, ready for a separate frontend if needed

## Tech stack

| Layer | Tools |
|---|---|
| Backend | Django 5, Django REST Framework, simplejwt |
| Auth | Custom email-based user model, JWT access + refresh, password validation |
| Billing | Stripe Python SDK, checkout sessions, signature-verified webhooks |
| Frontend | Bootstrap 5 + Bootstrap Icons (CDN), gradient hero, sticky nav |
| Database | SQLite for dev; Postgres-ready (env-driven) |
| DevOps | Dockerfile + docker-compose with Postgres service, GitHub Actions CI |

## Architecture in one paragraph

The `accounts` app owns users + auth (custom user manager, email field as USERNAME_FIELD). The `organizations` app owns the multi-tenant model: an `Organization` row plus a `Membership` join table with a role choice; querysets in views filter by `memberships__user=request.user` so users only ever see data scoped to their orgs. The `billing` app owns Stripe integration: a `Subscription` model linked one-to-one with Organization, a checkout endpoint that creates a Stripe session with the org_id in metadata, and a webhook endpoint that verifies signatures and updates Subscription state on `checkout.session.completed` / `customer.subscription.updated` / `customer.subscription.deleted`.

## Skills demonstrated

- Custom user models done correctly (replacing username with email, custom manager, REQUIRED_FIELDS)
- DRF generic views + permission classes + JWT authentication
- Multi-tenant data scoping (via querysets, not middleware — simpler and easier to test)
- Inline formsets for invitations + member management
- Stripe integration end-to-end including signature-verified webhooks
- Environment-driven settings via `python-dotenv`
- Branded auth pages built on top of DRF endpoints (HTML form posts to JSON API, stores tokens in `localStorage`)

## What I deliver to a client

Full source code transferred to the client's GitHub org, `.env.example`, deployment guide for Railway / Render / Fly / VPS, Stripe setup walkthrough (test mode → live mode), seed command for demo data, and 30-day support for bug fixes.

## Variations I've built on top of this

- A two-sided marketplace (sellers + buyers as different memberships)
- Per-seat pricing instead of flat-rate subscription
- Email-confirmation-required signup with magic-link login
- SSO via Google / Microsoft for enterprise customers
- API-key issuance for headless / programmatic access

## Lessons / opinions

Multi-tenancy on Django is best done with row-level queryset scoping unless you genuinely have noisy-neighbor problems — schema-per-tenant adds operational complexity that small teams can't afford. JWT in cookies is more secure than localStorage, but localStorage is simpler if your frontend is a single trusted app. Stripe webhooks must be idempotent — same event ID can arrive twice; handle gracefully.

---

← [Back to portfolio](../README.md)
