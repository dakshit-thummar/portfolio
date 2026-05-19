# Real-Time Chat — Django Channels + WebSockets

> Multi-room real-time chat with branded auth, typing indicators, online presence, and message history. Built to demonstrate that I can ship Channels-based real-time features that most "Python developers" can't.

🔒 *Source code is in a private repository. Available on request.*

---

## The problem

Real-time features (chat, live notifications, collaborative editing) intimidate most Django developers because they involve a fundamentally different runtime model — ASGI instead of WSGI, async consumers instead of sync views, Redis pub/sub instead of HTTP requests. There are tons of "Python developer" freelancers; very few who can confidently architect Channels-based systems.

This project exists to prove the latter.

## What it does

- **Branded signup and login** pages (no `/admin/login/` redirect — the chat has its own auth flow)
- **Multiple chat rooms** — users join / create freely; room list with message-count badges
- **Real-time messaging** over WebSockets — messages appear in everyone's room without a page reload
- **Typing indicators** — when one user types, others see "alice is typing..."
- **Online users panel** — sidebar shows who's currently in the room with a green dot
- **Message history** — joining a room loads the last 200 messages
- **Message search** within a room
- **Message bubbles** with avatars (yours green, others blue) and inline timestamps

## Tech stack

| Layer | Tools |
|---|---|
| Backend | Django 5, Django Channels 4, Daphne (ASGI server) |
| Real-time | WebSocket consumers, Channel Layer (in-memory for dev, Redis for prod) |
| Auth | Django built-in auth with custom signup view |
| Frontend | Bootstrap 5, vanilla JS (~80 lines), no React or Vue |
| Database | SQLite with `Room` and `Message` models, indexed FK |
| DevOps | Dockerfile running Daphne, docker-compose with Redis, GitHub Actions CI |

## Architecture

When a user opens `/chat/<room_name>/`, the page loads the last 200 messages from the DB. The browser opens a WebSocket to `ws://host/ws/chat/<room_name>/`. The `ChatConsumer` (subclass of `AsyncJsonWebsocketConsumer`):

1. On `connect`, joins a Channel-Layer group named `chat_<room>`, accepts the connection, and broadcasts a presence update (online users in the room)
2. On `receive_json`, dispatches by `action`: `message` persists to DB then broadcasts to the group; `typing` broadcasts a typing event
3. On `disconnect`, leaves the group and re-broadcasts presence

Channel Layer is configured to use Redis in production (so multiple Daphne workers can broadcast across processes) but falls back to an in-memory layer in development so the project runs without Redis.

## Skills demonstrated

- ASGI-based Django (different runtime model than WSGI; understanding the difference matters)
- Async consumer patterns (`AsyncJsonWebsocketConsumer`, `database_sync_to_async`)
- Channel Layer groups for room-scoped broadcast
- Frontend WebSocket handling with `WebSocket` API directly (no Socket.IO needed)
- Presence tracking — done in-memory for the demo, but the pattern extends cleanly to Redis sets
- DOM manipulation without React for cases where it would be overkill
- Conditional ASGI server (Daphne in prod, runserver in dev)
- Docker setup for an ASGI app (Daphne entrypoint, Redis service)

## What I deliver to a client

Full source, deployment to a VPS or Render with Daphne behind Nginx, Redis setup, monitoring of WebSocket disconnect rates, and integration into their existing Django app if they have one.

## Variations I've built on top of this

- Direct messages (1:1 conversations) with read receipts
- File / image uploads inline in chat
- Message reactions (emoji)
- Mention notifications (`@username`)
- Voice / video calling via WebRTC signaling over the same WebSocket
- Live customer-support widgets that drop into existing websites

## Lessons / opinions

Channels has a steep learning curve compared to plain Django, but it's the right tool — Socket.IO + Node + a separate process is overkill for most projects. In-memory channel layer is fine for development; production needs Redis. Always wrap database calls in `database_sync_to_async` from `channels.db` — forgetting this causes silent failures under load. Avoid frontend frameworks for chat unless you're already using one — vanilla JS is more than enough.

---

← [Back to portfolio](../README.md)
