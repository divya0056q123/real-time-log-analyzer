# Real-Time Log Analyzer

A full-stack, local-first dashboard for ingesting, searching, and monitoring application logs in real time.

## Features

- **Live monitoring:** FastAPI backend, WebSocket updates, metrics, and event timeline.
- **Flexible ingestion:** Import `.log`/`.txt` files, generate sample logs, or push structured events through the authenticated HTTP ingestion API.
- **Parsing and exploration:** Parses common application, Apache, and syslog formats. Search and filter by severity, service, and time range.
- **Detection and analysis:** Configurable error-burst alerts, robust median/MAD anomaly detection, and similar-message template grouping.
- **Accounts:** First-run signup, login, signed expiring sessions, password hashing, and login throttling.
- **Storage:** SQLite by default, with an optional PostgreSQL backend.
- **Evaluation:** Synthetic detector evaluation, a documented small Loghub BGL sample evaluation, and an ingestion throughput benchmark.

## Stack

- **Frontend:** React, TypeScript, Vite
- **Backend:** Python, FastAPI, WebSockets
- **Database:** SQLite or PostgreSQL

## Quick start (Windows PowerShell)

### Backend

```powershell
cd backend
py -3.13 -m venv ..\.venv
..\.venv\Scripts\python.exe -m pip install -r requirements.txt
..\.venv\Scripts\python.exe -m uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

### Frontend (new terminal)

```powershell
cd frontend
npm install
npm run dev
```

Open <http://127.0.0.1:5173/>. On first launch, create an account. The backend serves the API on port 8000; Vite proxies `/api` and `/ws` to it.

## Using the dashboard

1. Sign up or sign in.
2. Choose **Generate events** to populate sample activity, or **Import logs** to load a text log file.
3. Search and filter the live log stream; inspect metrics, alert activity, anomalies, and error patterns.
4. Add an alert rule from **Alert activity** to monitor an error threshold.

## Configuration and security

Authentication and registration are enabled by default. For a stable signing secret, set `SECRET_KEY` in a backend `.env` file using a long random value. Set `ALLOW_REGISTRATION=false` after creating permitted accounts. Do not commit `.env` files or real secrets. Accounts currently share the same workspace and data; per-user tenant isolation is not implemented.

Password recovery is administrator-operated because no email service is configured. From the project root, run `..\.venv\Scripts\python.exe backend\scripts\reset_password.py` in PowerShell and follow the hidden prompts. Use the same database configuration as the backend.

## Evaluation

The synthetic benchmark and its results are documented in the project README. Real-data evaluation used a small, sparse Loghub BGL sample; it does **not** establish general production accuracy. See [`docs/REAL_WORLD_EVALUATION.md`](docs/REAL_WORLD_EVALUATION.md) for protocol, results, and limitations.

Run backend tests:

```powershell
cd backend
..\.venv\Scripts\python.exe -m pytest tests -q
```

Run frontend tests and production build:

```powershell
cd frontend
npm test
npm run build
```

## Project structure

- `backend/app/` — API, authentication, parsing, ingestion, storage, alerts, anomaly detection, and clustering.
- `backend/scripts/` — log shipper, password reset utility, evaluators, and benchmark.
- `frontend/src/` — dashboard, authentication flow, API client, and tests.
- `docs/` — project report outline and real-data evaluation.

## Known limitations

- User accounts share logs and alert rules in a single workspace.
- Local password recovery requires access to the backend machine.
- This is a prototype/academic project; evaluate on representative logs and harden configuration before public deployment.

See the [full project documentation](./README.md) in this repository's downloadable archive for deeper implementation notes.
