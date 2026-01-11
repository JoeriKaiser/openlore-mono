# OpenLore Monorepo

OpenLore is a full-stack application for managing AI chat characters with RAG-enhanced conversations.

## Quick Start

### Prerequisites
- Docker & Docker Compose
- (Optional) Bun runtime for local development

### Production Deployment with Docker

1. **Clone and configure:**
   ```bash
   git clone git@github.com:JoeriKaiser/openlore-mono.git
   cd openlore-mono
   cp .env.example .env
   # Edit .env and set all CHANGE_ME values
   ```

2. **Start all services:**
   ```bash
   docker-compose up -d
   ```

3. **Access the application:**
   - Application: http://localhost
   - Health check: http://localhost/health

### Local Development (without Docker)

1. **Start PostgreSQL:**
   ```bash
   docker-compose up -d postgres
   ```

2. **Start backend:**
   ```bash
   cd backend
   bun install
   cp .env.example .env
   # Configure .env (use localhost for DATABASE_URL)
   bun run migrate
   bun run dev  # Runs on localhost:3000
   ```

3. **Start frontend:**
   ```bash
   cd frontend
   bun install
   cp .env.example .env
   # Set VITE_API_URL=http://localhost:3000/api
   bun dev  # Runs on localhost:5173
   ```

## Architecture

```
┌──────────────────────────────────────────────────────┐
│                  Nginx Reverse Proxy                  │
│                     (Port 80)                         │
│  Routes: /api/* → backend, /* → frontend             │
└────────────┬──────────────────────┬──────────────────┘
             │                       │
    ┌────────▼────────┐     ┌───────▼────────┐
    │    Frontend      │     │    Backend      │
    │  (React + Nginx) │     │   (Bun API)     │
    │    Port 80       │     │   Port 3000     │
    └──────────────────┘     └────────┬────────┘
                                      │
                             ┌────────▼────────┐
                             │   PostgreSQL     │
                             │   + pgvector     │
                             │   Port 5432      │
                             └──────────────────┘
```

## Project Structure

- `/frontend` - React application (Vite + Tailwind CSS)
- `/backend` - Bun REST API (Drizzle ORM + Better Auth)
- `/nginx` - Reverse proxy configuration
- `/docker-compose.yml` - Orchestrates all services

## Tech Stack

**Frontend:**
- React 19 + React Router 7
- Tailwind CSS 4 + Radix UI
- React Query 5 + Zustand
- Vite 7

**Backend:**
- Bun runtime
- PostgreSQL 16 + pgvector
- Drizzle ORM
- Better Auth
- HuggingFace Transformers (local embeddings)

## Docker Services

- **postgres** - PostgreSQL with pgvector extension
- **backend** - Bun API server
- **frontend** - React SPA with nginx
- **proxy** - Nginx reverse proxy (only service with external port)

## Environment Variables

See `.env.example` for all configuration options. Critical variables:

- **Secrets**: Generate with `openssl rand -hex 32`
  - `BETTER_AUTH_SECRET`
  - `AUTH_SECRET`
  - `ENCRYPTION_SECRET`
  - `POSTGRES_PASSWORD`

- **URLs**: Update for production deployment
  - `BETTER_AUTH_URL`
  - `CLIENT_URL`
  - `CORS_ORIGINS`

## Commands

### Docker
```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f [service_name]

# Stop all services
docker-compose down

# Rebuild after code changes
docker-compose up -d --build

# Reset everything (CAUTION: deletes database)
docker-compose down -v
```

### Backend Development
```bash
cd backend
bun run dev        # Start dev server with hot reload
bun run migrate    # Run database migrations
bun run seed       # Seed database with sample data
bun run db:generate # Generate new migration
```

### Frontend Development
```bash
cd frontend
bun dev            # Start dev server (port 5173)
bun run build      # Build for production
bun preview        # Preview production build
bun lint           # Lint code
bun run biome      # Format code
```

## Migration History

This monorepo was created by merging two repositories while preserving complete commit history:
- Frontend: git@github.com:JoeriKaiser/openlore.git
- Backend: git@github.com:JoeriKaiser/openlore-api.git

All commit history has been preserved. Use `git log frontend/` and `git log backend/` to view individual histories.

## License

MIT
