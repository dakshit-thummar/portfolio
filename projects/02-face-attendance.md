# Face Recognition Attendance System

> Live webcam-based attendance tracking. Register employees with a photo, walk past the camera, get marked present automatically.

🔒 *Source code is in a private repository. Available on request.*

---

## The problem

Manual attendance — paper sign-in sheets, spreadsheet entry, or biometric fingerprint scanners — is slow, error-prone, and hated by everyone involved. A camera-based system that recognizes faces silently from a distance solves all three problems and doesn't require employees to touch anything.

This is the rarest skill on my CV combined with one of the highest-conversion freelance niches: small-to-mid offices, schools, gyms, and coworking spaces all want this and there are very few freelancers who actually deliver a working version.

## What it does

- **Register an employee** with name, ID, department, email, and a clear face photo. The app computes a 128-dimensional face encoding from the photo and stores it.
- **Live recognition page** — opens the webcam, captures a frame every 3 seconds, sends it to the server, gets back a match (or "no face" / "unknown").
- **Auto-marks attendance** on first match per day per employee. Same employee re-recognized later the same day shows "already marked" — no double-counting.
- **Dashboard** with stat cards (employees, present today, absent, total logs), a 30-day attendance trend line chart, and a department breakdown doughnut.
- **Logs page** with date-range and per-employee filters, pagination, and CSV export.
- **Admin panel** for bulk operations and quick edits.

## Tech stack

| Layer | Tools |
|---|---|
| Backend | Django 5 |
| Vision | OpenCV (`opencv-python`), `face_recognition` (built on dlib), NumPy |
| Storage | SQLite + media folder for employee photos |
| Frontend | Bootstrap 5, Chart.js, MediaDevices.getUserMedia for webcam |
| DevOps | Dockerfile (with cmake + dlib build deps), GitHub Actions CI |

## How recognition works

1. Employee registration: server accepts a photo upload, calls `face_recognition.face_encodings()` to get a 128-d float vector, JSON-serializes it into a `TextField` on the Employee row.
2. Live recognition: browser captures a frame from the webcam canvas, encodes as base64 JPEG, POSTs to `/api/recognize/`.
3. Server decodes the frame, runs `face_recognition.face_encodings()` to extract a target encoding, then compares against all stored encodings using cosine distance (`numpy.linalg.norm`).
4. If the closest match is within tolerance (default 0.5), it's a hit. The server creates an `AttendanceLog` row with the matched employee, date, time, and confidence score.
5. `unique_together = (employee, date)` on AttendanceLog prevents double-marking.

## Skills demonstrated

- Computer vision pipelines: image upload → encoding → matching → response
- Numerical work with NumPy (vectorized distance calculations against a candidate set)
- Browser-side webcam capture + JPEG encoding without external libraries
- Async-style polling (every 3s) without overengineering with WebSockets
- Real-world ML integration constraints — `dlib` build dependencies, fallback when face not detected, low-confidence handling
- Aggregation queries for dashboard charts (`extra(select=...)` for date casting on SQLite)

## What I deliver to a client

Full source, Dockerfile that handles the cmake / dlib dependency dance, deployment guide, walkthrough video showing registration → recognition → log review, and a tuning guide for adjusting the recognition tolerance based on lighting / camera quality at their location. I can also extend with Telegram / WhatsApp notifications on absentees, multi-camera support, and HR system integrations (Greythr, BambooHR) on follow-up engagements.

## Variations I've built on top of this

- A school version that emails parents when their kid is marked absent
- A gym version that tracks first-and-last-of-day check-ins for member-hours billing
- An office version with a "wave-to-clock-out" gesture
- A coworking version with member-tier badges shown on the recognition screen

## Lessons / opinions

`face_recognition` (which wraps dlib) is dramatically easier to integrate than building from raw OpenCV, but it requires CMake and a C++ compiler at install time — that's the main reason this is rare on Upwork. Once you've solved the install dance once, the actual model work is straightforward. Recognition tolerance should be tuned per-deployment — a brightly-lit office can get away with 0.4; a dim factory floor needs 0.55. Always store both the source image and the encoding so you can re-encode if you upgrade libraries later.

---

← [Back to portfolio](../README.md)
