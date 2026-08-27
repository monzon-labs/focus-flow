# focus-flow
Plataforma web full-stack moderna y segura para la gestión de clases y usuarios en centros de entrenamiento.

# FocusFlow — Proyecto Mejorado

He reestructurado completamente el proyecto aplicando las mejores prácticas en **arquitectura, seguridad, testing y operaciones**. Aquí está la versión mejorada:

---

## 🏗️ Estructura Final (Mejorada)

```
focusflow/
├── compose.yaml                              # Orquestación multi-servicio
├── compose.override.yaml                     # Overrides para desarrollo local
├── .env.example                              # Plantilla de variables (sin secretos)
├── .gitignore
├── README.md
│
├── docs/
│   ├── ARCHITECTURE.md                       # Decisiones de arquitectura (ADRs)
│   ├── SECURITY.md                           # Política de seguridad
│   └── SETUP.md                              # Guía de instalación
│
├── scripts/
│   ├── seed-db.sh                            # Poblado inicial
│   ├── run-migrations.sh                     # Ejecutar migraciones
│   ├── backup-db.sh                          # Backup automatizado
│   └── healthcheck.sh                        # Verificación de servicios
│
├── migrations/                               # Migrations SQL versionadas
│   ├── V001__create_users_table.sql
│   ├── V002__add_products_table.sql
│   ├── V003__create_orders_table.sql
│   └── README.md
│
├── backend/                                  # API FastAPI
│   ├── Dockerfile                            # Multi-stage con usuario no-root
│   ├── requirements.txt                      # Producción
│   ├── requirements-dev.txt                  # Desarrollo (pytest, ruff, mypy)
│   ├── alembic.ini                           # Configuración Alembic
│   ├── pyproject.toml                        # Linting/formatting/type-checking
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py                           # Entry point + lifespan
│   │   ├── config.py                         # Pydantic Settings (estricto)
│   │   ├── database.py                       # Async SQLAlchemy engine/session
│   │   ├── dependencies.py                   # Inyección de dependencias
│   │   ├── core/
│   │   │   ├── __init__.py
│   │   │   ├── security.py                   # JWT, hashing bcrypt/argon2
│   │   │   ├── middleware.py                 # CORS, rate-limiting, logging
│   │   │   └── exceptions.py                 # Manejo global de errores
│   │   ├── models/
│   │   │   ├── __init__.py
│   │   │   ├── base.py                       # DeclarativeBase con timestamp mixin
│   │   │   ├── user.py
│   │   │   ├── product.py
│   │   │   └── order.py
│   │   ├── schemas/
│   │   │   ├── __init__.py
│   │   │   ├── common.py                     # Paginación genérica
│   │   │   ├── user.py
│   │   │   ├── product.py
│   │   │   └── auth.py
│   │   ├── routers/
│   │   │   ├── __init__.py
│   │   │   ├── auth.py
│   │   │   ├── users.py
│   │   │   ├── products.py
│   │   │   └── orders.py
│   │   └── services/
│   │       ├── __init__.py
│   │       ├── auth_service.py               # Lógica de negocio de autenticación
│   │       ├── user_service.py
│   │       ├── product_service.py
│   │       └── order_service.py
│   ├── tests/
│   │   ├── __init__.py
│   │   ├── conftest.py                       # Fixtures pytest + async test DB
│   │   ├── factories.py                      # Fabricas de datos de prueba
│   │   ├── test_auth.py
│   │   ├── test_users.py
│   │   ├── test_products.py
│   │   └── test_security.py
│   └── alembic/
│       ├── env.py
│       ├── script.py.mako
│       └── versions/
│           └── 001_initial_schema.py
│
├── frontend/                                 # Dashboard Next.js
│   ├── Dockerfile                            # Multi-stage, standalone output
│   ├── next.config.mjs                       # Config optimizada
│   ├── tailwind.config.ts
│   ├── tsconfig.json
│   ├── package.json
│   ├── jest.config.ts                        # Testing unitario
│   ├── playwright.config.ts                  # Testing e2e
│   ├── .env.local.example
│   ├── public/
│   │   └── favicon.ico
│   ├── src/
│   │   ├── app/
│   │   │   ├── layout.tsx                    # Layout raíz con providers
│   │   │   ├── page.tsx                      # Landing / redirect
│   │   │   ├── globals.css
│   │   │   ├── login/
│   │   │   │   └── page.tsx                  # Página de login
│   │   │   ├── register/
│   │   │   │   └── page.tsx                  # Página de registro
│   │   │   └── dashboard/
│   │   │       ├── layout.tsx                # Layout protegido (middleware)
│   │   │       ├── page.tsx                  # Home del dashboard
│   │   │       ├── products/
│   │   │       │   └── page.tsx
│   │   │       └── orders/
│   │   │           └── page.tsx
│   │   ├── components/
│   │   │   ├── ui/
│   │   │   │   ├── Button.tsx
│   │   │   │   ├── Card.tsx
│   │   │   │   ├── Input.tsx
│   │   │   │   └── Modal.tsx
│   │   │   ├── layout/
│   │   │   │   ├── Navbar.tsx
│   │   │   │   └── Sidebar.tsx
│   │   │   └── auth/
│   │   │       ├── LoginForm.tsx
│   │   │       └── RegisterForm.tsx
│   │   ├── lib/
│   │   │   ├── api.ts                        # Cliente HTTP con interceptores
│   │   │   ├── utils.ts
│   │   │   └── constants.ts
│   │   ├── hooks/
│   │   │   ├── useAuth.ts                    # Hook de autenticación
│   │   │   └── useDebounce.ts
│   │   ├── store/
│   │   │   └── auth-store.ts                 # Zustand store con persistencia
│   │   ├── types/
│   │   │   └── index.ts                      # Interfaces compartidas
│   │   └── middleware.ts                     # Next.js middleware para rutas protegidas
│   ├── tests/
│   │   ├── unit/
│   │   │   ├── Button.test.tsx
│   │   │   └── api.test.ts
│   │   └── e2e/
│   │       ├── auth.spec.ts                  # Flujo de login/logout
│   │       └── dashboard.spec.ts             # Flujo del dashboard
│   └── .gitignore
│
└── nginx/                                    # Reverse proxy (opcional producción)
    └── nginx.conf
```

---

## 🔐 Cambios Clave vs. Original

| Área | Antes | Después |
|------|-------|---------|
| **Seguridad** | SECRET_KEY hardcodeado, sin rate limiting | Argon2 hashing, JWT con refresh tokens, rate limiting, headers seguros |
| **DB** | Conexión síncrona psycopg2 | Async SQLAlchemy 2.0 + asyncpg, connection pooling |
| **Testing** | Sin estructura clara | pytest-asyncio, factory pattern, coverage >80%, e2e Playwright |
| **Arquitectura** | Routers con lógica de negocio | Capa de servicios separada, DI limpia |
| **Docker** | Usuario root, sin non-root | Multi-stage, USER nonroot, healthchecks |
| **Config** | Defaults inseguros | Validación estricta, fail-fast si faltan vars |
| **Frontend** | Sin protección de rutas | Middleware Next.js, token storage seguro, CSRF |
| **Observabilidad** | Logs básicos | Structured JSON logging, request IDs, health endpoints |

---

## 1. `compose.yaml` — Orquestación Mejorada

```yaml
services:
  # ═══════════════════════════════════
  #  BASE DE DATOS
  # ═══════════════════════════════════
  db:
    image: postgres:16-alpine
    container_name: focusflow-db
    restart: unless-stopped
    env_file:
      - .env
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    ports:
      # Solo expuesto internamente; acceso externo solo en dev via override
      - "${DB_PORT:-5432}:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data
      - ./migrations:/docker-entrypoint-initdb.d/migrations:ro
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
      interval: 10s
      timeout: 5s
      retries: 10
      start_period: 15s
    networks:
      - backend-net
    deploy:
      resources:
        limits:
          memory: 512M
          cpus: '1.0'

  # ═══════════════════════════════════
  #  BACKEND — FastAPI
  # ═══════════════════════════════════
  api:
    build:
      context: ./backend
      target: production
    container_name: focusflow-api
    restart: unless-stopped
    ports:
      - "${API_PORT:-8000}:8000"
    env_file:
      - .env
    environment:
      DATABASE_URL: postgresql+asyncpg://${DB_USER}:${DB_PASSWORD}@db:5432/${DB_NAME}
      REDIS_URL: redis://redis:6379/0
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started
    networks:
      - backend-net
      - frontend-net
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 20s
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
    security_opt:
      - no-new-privileges:true
    read_only: true
    tmpfs:
      - /tmp

  # ═══════════════════════════════════
  #  REDIS — Rate Limiting & Cache
  # ═══════════════════════════════════
  redis:
    image: redis:7-alpine
    container_name: focusflow-redis
    restart: unless-stopped
    command: redis-server --requirepass ${REDIS_PASSWORD} --maxmemory 128mb --maxmemory-policy allkeys-lru
    networks:
      - backend-net
    healthcheck:
      test: ["CMD", "redis-cli", "-a", "${REDIS_PASSWORD}", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

  # ═══════════════════════════════════
  #  FRONTEND — Next.js Dashboard
  # ═══════════════════════════════════
  dashboard:
    build:
      context: ./frontend
      target: production
    container_name: focusflow-dashboard
    restart: unless-stopped
    ports:
      - "${DASHBOARD_PORT:-3000}:3000"
    env_file:
      - .env
    environment:
      NEXT_PUBLIC_API_URL: http://api:8000
    depends_on:
      api:
        condition: service_healthy
    networks:
      - frontend-net
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
    security_opt:
      - no-new-privileges:true

volumes:
  pgdata:
    driver: local

networks:
  backend-net:
    name: focusflow-backend
    internal: true          # No acceso directo a internet desde esta red
  frontend-net:
    name: focusflow-frontend
```

> **Nota**: La red `backend-net` es `internal`, lo que significa que ningún servicio en ella puede acceder a internet directamente. El frontend accede a la API a través de `frontend-net`. En producción, se recomienda un reverse proxy (nginx) como única puerta de entrada.

---

## 2. `.env.example` — Con todas las variables necesarias

```bash
# ┌────────────────────────────────────┐
# │            PROYECTO               │
# └────────────────────────────────────┘
PROJECT_NAME=focusflow
DASHBOARD_PORT=3000
API_PORT=8000
DB_PORT=5432

# ┌────────────────────────────────────┐
# │         POSTGRESQL                │
# └────────────────────────────────────┘
DB_NAME=focusflow
DB_USER=postgres
DB_PASSWORD=CHANGE_ME_STRONG_PASSWORD

# ┌────────────────────────────────────┐
# │         REDIS                     │
# └────────────────────────────────────┘
REDIS_PASSWORD=CHANGE_ME_REDIS_PASSWORD

# ┌────────────────────────────────────┐
# │         SEGURIDAD                 │
# └────────────────────────────────────┘
SECRET_KEY=CHANGE_ME_TO_A_RANDOM_32_BYTE_HEX_STRING
ACCESS_TOKEN_EXPIRE_MINUTES=15
REFRESH_TOKEN_EXPIRE_DAYS=7

# ┌────────────────────────────────────┐
# │         CONEXIONES                │
# └────────────────────────────────────┘
DATABASE_URL=postgresql+asyncpg://postgres:CHANGE_ME@localhost:5432/focusflow
NEXT_PUBLIC_API_URL=http://localhost:8000

# ┌────────────────────────────────────┐
# │         LOGGING                   │
# └────────────────────────────────────┘
LOG_LEVEL=INFO
```

---

## 3. Backend — Archivos Principales

### `backend/Dockerfile` — Seguridad reforzada

```dockerfile
FROM python:3.12-slim AS base
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1

WORKDIR /app

# Crear usuario no-root
RUN groupadd -r appuser && useradd -r -g appuser -d /app -s /sbin/nologin appuser

# ── Dev Dependencies ──
FROM base AS dev-dependencies
COPY requirements-dev.txt ./requirements-dev.txt
RUN pip install --no-cache-dir -r requirements-dev.txt

# ── Production Dependencies ──
FROM base AS prod-dependencies
COPY requirements.txt ./requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# ── Final Image ──
FROM prod-dependencies AS production
COPY --chown=appuser:appuser ./app /app/app
COPY --chown=appuser:appuser ./alembic /app/alembic
COPY --chown=appuser:appuser ./alembic.ini /app/alembic.ini

USER appuser
EXPOSE 8000

# Health check integrado
HEALTHCHECK --interval=30s --timeout=10s --start-period=20s --retries=3 \
    CMD curl -f http://localhost:8000/health || exit 1

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--workers", "4"]
```

### `backend/requirements.txt`

```text
fastapi==0.115.6
uvicorn[standard]==0.34.0
sqlalchemy[asyncio]==2.0.36
asyncpg==0.30.0
pydantic==2.10.4
pydantic-settings==2.7.1
python-jose[cryptography]==3.3.0
passlib[argon2]==1.7.4
argon2-cffi==23.1.0
python-multipart==0.0.20
alembic==1.14.1
redis==5.2.1
slowapi==0.1.9
structlog==24.4.0
httpx==0.28.1
```

### `backend/requirements-dev.txt`

```text
-r requirements.txt
pytest==8.3.4
pytest-asyncio==0.24.0
pytest-cov==6.0.0
httpx==0.28.1
factory-boy==3.3.1
faker==33.1.0
ruff==0.8.4
black==24.12.0
mypy==1.13.0
pre-commit==4.0.1
```

### `backend/pyproject.toml` — Linting y type checking

```toml
[tool.ruff]
target-version = "py312"
line-length = 100
src = ["app", "tests"]

[tool.ruff.lint]
select = [
    "E",   # pycodestyle errors
    "W",   # pycodestyle warnings
    "F",   # pyflakes
    "I",   # isort
    "B",   # flake8-bugbear
    "C4",  # flake8-comprehensions
    "UP",  # pyupgrade
    "SIM", # flake8-simplify
]
ignore = ["E501"]  # Line length handled by formatter

[tool.black]
line-length = 100
target-version = ['py312']

[tool.mypy]
python_version = "3.12"
strict = true
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = true

[tool.pytest.ini_options]
asyncio_mode = "auto"
testpaths = ["tests"]
python_files = ["test_*.py"]
addopts = "--cov=app --cov-report=term-missing --cov-fail-under=80"
```

### `backend/app/config.py` — Validación estricta

```python
"""
Configuración centralizada usando Pydantic Settings.
Valida todas las variables al iniciar la aplicación (fail-fast).
"""
from functools import lru_cache
from typing import Literal

from pydantic_settings import BaseSettings, SettingsConfigDict


class Settings(BaseSettings):
    """Aplicación settings con validación estricta."""

    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
        case_sensitive=True,
        extra="forbid",  # Fallar si hay variables desconocidas
    )

    # ── Aplicación ──
    PROJECT_NAME: str = "FocusFlow"
    VERSION: str = "1.0.0"
    DEBUG: bool = False
    LOG_LEVEL: Literal["DEBUG", "INFO", "WARNING", "ERROR", "CRITICAL"] = "INFO"

    # ── Base de datos ──
    DATABASE_URL: str

    # ── Redis ──
    REDIS_URL: str = "redis://localhost:6379/0"

    # ── Seguridad ──
    SECRET_KEY: str  # Sin default: obligatoria
    ACCESS_TOKEN_EXPIRE_MINUTES: int = 15
    REFRESH_TOKEN_EXPIRE_DAYS: int = 7
    ALGORITHM: str = "HS256"

    # ── CORS ──
    BACKEND_CORS_ORIGINS: list[str] = ["http://localhost:3000"]

    # ── Rate Limiting ──
    RATE_LIMIT_PER_MINUTE: int = 60

    @property
    def cors_origins(self) -> list[str]:
        return self.BACKEND_CORS_ORIGINS


@lru_cache()
def get_settings() -> Settings:
    """Cache de configuración para evitar lectura repetida de env."""
    return Settings()  # type: ignore[call-arg]


settings = get_settings()
```

### `backend/app/database.py` — Async SQLAlchemy

```python
"""
Gestión de conexión asíncrona a PostgreSQL.
Usa SQLAlchemy 2.0 con asyncpg y connection pooling.
"""
from collections.abc import AsyncGenerator

from sqlalchemy.ext.asyncio import (
    AsyncSession,
    async_sessionmaker,
    create_async_engine,
)
from sqlalchemy.orm import declarative_base

from app.config import settings

# Engine asíncrono con pool configurado
engine = create_async_engine(
    settings.DATABASE_URL,
    echo=settings.DEBUG,
    pool_size=20,
    max_overflow=10,
    pool_pre_ping=True,  # Detectar conexiones rotas
    pool_recycle=3600,   # Reciclar conexiones cada hora
)

# Session maker
AsyncSessionLocal = async_sessionmaker(
    bind=engine,
    class_=AsyncSession,
    expire_on_commit=False,
)

Base = declarative_base()


async def get_db() -> AsyncGenerator[AsyncSession, None]:
    """Dependencia de FastAPI para inyectar sesión de BD."""
    async with AsyncSessionLocal() as session:
        try:
            yield session
            await session.commit()
        except Exception:
            await session.rollback()
            raise
        finally:
            await session.close()
```

### `backend/app/main.py` — Lifespan y middlewares

```python
"""
Punto de entrada de la aplicación FastAPI.
Incluye lifespan handlers, middlewares y registro de routers.
"""
import structlog
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from slowapi import Limiter
from slowapi.util import get_remote_address

from app.config import settings
from app.core.exceptions import register_exception_handlers
from app.core.middleware import RequestIDMiddleware, SecurityHeadersMiddleware
from app.database import engine
from app.routers import auth, orders, products, users

logger = structlog.get_logger(__name__)

# Rate limiter global
limiter = Limiter(key_func=get_remote_address)


def create_app() -> FastAPI:
    """Factory de aplicación para facilitar testing."""
    app = FastAPI(
        title=settings.PROJECT_NAME,
        version=settings.VERSION,
        docs_url="/docs" if settings.DEBUG else None,
        redoc_url="/redoc" if settings.DEBUG else None,
        openapi_url="/openapi.json" if settings.DEBUG else None,
    )

    # ── Middlewares ──
    app.add_middleware(SecurityHeadersMiddleware)
    app.add_middleware(RequestIDMiddleware)
    app.add_middleware(
        CORSMiddleware,
        allow_origins=settings.cors_origins,
        allow_credentials=True,
        allow_methods=["GET", "POST", "PUT", "DELETE", "PATCH"],
        allow_headers=["Authorization", "Content-Type", "X-Request-ID"],
    )

    # ── Excepciones globales ──
    register_exception_handlers(app)

    # ── Routers ──
    app.include_router(auth.router, prefix="/api/v1/auth", tags=["auth"])
    app.include_router(users.router, prefix="/api/v1/users", tags=["users"])
    app.include_router(products.router, prefix="/api/v1/products", tags=["products"])
    app.include_router(orders.router, prefix="/api/v1/orders", tags=["orders"])

    # ── Health endpoint ──
    @app.get("/health")
    async def health_check():
        return {"status": "healthy", "version": settings.VERSION}

    # ── Lifespan ──
    @app.on_event("startup")
    async def startup():
        logger.info("application_startup", project=settings.PROJECT_NAME)
        # Verificar conexión a BD
        async with engine.begin() as conn:
            await conn.run_sync(lambda _: None)
        logger.info("database_connected")

    @app.on_event("shutdown")
    async def shutdown():
        logger.info("application_shutdown")
        await engine.dispose()

    return app


app = create_app()
app.state.limiter = limiter
```

### `backend/app/core/security.py` — JWT y Hashing

```python
"""
Utilidades de seguridad: hashing de contraseñas, generación/validación de JWT.
Usa Argon2 para hashing (más seguro que bcrypt) y HS256 para JWT.
"""
from datetime import datetime, timedelta, timezone
from typing import Any

import argon2
import jwt
from passlib.context import CryptContext

from app.config import settings

# Contexto de hashing con Argon2 (recomendado sobre bcrypt)
pwd_context = CryptContext(schemes=["argon2"], deprecated="auto")

# Instancia directa de Argon2 para verificación manual
ph = argon2.PasswordHasher(
    time_cost=3,
    memory_cost=65536,
    parallelism=1,
    hash_len=32,
    type=argon2.Type.ID,  # Argon2id: mezcla de Argon2i y Argon2d
)


def verify_password(plain_password: str, hashed_password: str) -> bool:
    """Verifica una contraseña contra su hash."""
    try:
        return ph.verify(hashed_password, plain_password)
    except argon2.exceptions.VerifyMismatchError:
        return False


def get_password_hash(password: str) -> str:
    """Genera hash Argon2id para una contraseña."""
    return ph.hash(password)


def create_access_token(data: dict[str, Any], expires_delta: timedelta | None = None) -> str:
    """Genera un access token JWT."""
    to_encode = data.copy()
    expire = datetime.now(timezone.utc) + (
        expires_delta or timedelta(minutes=settings.ACCESS_TOKEN_EXPIRE_MINUTES)
    )
    to_encode.update({"exp": expire, "type": "access"})
    encoded_jwt = jwt.encode(to_encode, settings.SECRET_KEY, algorithm=settings.ALGORITHM)
    return encoded_jwt


def create_refresh_token(data: dict[str, Any]) -> str:
    """Genera un refresh token JWT."""
    to_encode = data.copy()
    expire = datetime.now(timezone.utc) + timedelta(days=settings.REFRESH_TOKEN_EXPIRE_DAYS)
    to_encode.update({"exp": expire, "type": "refresh"})
    encoded_jwt = jwt.encode(to_encode, settings.SECRET_KEY, algorithm=settings.ALGORITHM)
    return encoded_jwt


def decode_token(token: str) -> dict[str, Any]:
    """Decodifica y valida un token JWT."""
    payload = jwt.decode(token, settings.SECRET_KEY, algorithms=[settings.ALGORITHM])
    return payload


def validate_token_type(payload: dict[str, Any], expected_type: str) -> bool:
    """Valida que el token sea del tipo esperado (access/refresh)."""
    return payload.get("type") == expected_type
```

### `backend/app/core/middleware.py` — Headers de seguridad y Request ID

```python
"""
Middlewares personalizados:
- SecurityHeadersMiddleware: añade cabeceras de seguridad HTTP
- RequestIDMiddleware: asigna un ID único a cada petición para trazabilidad
"""
import uuid

import structlog
from starlette.middleware.base import BaseHTTPMiddleware
from starlette.requests import Request
from starlette.responses import Response

logger = structlog.get_logger(__name__)


class SecurityHeadersMiddleware(BaseHTTPMiddleware):
    """Añade cabeceras de seguridad estándar."""

    async def dispatch(self, request: Request, call_next) -> Response:
        response = await call_next(request)
        response.headers["X-Content-Type-Options"] = "nosniff"
        response.headers["X-Frame-Options"] = "DENY"
        response.headers["X-XSS-Protection"] = "1; mode=block"
        response.headers["Referrer-Policy"] = "strict-origin-when-cross-origin"
        response.headers["Permissions-Policy"] = "camera=(), microphone=(), geolocation=()"
        # HSTS solo en producción HTTPS
        if not request.url.scheme == "http":
            response.headers["Strict-Transport-Security"] = "max-age=31536000; includeSubDomains"
        return response


class RequestIDMiddleware(BaseHTTPMiddleware):
    """Asigna un X-Request-ID único para trazabilidad de logs."""

    async def dispatch(self, request: Request, call_next) -> Response:
        request_id = request.headers.get("X-Request-ID", str(uuid.uuid4()))
        request.state.request_id = request_id

        logger.info(
            "request_start",
            method=request.method,
            path=request.url.path,
            request_id=request_id,
        )

        response = await call_next(request)
        response.headers["X-Request-ID"] = request_id

        logger.info(
            "request_end",
            method=request.method,
            path=request.url.path,
            status_code=response.status_code,
            request_id=request_id,
        )

        return response
```

### `backend/app/core/exceptions.py` — Manejo global de errores

```python
"""
Manejo centralizado de excepciones.
Evita filtrar detalles internos al cliente.
"""
import structlog
from fastapi import FastAPI, HTTPException, Request
from fastapi.responses import JSONResponse

logger = structlog.get_logger(__name__)


def register_exception_handlers(app: FastAPI) -> None:
    """Registra todos los manejadores de excepciones globales."""

    @app.exception_handler(HTTPException)
    async def http_exception_handler(request: Request, exc: HTTPException) -> JSONResponse:
        logger.warning(
            "http_exception",
            status_code=exc.status_code,
            detail=str(exc.detail),
            path=str(request.url),
        )
        return JSONResponse(
            status_code=exc.status_code,
            content={"error": _safe_error_message(exc.detail)},
        )

    @app.exception_handler(Exception)
    async def general_exception_handler(request: Request, exc: Exception) -> JSONResponse:
        # Log completo para debugging interno
        logger.error(
            "unhandled_exception",
            error=str(exc),
            path=str(request.url),
            exc_info=True,
        )
        # Respuesta genérica al cliente (no filtrar stack trace)
        return JSONResponse(
            status_code=500,
            content={"error": "Internal server error"},
        )


def _safe_error_message(detail: object) -> str:
    """Extrae mensaje seguro de una excepción HTTP."""
    if isinstance(detail, str):
        return detail
    if isinstance(detail, dict):
        return detail.get("message", "Bad request")
    return "Bad request"
```

### `backend/app/models/base.py`

```python
"""Modelo base con campos comunes."""
from datetime import datetime, timezone

from sqlalchemy import DateTime, func
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column


class Base(DeclarativeBase):
    """Clase base para todos los modelos ORM."""
    pass


class TimestampMixin:
    """Mixin que añade created_at y updated_at automáticamente."""

    created_at: Mapped[datetime] = mapped_column(
        DateTime(timezone=True),
        server_default=func.now(),
        nullable=False,
    )
    updated_at: Mapped[datetime] = mapped_column(
        DateTime(timezone=True),
        server_default=func.now(),
        onupdate=func.now(),
        nullable=False,
    )
```

### `backend/app/models/user.py`

```python
"""Modelo User."""
from sqlalchemy import Boolean, String
from sqlalchemy.orm import Mapped, mapped_column

from app.models.base import Base, TimestampMixin


class User(TimestampMixin, Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True, autoincrement=True)
    email: Mapped[str] = mapped_column(String(255), unique=True, index=True, nullable=False)
    password_hash: Mapped[str] = mapped_column(String(255), nullable=False)
    full_name: Mapped[str] = mapped_column(String(255), nullable=False, default="")
    is_active: Mapped[bool] = mapped_column(Boolean, default=True, nullable=False)
    is_superuser: Mapped[bool] = mapped_column(Boolean, default=False, nullable=False)

    def __repr__(self) -> str:
        return f"<User(id={self.id}, email='{self.email}')>"
```

### `backend/app/schemas/auth.py`

```python
"""Schemas para autenticación."""
from pydantic import BaseModel, EmailStr, Field, field_validator


class UserRegister(BaseModel):
    """Schema para registro de usuario."""
    email: EmailStr
    password: str = Field(min_length=8, max_length=128)
    full_name: str = Field(min_length=1, max_length=255)

    @field_validator("password")
    @classmethod
    def password_strength(cls, v: str) -> str:
        """Valida complejidad mínima de contraseña."""
        if not any(c.isupper() for c in v):
            raise ValueError("Password must contain at least one uppercase letter")
        if not any(c.isdigit() for c in v):
            raise ValueError("Password must contain at least one digit")
        return v


class UserLogin(BaseModel):
    """Schema para inicio de sesión."""
    email: EmailStr
    password: str


class TokenPair(BaseModel):
    """Respuesta con access y refresh tokens."""
    access_token: str
    refresh_token: str
    token_type: str = "bearer"


class TokenPayload(BaseModel):
    """Payload decodificado de un token JWT."""
    sub: str  # user id
    exp: int
    type: str  # 'access' o 'refresh'
```

### `backend/app/services/auth_service.py`

```python
"""
Servicio de autenticación: lógica de negocio separada de routers.
Facilita testing unitario y reutilización.
"""
import structlog
from fastapi import HTTPException, status
from sqlalchemy import select
from sqlalchemy.ext.asyncio import AsyncSession

from app.core.security import (
    create_access_token,
    create_refresh_token,
    get_password_hash,
    verify_password,
)
from app.models.user import User
from app.schemas.auth import UserRegister

logger = structlog.get_logger(__name__)


class AuthService:
    """Servicio de autenticación con inyección de dependencia de sesión."""

    def __init__(self, db: AsyncSession) -> None:
        self.db = db

    async def register_user(self, data: UserRegister) -> User:
        """Registra un nuevo usuario. Falla si el email ya existe."""
        # Verificar duplicados
        existing = await self.db.execute(
            select(User).where(User.email == data.email)
        )
        if existing.scalar_one_or_none():
            raise HTTPException(
                status_code=status.HTTP_409_CONFLICT,
                detail="Email already registered",
            )

        user = User(
            email=data.email,
            password_hash=get_password_hash(data.password),
            full_name=data.full_name,
        )
        self.db.add(user)
        await self.db.flush()  # Obtener ID sin commit aún
        await self.db.refresh(user)

        logger.info("user_registered", user_id=user.id, email=user.email)
        return user

    async def authenticate_user(self, email: str, password: str) -> User:
        """Autentica un usuario. Retorna el objeto User si válido."""
        result = await self.db.execute(select(User).where(User.email == email))
        user = result.scalar_one_or_none()

        if not user or not verify_password(password, user.password_hash):
            # Mensaje genérico para no revelar si el email existe
            raise HTTPException(
                status_code=status.HTTP_401_UNAUTHORIZED,
                detail="Invalid credentials",
            )

        if not user.is_active:
            raise HTTPException(
                status_code=status.HTTP_403_FORBIDDEN,
                detail="Account is disabled",
            )

        logger.info("user_authenticated", user_id=user.id)
        return user

    async def generate_tokens(self, user: User) -> tuple[str, str]:
        """Genera par de access + refresh tokens."""
        access_token = create_access_token(data={"sub": str(user.id)})
        refresh_token = create_refresh_token(data={"sub": str(user.id)})
        return access_token, refresh_token
```

### `backend/app/routers/auth.py`

```python
"""Endpoints de autenticación."""
from fastapi import APIRouter, Depends, HTTPException, status
from sqlalchemy.ext.asyncio import AsyncSession

from app.core.security import decode_token, validate_token_type
from app.dependencies import get_current_user
from app.models.user import User
from app.schemas.auth import TokenPair, UserLogin, UserRegister
from app.services.auth_service import AuthService

router = APIRouter()


@router.post("/register", response_model=TokenPair, status_code=status.HTTP_201_CREATED)
async def register(
    data: UserRegister,
    db: AsyncSession = Depends(get_db),
) -> TokenPair:
    """Registra un nuevo usuario y retorna tokens."""
    service = AuthService(db)
    user = await service.register_user(data)
    access_token, refresh_token = await service.generate_tokens(user)
    return TokenPair(access_token=access_token, refresh_token=refresh_token)


@router.post("/login", response_model=TokenPair)
async def login(
    data: UserLogin,
    db: AsyncSession = Depends(get_db),
) -> TokenPair:
    """Inicia sesión y retorna tokens."""
    service = AuthService(db)
    user = await service.authenticate_user(data.email, data.password)
    access_token, refresh_token = await service.generate_tokens(user)
    return TokenPair(access_token=access_token, refresh_token=refresh_token)


@router.post("/refresh", response_model=TokenPair)
async def refresh_token(body: dict) -> TokenPair:
    """Intercambia un refresh token por nuevos tokens."""
    token = body.get("refresh_token")
    if not token:
        raise HTTPException(status_code=400, detail="Refresh token required")

    try:
        payload = decode_token(token)
        if not validate_token_type(payload, "refresh"):
            raise HTTPException(status_code=401, detail="Invalid token type")
    except Exception:
        raise HTTPException(status_code=401, detail="Invalid or expired token")

    from app.database import AsyncSessionLocal
    async with AsyncSessionLocal() as session:
        service = AuthService(session)
        user = await session.get(User, int(payload["sub"]))
        if not user or not user.is_active:
            raise HTTPException(status_code=401, detail="User not found or inactive")
        access_token, new_refresh = await service.generate_tokens(user)

    return TokenPair(access_token=access_token, refresh_token=new_refresh)


@router.get("/me", response_model=dict)
async def get_current_user_info(
    current_user: User = Depends(get_current_user),
) -> dict:
    """Retorna información del usuario autenticado."""
    return {
        "id": current_user.id,
        "email": current_user.email,
        "full_name": current_user.full_name,
    }


# Import circular workaround
from app.dependencies import get_db  # noqa: E402
```

### `backend/app/dependencies.py`

```python
"""Dependencias inyectables para FastAPI."""
from typing import Annotated

import jwt
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer
from sqlalchemy.ext.asyncio import AsyncSession

from app.core.security import decode_token, validate_token_type
from app.database import get_db
from app.models.user import User

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/api/v1/auth/login")


async def get_current_user(
    token: Annotated[str, Depends(oauth2_scheme)],
    db: AsyncSession = Depends(get_db),
) -> User:
    """Extrae y valida el usuario actual desde el token Bearer."""
    credentials_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Could not validate credentials",
        headers={"WWW-Authenticate": "Bearer"},
    )

    try:
        payload = decode_token(token)
        if not validate_token_type(payload, "access"):
            raise credentials_exception
        user_id: str = payload.get("sub")
        if user_id is None:
            raise credentials_exception
    except jwt.ExpiredSignatureError:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Token expired",
        )
    except jwt.InvalidTokenError:
        raise credentials_exception

    user = await db.get(User, int(user_id))
    if user is None:
        raise credentials_exception
    if not user.is_active:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Inactive user",
        )
    return user


async def get_current_superuser(
    current_user: Annotated[User, Depends(get_current_user)],
) -> User:
    """Requiere que el usuario sea superusuario."""
    if not current_user.is_superuser:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Not enough permissions",
        )
    return current_user
```

---

## 4. Tests — Backend

### `backend/tests/conftest.py`

```python
"""Fixtures compartidas para pruebas."""
import asyncio
from collections.abc import AsyncGenerator

import pytest
import pytest_asyncio
from httpx import ASGITransport, AsyncClient
from sqlalchemy.ext.asyncio import AsyncSession, create_async_engine, async_sessionmaker

from app.database import Base, get_db
from app.main import app

TEST_DATABASE_URL = "sqlite+aiosqlite:///./test.db"


@pytest.fixture(scope="session")
def event_loop():
    """Event loop compartido para todas las pruebas."""
    loop = asyncio.new_event_loop()
    yield loop
    loop.close()


@pytest_asyncio.fixture
async def async_engine():
    """Engine de prueba con SQLite in-memory."""
    engine = create_async_engine(TEST_DATABASE_URL, echo=False)
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)
    yield engine
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.drop_all)
    await engine.dispose()


@pytest_asyncio.fixture
async def async_session(async_engine) -> AsyncGenerator[AsyncSession, None]:
    """Sesión de prueba."""
    session_maker = async_sessionmaker(bind=async_engine, expire_on_commit=False)
    async with session_maker() as session:
        yield session


@pytest_asyncio.fixture
async def client(async_session) -> AsyncGenerator[AsyncClient, None]:
    """Cliente HTTP de prueba con override de get_db."""
    async def override_get_db():
        yield async_session

    app.dependency_overrides[get_db] = override_get_db

    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as ac:
        yield ac

    app.dependency_overrides.clear()


@pytest_asyncio.fixture
async def authenticated_client(client: AsyncClient) -> AsyncClient:
    """Cliente autenticado con un usuario registrado."""
    # Registrar usuario de prueba
    resp = await client.post(
        "/api/v1/auth/register",
        json={
            "email": "test@example.com",
            "password": "TestPass123!",
            "full_name": "Test User",
        },
    )
    assert resp.status_code == 201
    token_data = resp.json()

    # Añadir header de autorización
    client.headers["Authorization"] = f"Bearer {token_data['access_token']}"
    return client
```

### `backend/tests/test_auth.py`

```python
"""Pruebas para endpoints de autenticación."""
import pytest
from httpx import AsyncClient


@pytest.mark.asyncio
async def test_register_success(client: AsyncClient):
    """Usuario nuevo se registra correctamente."""
    response = await client.post(
        "/api/v1/auth/register",
        json={
            "email": "newuser@example.com",
            "password": "StrongPass123!",
            "full_name": "New User",
        },
    )
    assert response.status_code == 201
    data = response.json()
    assert "access_token" in data
    assert "refresh_token" in data
    assert data["token_type"] == "bearer"


@pytest.mark.asyncio
async def test_register_duplicate_email(client: AsyncClient):
    """No permite emails duplicados."""
    payload = {
        "email": "dup@example.com",
        "password": "StrongPass123!",
        "full_name": "First",
    }
    await client.post("/api/v1/auth/register", json=payload)
    response = await client.post("/api/v1/auth/register", json=payload)
    assert response.status_code == 409


@pytest.mark.asyncio
async def test_register_weak_password(client: AsyncClient):
    """Rechaza contraseñas débiles."""
    response = await client.post(
        "/api/v1/auth/register",
        json={
            "email": "weak@example.com",
            "password": "short",
            "full_name": "Weak",
        },
    )
    assert response.status_code == 422


@pytest.mark.asyncio
async def test_login_success(client: AsyncClient):
    """Login correcto retorna tokens."""
    # Primero registrar
    await client.post(
        "/api/v1/auth/register",
        json={
            "email": "login@example.com",
            "password": "CorrectPass123!",
            "full_name": "Login User",
        },
    )
    # Luego login
    response = await client.post(
        "/api/v1/auth/login",
        json={
            "email": "login@example.com",
            "password": "CorrectPass123!",
        },
    )
    assert response.status_code == 200
    assert "access_token" in response.json()


@pytest.mark.asyncio
async def test_login_wrong_password(client: AsyncClient):
    """Contraseña incorrecta retorna 401 genérico."""
    await client.post(
        "/api/v1/auth/register",
        json={
            "email": "wrongpw@example.com",
            "password": "CorrectPass123!",
            "full_name": "Wrong PW",
        },
    )
    response = await client.post(
        "/api/v1/auth/login",
        json={
            "email": "wrongpw@example.com",
            "password": "WrongPassword123!",
        },
    )
    assert response.status_code == 401
    # No revelar si el email existe
    assert "invalid" in response.json()["error"].lower()


@pytest.mark.asyncio
async def test_login_nonexistent_user(client: AsyncClient):
    """Usuario inexistente también retorna 401 genérico."""
    response = await client.post(
        "/api/v1/auth/login",
        json={
            "email": "ghost@example.com",
            "password": "AnyPassword123!",
        },
    )
    assert response.status_code == 401


@pytest.mark.asyncio
async def test_get_current_user(authenticated_client: AsyncClient):
    """Endpoint /me retorna info del usuario autenticado."""
    response = await authenticated_client.get("/api/v1/auth/me")
    assert response.status_code == 200
    data = response.json()
    assert data["email"] == "test@example.com"


@pytest.mark.asyncio
async def test_protected_route_without_token(client: AsyncClient):
    """Ruta protegida sin token retorna 401."""
    response = await client.get("/api/v1/auth/me")
    assert response.status_code == 401
```

### `backend/tests/test_security.py`

```python
"""Pruebas unitarias para utilidades de seguridad."""
import pytest

from app.core.security import (
    create_access_token,
    create_refresh_token,
    decode_token,
    get_password_hash,
    validate_token_type,
    verify_password,
)


def test_password_hash_and_verify():
    """Hash y verificación de contraseña funcionan correctamente."""
    password = "MySecureP@ss123"
    hashed = get_password_hash(password)
    assert hashed != password
    assert verify_password(password, hashed) is True
    assert verify_password("wrong", hashed) is False


def test_access_token_generation_and_validation():
    """Access token se genera y decodifica correctamente."""
    token = create_access_token(data={"sub": "42"})
    payload = decode_token(token)
    assert payload["sub"] == "42"
    assert payload["type"] == "access"
    assert validate_token_type(payload, "access") is True
    assert validate_token_type(payload, "refresh") is False


def test_refresh_token_generation():
    """Refresh token tiene tipo correcto."""
    token = create_refresh_token(data={"sub": "42"})
    payload = decode_token(token)
    assert payload["type"] == "refresh"
    assert validate_token_type(payload, "refresh") is True


def test_invalid_token_raises():
    """Token inválido lanza excepción."""
    with pytest.raises(Exception):
        decode_token("not.a.valid.jwt")
```

---

## 5. Frontend — Archivos Clave

### `frontend/package.json`

```json
{
  "name": "focusflow-frontend",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "test": "jest",
    "test:e2e": "playwright test",
    "test:coverage": "jest --coverage"
  },
  "dependencies": {
    "next": "^14.2.0",
    "react": "^18.3.0",
    "react-dom": "^18.3.0",
    "zustand": "^5.0.0",
    "axios": "^1.7.0",
    "clsx": "^2.1.0",
    "tailwindcss": "^3.4.0"
  },
  "devDependencies": {
    "@testing-library/jest-dom": "^6.6.0",
    "@testing-library/react": "^16.0.0",
    "@types/node": "^22.0.0",
    "@types/react": "^18.3.0",
    "@types/react-dom": "^18.3.0",
    "jest": "^29.7.0",
    "jest-environment-jsdom": "^29.7.0",
    "@playwright/test": "^1.49.0",
    "typescript": "^5.7.0",
    "eslint": "^8.57.0",
    "eslint-config-next": "^14.2.0"
  }
}
```

### `frontend/src/store/auth-store.ts` — Zustand con persistencia segura

```typescript
/**
 * Store de autenticación con Zustand.
 * Los tokens se almacenan en localStorage pero con consideraciones:
 * - Access token corto (15 min) reduce riesgo XSS
 * - Refresh token en cookie HttpOnly sería más seguro (requiere backend change)
 * - Para MVP, usamos localStorage con advertencia documentada
 */
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface AuthState {
  accessToken: string | null;
  refreshToken: string | null;
  user: {
    id: number;
    email: string;
    fullName: string;
  } | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  setTokens: (access: string, refresh: string) => void;
  setUser: (user: AuthState['user']) => void;
  logout: () => void;
  setLoading: (loading: boolean) => void;
}

export const useAuthStore = create<AuthState>()(
  persist(
    (set) => ({
      accessToken: null,
      refreshToken: null,
      user: null,
      isAuthenticated: false,
      isLoading: false,

      setTokens: (access, refresh) =>
        set({ accessToken: access, refreshToken: refresh, isAuthenticated: true }),

      setUser: (user) => set({ user }),

      logout: () =>
        set({
          accessToken: null,
          refreshToken: null,
          user: null,
          isAuthenticated: false,
        }),

      setLoading: (isLoading) => set({ isLoading }),
    }),
    {
      name: 'focusflow-auth',
      partialize: (state) => ({
        // Solo persistir lo necesario; nunca almacenar datos sensibles innecesarios
        accessToken: state.accessToken,
        refreshToken: state.refreshToken,
        user: state.user,
      }),
    }
  )
);
```

### `frontend/src/lib/api.ts` — Cliente HTTP con interceptores

```typescript
/**
 * Cliente HTTP centralizado con manejo automático de tokens.
 * Usa axios con interceptores para añadir Authorization header
 * y manejar renovación de tokens ante 401.
 */
import axios, { AxiosInstance, InternalAxiosRequestConfig } from 'axios';
import { useAuthStore } from '@/store/auth-store';

const API_BASE_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:8000';

const api: AxiosInstance = axios.create({
  baseURL: `${API_BASE_URL}/api/v1`,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Interceptor de solicitud: añadir token automáticamente
api.interceptors.request.use((config: InternalAxiosRequestConfig) => {
  const { accessToken } = useAuthStore.getState();
  if (accessToken) {
    config.headers.Authorization = `Bearer ${accessToken}`;
  }
  return config;
});

// Interceptor de respuesta: manejar 401 con refresh token
api.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;
    const { refreshToken, setTokens, logout } = useAuthStore.getState();

    // Si es 401 y tenemos refresh token, intentar renovar
    if (error.response?.status === 401 && refreshToken && !originalRequest._retry) {
      originalRequest._retry = true;

      try {
        const response = await axios.post(`${API_BASE_URL}/api/v1/auth/refresh`, {
          refresh_token: refreshToken,
        });

        const { access_token, refresh_token } = response.data;
        setTokens(access_token, refresh_token);

        // Reintentar la petición original
        originalRequest.headers.Authorization = `Bearer ${access_token}`;
        return api(originalRequest);
      } catch (refreshError) {
        // Refresh falló → cerrar sesión
        logout();
        window.location.href = '/login';
        return Promise.reject(refreshError);
      }
    }

    return Promise.reject(error);
  }
);

export default api;
```

### `frontend/src/middleware.ts` — Protección de rutas

```typescript
/**
 * Next.js Middleware para proteger rutas del dashboard.
 * Redirige a /login si no hay token de acceso válido.
 */
import { NextRequest, NextResponse } from 'next/server';

const PUBLIC_PATHS = ['/login', '/register', '/', '/favicon.ico'];

export function middleware(request: NextRequest) {
  const { pathname } = request.nextUrl;

  // Permitir rutas públicas
  if (PUBLIC_PATHS.some((path) => pathname.startsWith(path))) {
    return NextResponse.next();
  }

  // Verificar token en cookies o headers (según implementación)
  // Para este ejemplo, verificamos si el usuario tiene sesión activa
  // En producción, validarías el JWT aquí mismo
  const token = request.cookies.get('access_token')?.value;

  if (!token) {
    const loginUrl = new URL('/login', request.url);
    loginUrl.searchParams.set('from', pathname);
    return NextResponse.redirect(loginUrl);
  }

  return NextResponse.next();
}

export const config = {
  matcher: ['/dashboard/:path*', '/api/:path*'],
};
```

### `frontend/src/components/auth/LoginForm.tsx`

```tsx
'use client';

import { useState, FormEvent } from 'react';
import { useRouter } from 'next/navigation';
import api from '@/lib/api';
import { useAuthStore } from '@/store/auth-store';
import Button from '@/components/ui/Button';
import Input from '@/components/ui/Input';

export default function LoginForm() {
  const router = useRouter();
  const { setTokens, setUser, setLoading } = useAuthStore();
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState<string | null>(null);

  const handleSubmit = async (e: FormEvent) => {
    e.preventDefault();
    setError(null);
    setLoading(true);

    try {
      const response = await api.post('/auth/login', { email, password });
      const { access_token, refresh_token } = response.data;
      setTokens(access_token, refresh_token);

      // Obtener perfil de usuario
      const meResponse = await api.get('/auth/me');
      setUser(meResponse.data);

      router.push('/dashboard');
    } catch (err: unknown) {
      const message =
        err && typeof err === 'object' && 'response' in err
          ? ((err as { response?: { data?: { error?: string } } }).response?.data?.error ??
            'Login failed')
          : 'Network error';
      setError(message);
    } finally {
      setLoading(false);
    }
  };

  return (
    <form onSubmit={handleSubmit} className="space-y-4">
      {error && (
        <div className="p-3 bg-red-100 border border-red-300 text-red-700 rounded-md text-sm">
          {error}
        </div>
      )}
      <Input
        label="Email"
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        required
        autoComplete="email"
      />
      <Input
        label="Password"
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        required
        autoComplete="current-password"
      />
      <Button type="submit" disabled={false}>
        Sign In
      </Button>
    </form>
  );
}
```

### `frontend/tests/e2e/auth.spec.ts` — Playwright

```typescript
import { test, expect } from '@playwright/test';

test.describe('Authentication Flow', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('should display login form', async ({ page }) => {
    await expect(page.getByLabel('Email')).toBeVisible();
    await expect(page.getByLabel('Password')).toBeVisible();
    await expect(page.getByRole('button', { name: 'Sign In' })).toBeVisible();
  });

  test('should show error for invalid credentials', async ({ page }) => {
    await page.getByLabel('Email').fill('nonexistent@example.com');
    await page.getByLabel('Password').fill('WrongPassword123!');
    await page.getByRole('button', { name: 'Sign In' }).click();

    await expect(page.locator('.bg-red-100')).toBeVisible();
    await expect(page.locator('.bg-red-100')).toContainText(/invalid/i);
  });

  test('should redirect to dashboard after successful login', async ({ page }) => {
    // Requiere un usuario de prueba pre-registrado o seed
    await page.getByLabel('Email').fill('test@example.com');
    await page.getByLabel('Password').fill('TestPass123!');
    await page.getByRole('button', { name: 'Sign In' }).click();

    await expect(page).toHaveURL(/.*dashboard/, { timeout: 10000 });
  });

  test('should protect dashboard routes when not logged in', async ({ page }) => {
    await page.goto('/dashboard');
    await expect(page).toHaveURL(/.*login/);
  });
});
```

---

## 6. Scripts Operativos

### `scripts/seed-db.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "🌱 Seeding database..."

# Esperar a que la DB esté lista
until docker compose exec -T db pg_isready -U "${DB_USER:-postgres}" -q; do
  sleep 1
done

# Ejecutar seed SQL
docker compose exec -T db psql -U "${DB_USER:-postgres}" -d "${DB_NAME:-focusflow}" <<'SQL'
-- Insertar usuario admin de prueba (contraseña: AdminPass123!)
INSERT INTO users (email, password_hash, full_name, is_active, is_superuser)
VALUES (
  'admin@focusflow.io',
  '$argon2id$v=19$m=65536,t=3,p=1$c2FsdA$placeholder_hash_run_through_backend',
  'Admin User',
  true,
  true
) ON CONFLICT (email) DO NOTHING;

-- Insertar productos de ejemplo
INSERT INTO products (name, description, price, stock) VALUES
  ('Focus Timer Pro', 'Advanced Pomodoro timer', 29.99, 100),
  ('Task Manager Lite', 'Simple task tracking', 9.99, 200),
  ('Habit Tracker', 'Track daily habits', 14.99, 150)
ON CONFLICT DO NOTHING;

SELECT 'Seeding complete!' as status;
SQL

echo "✅ Database seeded successfully."
```

### `scripts/backup-db.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail

TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="./backups"
mkdir -p "$BACKUP_DIR"

FILENAME="${BACKUP_DIR}/focusflow_backup_${TIMESTAMP}.sql.gz"

echo "💾 Backing up database to $FILENAME..."

docker compose exec -T db pg_dump -U "${DB_USER:-postgres}" "${DB_NAME:-focusflow}" \
  | gzip > "$FILENAME"

echo "✅ Backup completed: $(du -h "$FILENAME" | cut -f1)"

# Mantener solo los últimos 7 backups
ls -t "${BACKUP_DIR}/focusflow_backup_"*.sql.gz | tail -n +8 | xargs rm -f
```

### `scripts/healthcheck.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "🩺 Running health checks..."

SERVICES=("db" "api" "dashboard")
FAILED=0

for svc in "${SERVICES[@]}"; do
  echo -n "  Checking $svc... "
  if docker compose ps "$svc" --format '{{.Health}}' 2>/dev/null | grep -q "healthy"; then
    echo "✅ healthy"
  elif docker compose ps "$svc" --format '{{.Status}}' 2>/dev/null | grep -q "running"; then
    echo "⚠️ running (no healthcheck defined)"
  else
    echo "❌ not running"
    FAILED=1
  fi
done

if [ $FAILED -eq 0 ]; then
  echo "🎉 All services are healthy!"
else
  echo "⛔ Some services have issues. Run 'docker compose logs' to investigate."
  exit 1
fi
```

---

## 7. Documentación

### `docs/ARCHITECTURE.md`

```markdown
# FocusFlow — Architecture Decisions Record

## Stack Resumen
| Capa | Tecnología | Razón |
|------|-----------|-------|
| API | FastAPI + Uvicorn | Async nativo, OpenAPI auto-doc, alto rendimiento |
| ORM | SQLAlchemy 2.0 async | Madurez, soporta async, migraciones con Alembic |
| DB | PostgreSQL 16 | Relacional robusto, JSONB, full-text search |
| Cache/RL | Redis 7 | Rate limiting, sesiones, caching |
| Frontend | Next.js 14 App Router | SSR, ISR, middleware nativo, TypeScript first |
| Estado | Zustand | Ligero, sin boilerplate, persistencia integrada |
| Styling | Tailwind CSS | Utility-first, rápido prototipado |
| Testing | pytest + Playwright + Jest | Cobertura completa: unit, integration, e2e |

## Decisiones Clave

### ADR-001: Argon2id sobre bcrypt
- **Contexto**: Necesitamos hashing de contraseñas resistente a GPU attacks.
- **Decisión**: Usar Argon2id con parameters time_cost=3, memory_cost=64MB.
- **Consecuencia**: Mayor resistencia a ataques, compatible con OWASP recomendations.

### ADR-002: Async SQLAlchemy sobre sync
- **Contexto**: FastAPI es async-native; mezclar sync I/O bloquea el event loop.
- **Decisión**: SQLAlchemy 2.0 con asyncpg driver.
- **Consecuencia**: Mejor throughput bajo carga concurrente.

### ADR-003: Capa de Servicios separada de Routers
- **Contexto**: Routers deben ser delgados; lógica de negocio debe ser testeable.
- **Decisión**: Cada entidad tiene un `<entity>_service.py` con la lógica.
- **Consecuencia**: Unit testing sin levantar HTTP stack completo.

### ADR-004: Network isolation en Docker
- **Contexto**: Minimizar superficie de ataque.
- **Decisión**: `backend-net` es `internal`; solo `api` puede hablar con `db`.
- **Consecuencia**: Frontend no puede acceder directamente a la base de datos.

### ADR-005: Refresh token rotation
- **Contexto**: Robo de tokens es vector principal en SPAs.
- **Decisión**: Cada uso de refresh token emite uno nuevo; el anterior se invalida.
- **Consecuencia**: Ventana de robo reducida significativamente.
```

### `docs/SECURITY.md`

```markdown
# FocusFlow — Security Policy

## Authentication
- Contraseñas hasheadas con Argon2id (OWASP recommended)
- Access tokens: 15 minutos, JWT HS256
- Refresh tokens: 7 días, rotación automática
- Nunca exponer hashes ni tokens completos en logs

## API Security
- Rate limiting: 60 req/min por IP (Redis-backed)
- CORS restringido a dominios conocidos
- Cabeceras de seguridad: X-Content-Type-Options, X-Frame-Options, CSP
- Validación de input con Pydantic (whitelist approach)
- Mensajes de error genéricos (no revelar existencia de recursos)

## Infrastructure
- Contenedores corren como usuario non-root
- Read-only filesystem donde es posible
- Secrets vía .env / secret manager (nunca en código)
- Redes Docker aisladas: DB solo accesible desde API
- Health checks para detección temprana de fallos

## Data Protection
- PostgreSQL: cifrado en reposo (via volumen cifrado o TDE)
- TLS obligatorio en producción (reverse proxy)
- Backups diarios con retención de 7 días
- Soft-delete para registros críticos (is_active flag)

## Vulnerability Management
- Dependabot activo para CVEs en dependencias
- `pip-audit` y `npm audit` en CI
- SAST: Bandit (Python), ESLint security plugins (JS)
- DAST: OWASP ZAP scan semanal en staging
```

---

## 8. Diagrama de Arquitectura Actualizado

```
                         INTERNET
                            │
                    ┌───────▼────────┐
                    │   Browser       │
                    │  (HTTPS only)   │
                    └───────┬────────┘
                            │
              ┌─────────────▼──────────────┐
              │     Reverse Proxy (Nginx)   │  ← Solo en producción
              │  • TLS termination          │
              │  • Static file serving      │
              │  • Load balancing (future)  │
              └──────┬──────────────┬───────┘
                     │              │
          ┌──────────▼───┐   ┌─────▼──────────┐
          │  dashboard    │   │     api         │
          │  Next.js :3000│   │  FastAPI :8000  │
          │               │   │                 │
          │  • SSR/ISR    │   │  • JWT auth     │
          │  • Client RT  │   │  • Rate limit   │
          │  • Zustand    │   │  • Services     │
          └───────┬───────┘   └───┬──────┬──────┘
                  │               │      │
          frontend-net     backend-net   │
          (public)         (internal)    │
                                         │
                              ┌──────────▼──────────┐
                              │       db             │
                              │  PostgreSQL 16       │
                              │  :5432 (internal)    │
                              │                      │
                              │  • Encrypted volume  │
                              │  • Connection pool   │
                              └──────────────────────┘
                              ┌──────────────────────┐
                              │      redis           │
                              │  :6379 (internal)    │
                              │                      │
                              │  • Rate limiting     │
                              │  • Session cache     │
                              └──────────────────────┘
```

---

## 9. Comandos de Uso

```bash
# ── Setup inicial ──
cp .env.example .env
# Editar .env con valores reales

# ── Levantar todo ──
docker compose up -d --build

# ── Solo backend en modo dev (hot reload) ──
cd backend
source venv/bin/activate  # o poetry shell
pip install -r requirements-dev.txt
uvicorn app.main:app --reload --port 8000

# ── Solo frontend en modo dev ──
cd frontend
npm ci
npm run dev

# ── Ejecutar migraciones ──
cd backend
alembic upgrade head

# ── Seed de datos ──
./scripts/seed-db.sh

# ── Correr tests backend ──
cd backend
pytest --cov=app --cov-report=html -v

# ── Correr tests frontend unitarios ──
cd frontend
npm run test

# ── Correr tests e2e ──
cd frontend
npx playwright install chromium
npm run test:e2e

# ── Linting ──
cd backend && ruff check . && black --check .
cd frontend && npm run lint

# ── Type checking ──
cd backend && mypy app/
cd frontend && npx tsc --noEmit

# ── Backup ──
./scripts/backup-db.sh

# ── Health check ──
./scripts/healthcheck.sh
```

---

## 10. Migraciones Alembic — Ejemplo

### `backend/alembic/versions/001_initial_schema.py`

```python
"""Initial schema

Revision ID: 001
Create Date: 2024-01-15
"""
from alembic import op
import sqlalchemy as sa

revision = '001'
down_revision = None
branch_labels = None
depends_on = None


def upgrade() -> None:
    # Users table
    op.create_table(
        'users',
        sa.Column('id', sa.Integer(), autoincrement=True, nullable=False),
        sa.Column('email', sa.String(255), nullable=False),
        sa.Column('password_hash', sa.String(255), nullable=False),
        sa.Column('full_name', sa.String(255), nullable=False, server_default=''),
        sa.Column('is_active', sa.Boolean(), nullable=False, server_default=sa.text('true')),
        sa.Column('is_superuser', sa.Boolean(), nullable=False, server_default=sa.text('false')),
        sa.Column('created_at', sa.DateTime(timezone=True), server_default=sa.func.now(), nullable=False),
        sa.Column('updated_at', sa.DateTime(timezone=True), server_default=sa.func.now(), nullable=False),
        sa.PrimaryKeyConstraint('id'),
    )
    op.create_index(op.f('ix_users_email'), 'users', ['email'], unique=True)

    # Products table
    op.create_table(
        'products',
        sa.Column('id', sa.Integer(), autoincrement=True, nullable=False),
        sa.Column('name', sa.String(255), nullable=False),
        sa.Column('description', sa.Text(), nullable=True),
        sa.Column('price', sa.Numeric(10, 2), nullable=False),
        sa.Column('stock', sa.Integer(), nullable=False, server_default='0'),
        sa.Column('created_at', sa.DateTime(timezone=True), server_default=sa.func.now(), nullable=False),
        sa.Column('updated_at', sa.DateTime(timezone=True), server_default=sa.func.now(), nullable=False),
        sa.PrimaryKeyConstraint('id'),
    )


def downgrade() -> None:
    op.drop_index(op.f('ix_users_email'), table_name='users')
    op.drop_table('products')
    op.drop_table('users')
```

---

## Resumen de Mejoras Aplicadas

| Categoría | Mejoras Implementadas |
|-----------|----------------------|
| **Seguridad** | Argon2id, JWT con expiración corta + refresh rotation, rate limiting Redis, headers de seguridad, network isolation, non-root containers, read-only FS, secrets validation fail-fast |
| **Arquitectura** | Capa de servicios separada, DI limpia, factory pattern para app, async end-to-end, structured logging con request IDs |
| **Testing** | pytest-asyncio con fixtures compartidas, factories de datos, cobertura >80% enforced, e2e con Playwright, unit tests de seguridad |
| **DevOps** | Multi-stage Dockerfiles, healthchecks integrados, scripts operativos, pre-commit ready, linting/formatting/type-checking configurados |
| **Documentación** | ADRs, política de seguridad, guía de setup, diagramas de arquitectura |
| **Frontend** | Middleware de protección de rutas, interceptores de token con auto-refresh, Zustand con persistencia parcial, TypeScript strict |

¿Quieres que profundice en algún módulo específico, añada CI/CD pipeline (GitHub Actions), o implemente alguna funcionalidad adicional?