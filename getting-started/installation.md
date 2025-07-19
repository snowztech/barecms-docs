# Installation Guide

This guide covers all the ways to install and run BareCMS, from local development to production deployment.

## 📋 Prerequisites

Before installing BareCMS, ensure you have:

- **Docker** - [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose** - [Install Docker Compose](https://docs.docker.com/compose/install/)
- **Make** (optional) - For simplified commands
- **Git** - [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

---

## 🚀 Quick Installation (Recommended)

### 1. Clone the Repository

```bash
git clone https://github.com/snowztech/barecms.git
cd barecms
```

### 2. Configure Environment

```bash
# Copy the environment template
cp .env.example .env

# Generate a secure JWT secret
openssl rand -base64 32
```

Edit `.env` and set your `JWT_SECRET`:

```env
# Required: Set a strong JWT secret
JWT_SECRET=your-super-secret-jwt-key-here

# Database Configuration (Docker Compose handles this)
POSTGRES_USER=barecms_user
POSTGRES_PASSWORD=your-secure-password
POSTGRES_DB=barecms_db
DATABASE_URL=postgresql://barecms_user:your-secure-password@postgres:5432/barecms_db

# Application Configuration
PORT=8080
```

### 3. Start BareCMS

```bash
# Using Make (recommended)
make up

# Or using Docker Compose directly
docker compose up -d
```

### 4. Access BareCMS

- **Web Interface**: [http://localhost:8080](http://localhost:8080)
- **API Base**: `http://localhost:8080/api`

🎉 **That's it!** BareCMS is now running locally.

---

## 📦 Installation Options

### Option 1: Docker Compose (Recommended)

Best for: **Local development, testing, small production setups**

```bash
git clone https://github.com/snowztech/barecms.git
cd barecms
cp .env.example .env
# Edit .env with your JWT_SECRET
make up
```

**Pros:**

- ✅ Everything included (app + database)
- ✅ Zero configuration
- ✅ Easy to update
- ✅ Data persisted in volumes

**Cons:**

- ❌ Single server only
- ❌ Not suitable for high-traffic sites

### Option 2: Docker Only (External Database)

Best for: **Production with managed database**

```bash
# Start with external PostgreSQL database
docker run -d \
  --name barecms \
  -p 8080:8080 \
  -e JWT_SECRET=your-super-secret-jwt-key \
  -e DATABASE_URL=postgresql://user:pass@your-db-host:5432/barecms_db \
  ghcr.io/snowztech/barecms:latest
```

**Pros:**

- ✅ Use managed database services
- ✅ Better for scaling
- ✅ Separate database backups

**Cons:**

- ❌ Requires existing PostgreSQL
- ❌ More configuration

### Option 3: Development Setup

Best for: **Contributing to BareCMS, custom modifications**

**Prerequisites:**

- [Go 1.21+](https://golang.org/doc/install)
- [Node.js 18+](https://nodejs.org/)
- [PostgreSQL](https://www.postgresql.org/download/)

```bash
# Clone and setup
git clone https://github.com/snowztech/barecms.git
cd barecms

# Backend setup
go mod tidy
cp .env.example .env
# Edit .env with database connection

# Frontend setup
cd ui
npm install
npm run build
cd ..

# Run development server
make dev
```

---

## ⚙️ Environment Configuration

### Required Variables

```env
# Security (REQUIRED)
JWT_SECRET=your-super-secret-jwt-key-here

# Database (Required for external database)
DATABASE_URL=postgresql://user:password@host:port/database
```

### Optional Variables

```env
# Application
PORT=8080                    # Server port (default: 8080)
ENV=production              # Environment mode

# Database (Docker Compose only)
POSTGRES_USER=barecms_user
POSTGRES_PASSWORD=secure_password
POSTGRES_DB=barecms_db

# Logging
LOG_LEVEL=info              # debug, info, warn, error
```

### Generating JWT Secret

```bash
# Option 1: OpenSSL (recommended)
openssl rand -base64 32

# Option 2: Online generator
# Visit: https://generate-secret.vercel.app/32

# Option 3: Node.js
node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"
```

---

## 🔧 Development Commands

BareCMS includes helpful Make commands for development:

```bash
# Start development environment
make up

# View logs
make logs

# Stop all services
make down

# Clean up (removes containers and volumes)
make clean

# Build frontend only
make ui

# Run tests
make test

# Show all available commands
make help
```

### Manual Commands

If you prefer not to use Make:

```bash
# Start services
docker compose up -d

# View logs
docker compose logs -f

# Stop services
docker compose down

# Update to latest version
docker compose pull
docker compose up -d

# Build frontend
cd ui && npm run build
```

---

## 🗄️ Database Setup

### Using Docker Compose (Default)

Database is automatically configured. Data is persisted in Docker volumes.

### Using External PostgreSQL

1. **Create Database:**

```sql
CREATE DATABASE barecms_db;
CREATE USER barecms_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE barecms_db TO barecms_user;
```

2. **Set DATABASE_URL:**

```env
DATABASE_URL=postgresql://barecms_user:your_password@localhost:5432/barecms_db
```

3. **Run Migrations:**

```bash
# Migrations run automatically on startup
docker run --rm \
  -e DATABASE_URL=your_database_url \
  ghcr.io/snowztech/barecms:latest
```

---

## 📊 Verification

### Health Check

```bash
# Check if BareCMS is running
curl http://localhost:8080/api/health

# Expected response:
# {"status": "ok", "database": "connected"}
```

### Create Test User

```bash
# Register first user (admin)
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@example.com",
    "password": "secure_password"
  }'
```

### Access Web Interface

Visit [http://localhost:8080](http://localhost:8080) and you should see the BareCMS login page.

---

## 🔄 Updates

### Updating BareCMS

```bash
# Using Docker Compose
git pull
docker compose pull
docker compose up -d

# Using Docker only
docker pull ghcr.io/snowztech/barecms:latest
docker stop barecms
docker rm barecms
# Run your docker run command again
```

### Version Pinning

For production, pin to specific versions:

```yaml
# docker-compose.yml
services:
  barecms:
    image: ghcr.io/snowztech/barecms:v1.0.0 # Pin to specific version
```

---

## 🐛 Troubleshooting

### Common Issues

**Port Already in Use:**

```bash
# Check what's using port 8080
sudo lsof -i :8080

# Or change port in .env
PORT=8081
```

**Database Connection Failed:**

```bash
# Check database logs
docker compose logs postgres

# Restart database
docker compose restart postgres
```

**Permission Denied:**

```bash
# Fix file permissions
sudo chown -R $USER:$USER .
```

### Getting Help

- **Check logs:** `docker compose logs barecms`
- **Database issues:** `docker compose logs postgres`
- **GitHub Issues:** [Report a bug](https://github.com/snowztech/barecms/issues)
- **Discussions:** [Ask for help](https://github.com/snowztech/barecms/discussions)

---

## 🚀 Next Steps

Now that BareCMS is installed:

1. **[Complete the Quick Start Tutorial →](quick-start.md)**
2. **[Build Your First Site →](first-site.md)**
3. **[Learn the API →](../api/README.md)**
4. **[Deploy to Production →](../deployment/self-hosting.md)**

---

_Ready to create something awesome? Let's build your first site! 🎯_
