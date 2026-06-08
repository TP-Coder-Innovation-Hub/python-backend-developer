# Logging and Monitoring

``

You can't fix what you can't see. Logging tells you what your application is doing. Monitoring tells you when something goes wrong.

## Why Logging

When your API returns a 500 error at 3 AM, you need to know:
- What request caused it?
- What was the error?
- What was the application state?

Without logs, you're guessing. With logs, you can diagnose and fix.

## Python Logging Module

Python has a built-in `logging` module:

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(name)s %(message)s",
)

logger = logging.getLogger("myapp")

logger.debug("Detailed diagnostic information")   # usually hidden
logger.info("Request processed successfully")      # normal operations
logger.warning("Disk usage above 80%")            # something unusual
logger.error("Failed to connect to database")     # something failed
logger.critical("System is down")                 # system unusable
```

Log levels from lowest to highest severity:

| Level | When to use |
|-------|------------|
| DEBUG | Detailed diagnostic info for debugging |
| INFO | Normal operational messages (request received, job completed) |
| WARNING | Something unexpected but the app still works |
| ERROR | A failure in a specific operation |
| CRITICAL | The entire application is in trouble |

## Structured Logging

In production, logs are consumed by tools (ELK, Datadog, CloudWatch). Structured logs (JSON) are easier to search and filter:

```python
import json
import logging
import sys

class JSONFormatter(logging.Formatter):
    def format(self, record):
        log_entry = {
            "timestamp": self.formatTime(record),
            "level": record.levelname,
            "message": record.getMessage(),
            "module": record.module,
            "function": record.funcName,
        }
        if hasattr(record, "request_id"):
            log_entry["request_id"] = record.request_id
        return json.dumps(log_entry)

handler = logging.StreamHandler(sys.stdout)
handler.setFormatter(JSONFormatter())

logger = logging.getLogger("myapp")
logger.addHandler(handler)
logger.setLevel(logging.INFO)
```

Output:

```json
{"timestamp": "2026-06-07 10:30:00", "level": "INFO", "message": "User created", "module": "users", "function": "create_user", "request_id": "abc-123"}
```

Structured logging lets you search: "show me all ERROR logs with request_id=abc-123".

## Logging in FastAPI

Add a request logging middleware:

```python
import logging
import time
import uuid
from fastapi import FastAPI, Request

logger = logging.getLogger("myapp")
app = FastAPI()

@app.middleware("http")
async def log_requests(request: Request, call_next):
    request_id = str(uuid.uuid4())
    start = time.time()

    response = await call_next(request)

    duration_ms = (time.time() - start) * 1000
    logger.info(
        "Request completed",
        extra={
            "request_id": request_id,
            "method": request.method,
            "path": request.url.path,
            "status": response.status_code,
            "duration_ms": round(duration_ms, 2),
        },
    )
    response.headers["X-Request-ID"] = request_id
    return response
```

Every request is logged with its method, path, status code, and duration.

## Health Checks

A health check endpoint tells monitoring systems whether your service is alive:

```python
@app.get("/health")
async def health():
    return {"status": "healthy"}

@app.get("/health/ready")
async def readiness(db: Session = Depends(get_db)):
    try:
        db.execute(text("SELECT 1"))
        return {"status": "ready"}
    except Exception:
        raise HTTPException(status_code=503, detail="Database unavailable")
```

- `/health` -- is the process running? (liveness)
- `/health/ready` -- can it handle requests? (readiness -- checks dependencies)

Orchestrators (Kubernetes, Docker Compose) use these to decide whether to route traffic to your service or restart it.

## Monitoring Basics

Logging tells you what happened. Monitoring tells you what's happening right now.

| What to monitor | Why |
|----------------|-----|
| Request rate | How much traffic are you handling? |
| Error rate | What percentage of requests fail? |
| Response time (p50, p95, p99) | How fast is your API? |
| CPU and memory usage | Is your service under strain? |
| Database connections | Are you running out of connections? |

Use a tool (Datadog, Prometheus + Grafana, CloudWatch) to collect and visualize these metrics. Set alerts for anomalies: error rate > 5%, response time p99 > 2s.
