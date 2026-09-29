# Yatra Planner API

An async FastAPI service that aggregates **weather**, **currency exchange rates** and **places to visit** into a single travel plan. It offers a standard JSON endpoint (sources fetched concurrently) and a **Server-Sent Events** endpoint that streams results as each source completes.

## Features
- `POST /plan/` returns weather, places and currency data in one response, fetched concurrently with `asyncio.gather`
- `POST /stream/plan` streams progress and results via Server-Sent Events (SSE)
- In-memory TTL cache (1 hour) for weather and currency calls
- Request validation with Pydantic (end date after start date, trip length 1 to 14 days)
- Unit test for SSE serialization

## Tech stack
Python, FastAPI, httpx, Pydantic, Uvicorn, WeatherAPI, ExchangeRate-API

## Setup
```bash
git clone https://github.com/Shivalika-15/yatra-app.git
cd yatra-app
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env    # then add your own API keys
```

Get free keys from [WeatherAPI](https://www.weatherapi.com/) and [ExchangeRate-API](https://www.exchangerate-api.com/).

## Run
```bash
cd app
uvicorn main:app --reload
```
Open http://127.0.0.1:8000/docs for the interactive API docs.

## Example request
```bash
curl -X POST http://127.0.0.1:8000/plan/ \
  -H "Content-Type: application/json" \
  -d '{"destination":"goa","start_date":"2026-12-20","end_date":"2026-12-24","base_currency":"INR"}'
```

## Tests
```bash
python -m unittest discover tests
```

## Notes
Places data is currently a small built-in dataset (Manali, Goa, Jaipur, ...). Future work: a real places API, persistent cache (Redis), and rate limiting.
