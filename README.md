# agentify-runtime

### Stack

| Layer                    | The Golden Standard Component | Why It Wins                                                                                     |
| :----------------------- | :---------------------------- | :---------------------------------------------------------------------------------------------- |
| **Web Framework**        | FastAPI + Uvicorn             | Built natively on ASGI for blazing-fast concurrent request handling.                            |
| **Primary Database**     | PostgreSQL                    | The undisputed standard relational database for Python apps.                                    |
| **Database ORM**         | SQLAlchemy 2.0 (Async)        | Massive feature set, fully async-compatible, type-hint friendly.                                |
| **Database Migrations**  | Alembic                       | The official migration engine for SQLAlchemy. Handles schema evolution seamlessly.              |
| **Data Validation**      | Pydantic v2                   | Core engine of FastAPI. Written in Rust; handles data parsing and serialization.                |
| **Environment Settings** | Pydantic-Settings             | Restricts env var type errors by loading settings directly into type-hinted classes.            |
| **Authentication**       | pwdlib (Argon2) + PyJWT       | The modern replacement for the deprecated passlib to secure and hash passwords safely.          |
| **Task Queue / Worker**  | Dramatiq or Arq               | Highly efficient alternatives to Celery for running heavy background workers natively in async. |
| **Caching / Session**    | Redis (redis-py)              | Used for blazingly fast in-memory caching and session management.                               |

### Structure

| Layer / Directory               | Primary Purpose & Responsibility                                                                                                                                                                 |
| :------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`app/main.py`**               | Creates the `FastAPI()` instance, configures global middlewares (CORS), handles startup/shutdown events, and mounts the API routers.                                                             |
| **`app/api/`**                  | **Strictly Route Handlers.** Receives HTTP requests, calls the appropriate service layer functions, and returns data. Keep this thin—no direct database queries or heavy business logic.         |
| **`app/core/`**                 | Global application setup. Includes system-wide environments parsed via **Pydantic Settings** (`config.py`), logger setups, and shared encryption functions.                                      |
| **`app/services/`**             | **The Business Logic Layer.** This handles internal workflows, calculations, data enrichment, and system interactions. Services can be easily unit-tested without launching a mock web server.   |
| **`app/repositories/`**         | **The Data Access Layer.** Handles isolated operations with your data persistence framework (SQLAlchemy, SQLModel, Tortoise). Keeps database query strings out of your business logic.           |
| **`app/models/` vs `schemas/`** | **Keep them separate.** `models/` contains your actual database tables (e.g., SQLAlchemy mappings). `schemas/` holds Pydantic models used to enforce API input/output validation data contracts. |
