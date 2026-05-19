# Web Scraping Dashboard

> Configurable, schedulable web scraping with a JSON-driven selector engine, success-rate analytics, and CSV export. Built so non-developers can add new scrape jobs without writing code.

🔒 *Source code is in a private repository. Available on request.*

---

## The problem

Clients constantly ask for "a script that scrapes X" — but they really want an ongoing system that re-scrapes daily, alerts them when the site structure changes, lets them search the results, and exports to spreadsheet. A one-shot script is a one-shot invoice; a dashboard becomes a retainer.

## What it does

- **Add a scrape job** through a form: name, URL, schedule (manual / hourly / daily), and a JSON-defined selector spec (`{"container": ".product", "fields": {"title": "h2", "link": "a@href"}}`)
- **Run jobs manually or on a schedule** (Celery + Celery Beat)
- **Dashboard** with active job count, total runs, success rate, total items scraped, plus a 14-day stacked bar chart (success vs failed runs)
- **Job detail page** with paginated results, full-text search across results, recent run history with status badges and error messages
- **Live job search** on the dashboard
- **CSV export** per job with all extracted fields

## Tech stack

| Layer | Tools |
|---|---|
| Backend | Django 5, Celery 5 + Celery Beat, Redis |
| Scraping | Requests + BeautifulSoup + lxml |
| Database | SQLite for dev; Postgres-ready |
| Frontend | Bootstrap 5, Bootstrap Icons, Chart.js (stacked bar) |
| DevOps | docker-compose with web + worker + beat + redis services, GitHub Actions CI with Redis service container |

## How the selector engine works

Selectors are stored as JSON on the `ScrapeJob` model:

```json
{
  "container": ".product-card",
  "fields": {
    "title": "h2",
    "price": ".price",
    "image": "img@src",
    "link": "a@href"
  }
}
```

The engine fetches the URL, parses with BeautifulSoup, finds all elements matching `container`, then for each container element extracts each field. The `@attr` syntax extracts an HTML attribute (e.g. `a@href` gets the link's href). Result is a list of dicts which get persisted as `ScrapeResult` rows with a JSONField for `data`.

## How scheduling works

Each job has a `schedule` field (manual / hourly / daily). Celery Beat reads the schedule from `django-celery-beat`'s database scheduler and enqueues runs at the right time. The worker picks up the task, calls the scrape engine, persists results, and updates the `ScrapeRun` row's status. If anything fails (timeout, connection error, parse error), the exception message gets stored on the run for debugging.

## Skills demonstrated

- Background task orchestration with Celery + Redis (not just "fire and forget" — actual lifecycle tracking with status, error capture, item counts)
- Periodic task scheduling with Celery Beat + Django integration
- JSON-as-config patterns (lets non-developers define new scrapers without code changes)
- Robust HTML parsing with BeautifulSoup edge cases (None checks, attribute extraction, missing fields)
- Aggregation queries for chart data (`.extra()` for date casting, group by status)
- Pagination over large result sets
- Docker Compose multi-service orchestration (web + worker + beat + redis)

## What I deliver to a client

Full source, Docker setup that brings up everything with one command, deployment guide for VPS / Railway, a JSON-spec cookbook with 10+ pre-built selectors for common sites, monitoring setup, and a guide on respectful scraping (rate limits, robots.txt, user-agent rotation).

## Variations I've built on top of this

- Proxy-rotation support for sites that block by IP
- JavaScript rendering via Playwright when sites are SPA
- Webhook delivery — push results to a client's Slack / endpoint as they're scraped
- Diff-detection — alert when a tracked product price changes
- Scheduled CSV email export to client's inbox every Monday morning

## Lessons / opinions

Web scraping projects almost always evolve into "we also need monitoring" and "we also need notifications" — design for that from day one. The JSON selector approach trades a little flexibility for huge maintainability: clients can update selectors when sites change without filing a ticket. Celery Beat is fine until you have hundreds of jobs — past that, switch to a real scheduler like Temporal or Airflow.

---

← [Back to portfolio](../README.md)
