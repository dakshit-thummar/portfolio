# Invoice & Billing System

> Freelancer-focused invoicing app: clients, products, multi-line invoices with tax + discounts, professional PDF generation, email-as-attachment delivery, and a revenue dashboard. Zero system dependencies — runs anywhere Python runs.

🔒 *Source code is in a private repository. Available on request.*

---

## The problem

Indian freelancers and small businesses overpay for invoicing software (Zoho Invoice, FreshBooks, QuickBooks) when their needs are pretty simple: track clients, generate professional PDFs, email them, mark paid, run reports. A self-hosted version saves ₹2,000–5,000/month per business and gives them complete control over branding and data.

## What it does

- **Client management** — name, email, phone, address, GSTIN (India), internal notes
- **Product / service catalog** with default rate and tax percent
- **Multi-line invoices** — per-line description, quantity, rate, tax %, computed amount; invoice-level discount and notes
- **Auto-numbered** with year prefix (`INV-2026-0001`, `INV-2026-0002`, ...)
- **Status workflow** — draft → sent → paid → overdue (auto-flagged on save) → cancelled
- **PDF generation** with ReportLab — header with company branding, bill-to block, items table, totals (subtotal / tax / discount / grand total), notes section
- **Email invoice as PDF attachment** with a single click
- **Mark paid / unpaid** with one-click status update
- **Dashboard** with paid revenue / outstanding / overdue stat cards, 6-month stacked revenue chart (paid vs outstanding), status doughnut, recent invoices table
- **Search + status filter + pagination** on every list

## Tech stack

| Layer | Tools |
|---|---|
| Backend | Django 5 with custom auto-numbered invoice codes |
| PDF | ReportLab (pure Python — works on Windows out of the box, no GTK / WeasyPrint hassle) |
| Email | Django EmailMessage with PDF attachment via SMTP / console / file backend |
| Frontend | Bootstrap 5, Bootstrap Icons, Chart.js (stacked bar + doughnut) |
| DevOps | Dockerfile, docker-compose, GitHub Actions CI |

## Why ReportLab instead of WeasyPrint

WeasyPrint generates beautiful PDFs from HTML+CSS — but on Windows it requires GTK to be installed, which is a 30-minute setup nightmare for clients. ReportLab is pure Python, works on every platform without native dependencies, and produces clean professional invoices that look identical to the HTML preview. The tradeoff is that you write the layout in Python instead of HTML, but for invoice-style structured documents that's actually clearer.

## Skills demonstrated

- Custom auto-incrementing identifiers with year prefix (`INV-{year}-{seq:04d}`)
- ModelForm + inline formset combination for multi-line invoices
- Pure-Python PDF generation with table layouts, custom styles, branding
- Email with binary attachment via Django's EmailMessage
- Decimal arithmetic done correctly (no float currency math)
- Property-based computed fields (`subtotal`, `tax_total`, `total`) on models
- Aggregation queries for monthly revenue (group by month, sum by status)
- Doughnut + stacked bar combination on a single dashboard with Chart.js

## What I deliver to a client

Full source, custom invoice template matching the client's branding (logo, colours, terms), demo data seed, deployment guide, SMTP / email-provider setup walkthrough, and a one-pager on Indian GST handling if they want me to add that.

## Variations I've built on top of this

- Recurring invoices that auto-generate monthly retainers
- Stripe / Razorpay payment-link inclusion in emails (`Pay this invoice` button)
- Customer portal for clients to view their own invoice history and download past invoices
- Multi-currency support
- Quote / estimate workflow that converts to invoice once accepted
- A WhatsApp-send option using the WhatsApp Business API

## Lessons / opinions

ReportLab gets unfairly dismissed because the API looks dated, but for structured business documents it's faster, more reliable, and easier to deploy than HTML-to-PDF tools. Always store currency as `Decimal`, never `float`. Auto-numbered IDs should be human-readable (`INV-2026-0001`) not UUIDs — clients reference them on the phone. PDF generation should be on-demand from data, not stored — when you fix a typo in a template, every old invoice gets the fix automatically.

---

← [Back to portfolio](../README.md)
