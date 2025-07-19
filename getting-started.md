# Getting Started

Get BareCMS up and running quickly with Docker. This guide provides step-by-step instructions for both quick deployment and local development.

## Prerequisites

**For Quick Deployment:**

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

**For Local Development:**

- [Node.js](https://nodejs.org/) (v18+ recommended)
- [Go](https://golang.org/) (v1.21+ recommended)

## Quick Deployment with Docker Compose

### Step 1: Create your project directory

```bash
mkdir barecms-app && cd barecms-app
```

### Step 2: Create configuration files

Create `.env` file:

```env
# Security (REQUIRED)
JWT_SECRET=your-super-secret-jwt-key-here

# Database Configuration
POSTGRES_USER=barecms_user
POSTGRES_PASSWORD=your-secure-password
POSTGRES_DB=barecms_db
DATABASE_URL=postgresql://barecms_user:your-secure-password@postgres:5432/barecms_db

# Application
PORT=8080
```

Create a `docker-compose.yml` file:

```yaml
services:
  postgres:
    image: postgres:16-alpine
    env_file: .env
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
      interval: 5s
      timeout: 5s
      retries: 5
    restart: unless-stopped

  barecms:
    image: ghcr.io/snowztech/barecms:latest
    ports:
      - "${PORT:-8080}:8080"
    env_file: .env
    depends_on:
      postgres:
        condition: service_healthy
    restart: unless-stopped

volumes:
  postgres_data:
```

### Step 3: Launch BareCMS

```bash
docker-compose up -d
```

🎉 **BareCMS is now running at [http://localhost:8080](http://localhost:8080)**

## Local Development Setup

### 1. Clone the repository

```bash
git clone https://github.com/snowztech/barecms.git
cd barecms
```

### 2. Set up environment variables

```bash
cp .env.example .env
```

Edit `.env` and update the `JWT_SECRET` with a strong, random string:

```bash
# Generate a secure JWT secret
openssl rand -base64 32
```

### 3. Start the application

```bash
make up
```

### 4. Access BareCMS

Open your browser and navigate to [http://localhost:8080](http://localhost:8080)

## Development Commands

```bash
make up       # Start development environment
make ui       # Build UI (frontend)
make clean    # Stop and cleanup containers
make logs     # View application logs
make help     # Show all available commands
```

## Management Commands

```bash
# View logs
docker compose logs -f barecms

# Stop BareCMS
docker compose down

# Update to latest version
docker compose pull && docker compose up -d

# Backup database
docker compose exec postgres pg_dump -U barecms_user barecms_db > backup.sql

# Access database shell
docker compose exec postgres psql -U barecms_user -d barecms_db
```

## Security Configuration

**Generate a secure JWT secret:**

```bash
openssl rand -base64 32
```

Make sure to use this generated value for `JWT_SECRET` in your `.env` file.

## Next Steps

- Create your first site and collection through the admin interface
- Check out the [Quick Example](quick-example.md) to see how to access your data
- Review the [API Reference](api.md) for complete documentation

## Need Help?

- **🐛 Report Issues**: [GitHub Issues](https://github.com/snowztech/barecms/issues)
- **💬 Discussions**: [GitHub Discussions](https://github.com/snowztech/barecms/discussions)
