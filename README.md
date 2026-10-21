

## Architecture

```mermaid
flowchart LR
    subgraph Sources[Log sources]
        FILE[Log file upload]
        SHIPPER[Local log shipper]
        CLIENT[External application]
        DEMO[Sample event generator]
    end

    subgraph Backend[FastAPI backend]
        API[REST API and validation]
        PARSER[Format parser and normalizer]
        INGEST[Batch ingestion service]
        ALERTS[Alert rules]
        ANOMALY[Anomaly detection]
        MINER[Log template miner]
        HUB[WebSocket event hub]
    end

    subgraph Storage[Persistence]
        SQLITE[(SQLite)]
        POSTGRES[(Optional PostgreSQL)]
    end

    subgraph Web[React dashboard]
        AUTH[Signup and login]
        UI[Metrics, charts, logs, alerts]
    end

    FILE --> API
    SHIPPER --> API
    CLIENT --> API
    DEMO --> API
    API --> PARSER --> INGEST
    INGEST --> SQLITE
    INGEST -. alternative .-> POSTGRES
    INGEST --> ALERTS
    INGEST --> ANOMALY
    INGEST --> MINER
    INGEST --> HUB
    HUB -->|live events| UI
    UI -->|REST queries| API
    AUTH -->|bearer token| API
```

### Data flow

1. Log files, the log shipper, external applications, or the sample generator submit events to the backend.
2. The API validates and parses log input, normalizes fields, and stores accepted events in a batch using SQLite or PostgreSQL.
3. Alert rules and anomaly detection evaluate incoming activity; the template miner groups similar messages into patterns.
4. New log events and detections are broadcast over WebSockets. The dashboard also queries REST endpoints for historical logs, metrics, and summaries.
5. The React interface displays live metrics, charts, searchable logs, anomalies, patterns, and alert activity.
