# Flask + Redis + SQLite — Products API

A high-performance REST API built with Flask, leveraging a dual-layer data strategy: SQLite for persistent storage and Redis as an in-memory cache layer to drastically reduce database queries and response latency.

## Architecture

The project follows a clean, layered architecture with clear separation of concerns:

- **Routes** — handle HTTP requests and delegate to use cases
- **Use Cases** (`data/`) — encapsulate business logic, completely decoupled from HTTP or database details
- **Repositories** — abstract all data access behind interfaces, making the persistence layer fully swappable
- **Composers** — wire up dependencies following the Dependency Injection pattern

The cache strategy is straightforward but effective: on every read, Redis is hit first. On a cache miss, the query falls through to SQLite and the result is automatically backfilled into Redis with a 60-second TTL — keeping the cache warm without stale data.

## Stack

- Python 3.10
- Flask
- SQLite (persistence)
- Redis (caching)

## Endpoints

```
POST /products
Body: { "name": string, "price": float, "quantity": int }

GET /products/<product_name>
```

## Getting Started

```bash
# Activate the virtual environment
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Start Redis
redis-server

# Run the application
python -m flask --app src/main/server/server_setting run
```

## Project Structure

```
src/
  data/           # Use cases (ProductCreator, ProductFinder)
  http_types/     # HttpRequest / HttpResponse types
  main/
    composer/     # Dependency injection composers
    routes/       # Flask route handlers
    server/       # App factory and server config
  models/
    redis/        # Redis repository + connection
    sqlite/       # SQLite repository + connection
redis_raw.py      # Standalone Redis command reference/scratch pad
```
