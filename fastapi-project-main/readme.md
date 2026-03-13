# FastAPI App

Minimal FastAPI app with a health check endpoint.

## Prerequisites

- Python 3.11 or 3.12
- pip

## Setup

```bash
# Check Python version
python3 --version

# Create and activate virtual environment (macOS/Linux)
python3 -m venv .venv
source .venv/bin/activate

# Windows (PowerShell)
# .venv\Scripts\Activate.ps1

# Install dependencies
pip install --upgrade pip
pip install fastapi uvicorn
```

## Run locally

```bash
uvicorn app.main:app --reload
```

- API: http://127.0.0.1:8000  
- Docs: http://127.0.0.1:8000/docs

## API Endpoints

| Method | Path    | Description        |
|--------|---------|--------------------|
| GET    | `/`     | Root; returns status |
| GET    | `/health` | Health check for load balancers / monitoring |

## Project structure

```
fastapi-app/
├── app/
│   └── main.py
└── readme.md
```
