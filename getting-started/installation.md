# Installation Guide

Get BareCMS running locally in 5 minutes.

## 📋 Prerequisites

- **Docker** - [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose** - [Install Docker Compose](https://docs.docker.com/compose/install/)
- **Git** - [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

---

## 🚀 Quick Installation

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
# Start the application
make up

# Or using Docker Compose directly
docker compose up -d
```

### 4. Access BareCMS

- **Web Interface**: [http://localhost:8080](http://localhost:8080)
- **API Base**: `http://localhost:8080/api`

🎉 **That's it!** BareCMS is now running locally.

---

## 🔧 Available Commands

BareCMS includes these Make commands:

```bash
# Start development environment
make up

# Build UI (frontend)
make ui

# View logs
make logs

# Stop and cleanup containers
make clean

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

# Build frontend
cd ui && npm install && npm run build
```

---

## 🗄️ Database Setup

Database is automatically configured with Docker Compose. Data is persisted in Docker volumes.

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
# Pull latest changes
git pull
docker compose pull
docker compose up -d
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

### Getting Help

- **Check logs:** `docker compose logs barecms`
- **Database issues:** `docker compose logs postgres`
- **GitHub Issues:** [Report a bug](https://github.com/snowztech/barecms/issues)
- **Discussions:** [Ask for help](https://github.com/snowztech/barecms/discussions)

---

## 🚀 Next Steps

Now that BareCMS is installed:

1. **[Complete the Quick Start Tutorial →](quick-start.md)**
2. **[Learn the API →](../api/README.md)**
3. **[Deploy to Production →](../deployment/self-hosting.md)**

---

_Ready to create something awesome? Let's build your first site! 🎯_
