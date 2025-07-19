# Configuration Guide

This guide covers all configuration options for BareCMS, from basic setup to advanced production configurations.

## 📋 Overview

BareCMS is configured primarily through environment variables. This approach makes it easy to deploy across different environments while keeping sensitive data secure.

---

## 🔧 Environment Variables

### Required Variables

These variables **must** be set for BareCMS to function:

```env
# JWT Secret (REQUIRED)
JWT_SECRET=your-super-secret-jwt-key-here

# Database Connection (REQUIRED)
DATABASE_URL=postgresql://user:password@host:port/database
```

### Application Variables

```env
# Server Configuration
PORT=8080                    # Port number (default: 8080)
HOST=0.0.0.0                # Host address (default: 0.0.0.0)
ENV=production              # Environment: development, production (default: development)

# CORS Configuration
CORS_ORIGIN=*               # Allowed origins (default: *)
CORS_METHODS=GET,POST,PUT,DELETE  # Allowed methods
CORS_HEADERS=Content-Type,Authorization  # Allowed headers
```

### Database Variables (Docker Compose)

When using Docker Compose, these are automatically configured:

```env
# PostgreSQL Container Configuration
POSTGRES_USER=barecms_user
POSTGRES_PASSWORD=your-secure-password
POSTGRES_DB=barecms_db
POSTGRES_HOST=postgres
POSTGRES_PORT=5432

# Constructed Database URL
DATABASE_URL=postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}@${POSTGRES_HOST}:${POSTGRES_PORT}/${POSTGRES_DB}
```

### Security Variables

```env
# JWT Configuration
JWT_SECRET=your-super-secret-jwt-key-here  # REQUIRED: 32+ character string
JWT_EXPIRATION=24h                         # Token expiration (default: 24h)

# Password Security
PASSWORD_MIN_LENGTH=8                      # Minimum password length (default: 8)
BCRYPT_ROUNDS=12                          # Bcrypt rounds (default: 12)

# Rate Limiting
RATE_LIMIT_WINDOW=15m                     # Rate limit window (default: 15m)
RATE_LIMIT_MAX=100                        # Max requests per window (default: 100)
```

### Logging Variables

```env
# Logging Configuration
LOG_LEVEL=info              # Levels: debug, info, warn, error (default: info)
LOG_FORMAT=json             # Formats: json, text (default: json)
LOG_OUTPUT=stdout           # Output: stdout, file (default: stdout)
LOG_FILE=./logs/app.log     # Log file path (if LOG_OUTPUT=file)
```

### Performance Variables

```env
# Database Pool Configuration
DB_MAX_CONNECTIONS=20       # Maximum database connections (default: 20)
DB_IDLE_TIMEOUT=30s        # Connection idle timeout (default: 30s)
DB_MAX_LIFETIME=1h         # Connection max lifetime (default: 1h)

# Request Configuration
MAX_REQUEST_SIZE=10MB      # Maximum request body size (default: 10MB)
REQUEST_TIMEOUT=30s        # Request timeout (default: 30s)

# Cache Configuration
CACHE_TTL=1h              # Cache time-to-live (default: 1h)
CACHE_SIZE=100MB          # Memory cache size (default: 100MB)
```

---

## 📁 Configuration Files

### .env File Structure

```env
# .env - Main configuration file

#====================
# REQUIRED SETTINGS
#====================

# Security (Generate with: openssl rand -base64 32)
JWT_SECRET=AbCdEfGhIjKlMnOpQrStUvWxYz1234567890

# Database
DATABASE_URL=postgresql://barecms_user:secure_password@localhost:5432/barecms_db

#====================
# APPLICATION SETTINGS
#====================

# Server
PORT=8080
ENV=production

# CORS (adjust for your frontend domain)
CORS_ORIGIN=https://yourdomain.com

#====================
# DOCKER COMPOSE SETTINGS
#====================

# PostgreSQL
POSTGRES_USER=barecms_user
POSTGRES_PASSWORD=secure_password_here
POSTGRES_DB=barecms_db

#====================
# OPTIONAL SETTINGS
#====================

# Logging
LOG_LEVEL=info
LOG_FORMAT=json

# Security
JWT_EXPIRATION=24h
PASSWORD_MIN_LENGTH=8
BCRYPT_ROUNDS=12

# Performance
DB_MAX_CONNECTIONS=20
MAX_REQUEST_SIZE=10MB
```

### docker-compose.yml Configuration

```yaml
# docker-compose.yml
version: "3.8"

services:
  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql # Optional: Custom init
    ports:
      - "5432:5432" # Optional: External access
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
    environment:
      # Pass all environment variables
      - JWT_SECRET=${JWT_SECRET}
      - DATABASE_URL=${DATABASE_URL}
      - PORT=${PORT:-8080}
      - ENV=${ENV:-production}
      - LOG_LEVEL=${LOG_LEVEL:-info}
      - CORS_ORIGIN=${CORS_ORIGIN:-*}
    depends_on:
      postgres:
        condition: service_healthy
    volumes:
      - ./logs:/app/logs # Optional: Persistent logs
    restart: unless-stopped

volumes:
  postgres_data:
```

---

## 🔐 Security Configuration

### JWT Secret Generation

Generate a secure JWT secret:

```bash
# Option 1: OpenSSL (recommended)
openssl rand -base64 32

# Option 2: Node.js
node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"

# Option 3: Python
python3 -c "import secrets; print(secrets.token_urlsafe(32))"

# Option 4: Online generator
# Visit: https://generate-secret.vercel.app/32
```

### Database Security

```env
# Use strong passwords
POSTGRES_PASSWORD=your-very-secure-password-with-numbers-123

# Consider using database URLs with SSL
DATABASE_URL=postgresql://user:pass@host:5432/db?sslmode=require
```

### CORS Configuration

```env
# Development (allow all)
CORS_ORIGIN=*

# Production (specific domains)
CORS_ORIGIN=https://yourdomain.com,https://www.yourdomain.com

# Local development with frontend
CORS_ORIGIN=http://localhost:3000,http://localhost:3001
```

---

## 🌍 Environment-Specific Configurations

### Development Environment

```env
# .env.development
ENV=development
PORT=8080
LOG_LEVEL=debug
LOG_FORMAT=text
CORS_ORIGIN=*
JWT_EXPIRATION=7d
BCRYPT_ROUNDS=10
```

### Staging Environment

```env
# .env.staging
ENV=staging
PORT=8080
LOG_LEVEL=info
LOG_FORMAT=json
CORS_ORIGIN=https://staging.yourdomain.com
JWT_EXPIRATION=24h
BCRYPT_ROUNDS=12
```

### Production Environment

```env
# .env.production
ENV=production
PORT=8080
LOG_LEVEL=warn
LOG_FORMAT=json
CORS_ORIGIN=https://yourdomain.com
JWT_EXPIRATION=24h
BCRYPT_ROUNDS=12
RATE_LIMIT_MAX=50
```

---

## 📊 Database Configuration

### Connection Pool Settings

```env
# Optimize for your server resources
DB_MAX_CONNECTIONS=20        # Start with 20, adjust based on load
DB_IDLE_TIMEOUT=30s         # Close idle connections
DB_MAX_LIFETIME=1h          # Refresh connections periodically
```

### PostgreSQL Optimization

```sql
-- Custom PostgreSQL settings (init.sql)
ALTER SYSTEM SET shared_buffers = '256MB';
ALTER SYSTEM SET effective_cache_size = '1GB';
ALTER SYSTEM SET maintenance_work_mem = '64MB';
ALTER SYSTEM SET checkpoint_completion_target = 0.9;
ALTER SYSTEM SET wal_buffers = '16MB';
ALTER SYSTEM SET default_statistics_target = 100;
```

---

## 🚀 Performance Configuration

### Memory Settings

```env
# Adjust based on available memory
CACHE_SIZE=100MB            # In-memory cache size
MAX_REQUEST_SIZE=10MB       # Maximum upload size
```

### Timeout Settings

```env
# Prevent hanging requests
REQUEST_TIMEOUT=30s         # API request timeout
DB_QUERY_TIMEOUT=15s       # Database query timeout
```

---

## 📈 Monitoring Configuration

### Health Check Endpoint

BareCMS provides a health check endpoint:

```bash
# Check application health
curl http://localhost:8080/api/health

# Expected response:
{
  "status": "ok",
  "database": "connected",
  "uptime": "2h 30m 15s",
  "version": "1.0.0"
}
```

### Logging Configuration

```env
# Structured logging for production
LOG_LEVEL=info
LOG_FORMAT=json
LOG_OUTPUT=stdout

# File logging (optional)
LOG_OUTPUT=file
LOG_FILE=./logs/barecms.log
```

---

## 🔧 Advanced Configuration

### Custom Makefile Variables

```makefile
# Makefile environment variables
include .env
export

# Custom commands
.PHONY: up-prod
up-prod:
	ENV=production docker-compose up -d

.PHONY: backup
backup:
	docker-compose exec postgres pg_dump -U $(POSTGRES_USER) $(POSTGRES_DB) > backup_$(shell date +%Y%m%d_%H%M%S).sql
```

### Docker Override

```yaml
# docker-compose.override.yml (for development)
version: "3.8"

services:
  barecms:
    ports:
      - "8080:8080"
    environment:
      - ENV=development
      - LOG_LEVEL=debug
    volumes:
      - ./:/app
      - /app/ui/node_modules
    command: make dev
```

---

## 🐛 Troubleshooting Configuration

### Common Issues

**JWT Secret Not Set:**

```bash
# Error: JWT secret is required
# Solution: Set JWT_SECRET in .env
JWT_SECRET=your-secret-here
```

**Database Connection Failed:**

```bash
# Error: failed to connect to database
# Check: DATABASE_URL format
DATABASE_URL=postgresql://user:password@host:port/database

# Verify database is running
docker-compose ps postgres
```

**Port Already in Use:**

```bash
# Error: bind: address already in use
# Solution: Change port or stop conflicting service
PORT=8081
```

### Configuration Validation

```bash
# Validate configuration
docker-compose config

# Check environment variables
docker-compose exec barecms env | grep -E "JWT_SECRET|DATABASE_URL|PORT"

# Test database connection
docker-compose exec postgres psql -U barecms_user -d barecms_db -c "SELECT 1;"
```

---

## 📋 Configuration Checklist

Before deploying BareCMS:

- [ ] **JWT_SECRET** is set and secure (32+ characters)
- [ ] **DATABASE_URL** is correctly formatted
- [ ] **POSTGRES_PASSWORD** is strong and secure
- [ ] **PORT** is available and not in use
- [ ] **CORS_ORIGIN** is configured for your frontend
- [ ] **ENV** is set to appropriate environment
- [ ] **LOG_LEVEL** is appropriate for environment
- [ ] Database connection tested and working
- [ ] Health check endpoint responds correctly

---

## 🚀 Next Steps

With BareCMS properly configured:

1. **[Complete the Quick Start →](quick-start.md)** - Build your first site
2. **[Deploy to Production →](../deployment/self-hosting.md)** - Go live
3. **[Learn the API →](../api/README.md)** - Explore endpoints
4. **[Monitor Performance →](../guides/performance.md)** - Optimize

---

_Configuration complete? Time to build something amazing! 🎯_
