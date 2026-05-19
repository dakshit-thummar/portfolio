# Online Booking & Appointment Platform

> A market-ready three-portal booking SaaS: customers book without an account, staff manage everything from a dashboard, providers manage their own profile + schedule from a separate self-service portal.

🔒 *Source code is in a private repository. Available on request.*

---

## The problem

Service businesses (clinics, salons, gyms, tutors, consultants) lose appointments every day to phone tag. Booking systems exist (Calendly, SetMore) but cost $25–50/month per provider and don't give the business any branding, customer database, or operational control. A self-hosted booking platform pays for itself within months and becomes part of the business's IP.

This is my deepest project — by far. It's the strongest portfolio piece for clients evaluating my Django depth.

## What it does

### For customers (no account needed)
- Browse a polished service catalog grouped by category (Salon, Wellness, Health, Tutoring, etc.)
- Pick a service → see provider photos and bios → choose a time slot
- Confirmation email with a `.ics` calendar invite attached (works in Gmail, Outlook, Apple Calendar)
- Find a booking later by entering booking code + email — then **cancel or reschedule themselves** within the configured window

### For staff (login required)
- Dashboard: today / this week / customers / completed counts, MTD revenue, outstanding payment count, 14-day chart, top services this month, upcoming list
- Today view: chronological list of today's appointments with check-in actions
- Week view: 7-column calendar grid, navigable forwards / backwards
- All bookings: searchable + filterable + paginated, with code / customer / service / status / payment search
- Booking detail: status workflow controls, payment status controls, internal staff notes (saved separately from customer notes)
- Customer database: search by name / email / phone, click into a customer profile to see all their booking history + total spend
- Walk-in form: create bookings on the spot for phone-in / in-person customers (bypasses slot validation)
- CSV export

### For service providers (separate login portal)
- Their own scoped login at `/provider/login/`
- Dashboard: KPIs scoped to me only, my today's schedule, my 14-day chart
- Profile editor: name, email, bio, photo URL — these are what customers see when booking me
- Working hours: inline formset to manage 7-day-a-week schedule
- Time off: add/remove vacation or blocked windows
- My bookings: search + filter only my own bookings
- Booking detail: mark completed / no-show, save my own internal notes (payment status remains admin-only)

## Tech stack

| Layer | Tools |
|---|---|
| Backend | Django 5 with 8 models (BusinessSettings, ServiceCategory, Service, Provider, WorkingHours, TimeOff, Customer, Booking) |
| Auth | Django auth with custom `provider_required` decorator that exposes `request.provider` |
| Calendar | iCalendar (.ics) generation with no external dependencies |
| Frontend | Bootstrap 5, Bootstrap Icons, Chart.js, three different colour themes (pink for customer-facing + admin, dark emerald sidebar for provider portal) |
| Database | SQLite with auto-generated booking codes (`BK-2026-0001`) and 32-char URL-safe manage tokens |
| DevOps | Dockerfile, docker-compose, GitHub Actions CI, MIT license |

## Smart slot scheduling

`slots_for(provider, service, day)` is the core algorithm. It generates available time slots by:

1. Looking up the provider's `WorkingHours` for that weekday
2. Stepping from start to end in increments of `service.duration_minutes + service.buffer_minutes`
3. Dropping candidates earlier than now + `min_lead_minutes` (configurable per business)
4. Dropping candidates that conflict with existing PENDING / CONFIRMED bookings (also respecting *their* buffer)
5. Dropping candidates inside any provider `TimeOff` window

So if a customer rebooks at 5:55pm for a service starting at 6pm and the business has min-lead = 60 min, no slot. If a stylist's previous client has a 45-min service with a 15-min buffer, the next slot starts a full hour later. All configurable from the admin panel — no code changes needed.

## Customer self-service flow

When a booking is created, the system generates a 32-character URL-safe `manage_token`. The confirmation email includes a link like `https://example.com/booking/<token>/manage/` that resolves to a page where the customer can cancel or reschedule. No password needed — the token itself is the credential. Cancellation is gated by `BusinessSettings.cancellation_hours` so businesses can prevent last-minute drops.

## Skills demonstrated

- Complex data modelling (8 interrelated models including a singleton settings model, OneToOne user link for provider portal, JSON snapshot fields for walk-ins where customer FK might be null)
- Inline formsets for working-hours management (per-provider, weekday-scoped)
- Custom `@provider_required` decorator for portal-level access control with `request.provider` injection
- Auto-generated booking codes (year-prefixed, sequential, unique-checked)
- iCalendar (RFC 5545) file generation from scratch (no library needed)
- Token-based URL access for unauthenticated self-service (signed URLs done with random tokens because they're simpler and equally secure)
- Conflict-prevention algorithms with buffer + time-off logic
- Three distinct UIs in one codebase (customer + admin + provider) with shared design language but theme-differentiated

## What I deliver to a client

Full source, branded theme matching their business, demo data seed, deployment to their domain, custom email templates, integration with their email provider (SMTP / Mailgun / SES), 30-day support, and a training session for their staff.

## Variations I've built on top of this

- Twilio SMS reminders 24h before appointments
- Razorpay / Stripe payment-on-booking instead of pay-on-arrival
- Google Calendar two-way sync for providers
- Multi-location support for chains
- A QR-code check-in flow for the customer-facing app

## Lessons / opinions

Booking systems live or die on slot generation correctness. Spend half your build time on `slots_for()`. Always store snapshot fields on Booking (`customer_name` etc.) in addition to the FK — customers' details change and you don't want historical data shifting. Token-based self-service URLs are massively underutilized — they're more secure than email-only and dramatically reduce password-reset support. The provider portal is the highest-leverage feature because it eliminates "the admin has to do everything" bottlenecks.

---

← [Back to portfolio](../README.md)
