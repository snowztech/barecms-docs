# Getting Started

Welcome to BareCMS! This comprehensive guide will help you set up BareCMS locally and get your first site running.

## 📋 Table of Contents

- [🔧 Prerequisites](#-prerequisites)
- [⚡ Quick Start](#-quick-start)
- [🔧 Development Setup](#-development-setup)
- [🎯 First Steps](#-first-steps)
- [💡 Available Commands](#-available-commands)
- [🐛 Troubleshooting](#-troubleshooting)
- [🚀 Next Steps](#-next-steps)

---

## 🔧 Prerequisites

Choose your setup based on your needs:

### For Quick Deployment (Recommended)

- **[Docker](https://docs.docker.com/get-docker/)** - Container platform
- **[Docker Compose](https://docs.docker.com/compose/install/)** - Multi-container orchestration
- **[Make](https://www.gnu.org/software/make/)** (optional) - For simplified commands

### For Local Development

- **[Node.js 18+](https://nodejs.org/)** - JavaScript runtime
- **[Go 1.21+](https://golang.org/)** - Go programming language
- **[Git](https://git-scm.com/)** - Version control

---

## ⚡ Quick Start

Get BareCMS running in under 5 minutes:

### 1. Clone the Repository

```bash
git clone https://github.com/snowztech/barecms.git
cd barecms
```

### 2. Environment Setup

Create your environment configuration:

```bash
cp .env.example .env
```

**Important**: Update your `.env` file with a secure JWT secret:

```bash
# Generate a secure JWT secret
openssl rand -base64 32

# Copy the output and add it to your .env file:
# JWT_SECRET=your-generated-secret-here
```

### 3. Start BareCMS

```bash
make up
```

This command will:

- ✅ Start PostgreSQL database with health checks
- ✅ Build and start the BareCMS application
- ✅ Set up the complete development environment

### 4. Access Your CMS

Open your browser and navigate to:

- **Application**: [http://localhost:8080](http://localhost:8080)
- **API**: [http://localhost:8080/api](http://localhost:8080/api)

🎉 **Congratulations! BareCMS is now running.**

---

## 🔧 Development Setup

### Prerequisites Installation

#### macOS (using Homebrew)

```bash
# Install Homebrew if you haven't already
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install prerequisites
brew install docker docker-compose node go git make
```

#### Ubuntu/Debian

```bash
# Update package index
sudo apt update

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.20.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install Go
wget https://go.dev/dl/go1.21.0.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.21.0.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc
```

### Environment Configuration

The `.env` file contains all configuration options:

```env
# Security Configuration (Required)
JWT_SECRET=your-super-secret-jwt-key-here

# Application Configuration
PORT=8080

# Database Configuration
POSTGRES_USER=barecms_user
POSTGRES_PASSWORD=your-secure-password
POSTGRES_DB=barecms_db
DATABASE_URL=postgresql://barecms_user:your-secure-password@postgres:5432/barecms_db
```

---

## 🎯 First Steps

Once BareCMS is running, here's how to create your first content:

### 1. Register Your First User

```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@example.com",
    "password": "securepassword123"
  }'
```

### 2. Login to Get Your Token

```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@example.com",
    "password": "securepassword123"
  }'
```

**Save the JWT token** from the response - you'll need it for authenticated requests.

### 3. Create Your First Site

```bash
curl -X POST http://localhost:8080/api/sites \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My First Site",
    "slug": "my-first-site",
    "description": "My first BareCMS site"
  }'
```

### 4. Create a Collection

```bash
curl -X POST http://localhost:8080/api/sites/1/collections \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Blog Posts",
    "slug": "posts",
    "description": "Collection of blog posts"
  }'
```

### 5. Create Your First Entry

```bash
curl -X POST http://localhost:8080/api/collections/1/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Welcome to BareCMS",
    "content": "This is my first blog post using BareCMS. It's simple and powerful!",
    "slug": "welcome-to-barecms"
  }'
```

### 6. Access Your Data Publicly

```bash
# Get all site data (no authentication needed!)
curl -X GET http://localhost:8080/my-first-site/data
```

**🎯 This is the core concept**: Use the authenticated API to manage content, then access it publicly for your frontend applications.

---

## 💡 Available Commands

### Make Commands (Recommended)

```bash
make up          # Start the development environment
make down        # Stop all services
make logs        # View application logs
make logs-db     # View database logs
make ui          # Build UI (frontend)
make clean       # Stop and cleanup containers
make restart     # Restart all services
make backup      # Backup database
make help        # Show all available commands
```

### Docker Compose Commands

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# View logs
docker compose logs -f barecms
docker compose logs -f postgres

# Restart specific service
docker compose restart barecms

# Pull latest images
docker compose pull
```

### Database Management

```bash
# Access database shell
docker compose exec postgres psql -U barecms_user -d barecms_db

# Backup database
docker compose exec postgres pg_dump -U barecms_user barecms_db > backup_$(date +%Y%m%d_%H%M%S).sql

# Restore database
docker compose exec -T postgres psql -U barecms_user -d barecms_db < backup_file.sql
```

---

## 🐛 Troubleshooting

### Common Issues and Solutions

#### Port Already in Use

```bash
# Check what's using port 8080
sudo lsof -i :8080

# Kill the process if needed
sudo kill -9 $(lsof -t -i:8080)

# Or change the port in .env
echo "PORT=8081" >> .env
```

#### Database Connection Issues

```bash
# Check database status
docker compose ps postgres

# Restart database
docker compose restart postgres

# View database logs
docker compose logs postgres
```

#### Application Won't Start

```bash
# Check application logs
make logs

# Verify environment variables
docker compose exec barecms env | grep JWT_SECRET
docker compose exec barecms env | grep DATABASE_URL

# Restart with fresh build
make clean
make up
```

#### Permission Issues

```bash
# Fix file permissions
sudo chown -R $USER:$USER .

# Reset Docker permissions
docker compose down
sudo rm -rf .docker-data
make up
```

### Debug Mode

Enable debug mode for more detailed logs:

```bash
# Add to your .env file
echo "DEBUG=true" >> .env
echo "LOG_LEVEL=debug" >> .env

# Restart
make restart
```

---

## 🚀 Next Steps

Now that you have BareCMS running:

### 🔍 Explore the Documentation

- **[API Reference](api.md)** - Complete API documentation
- **[Self-Hosting Guide](self-hosting.md)** - Deploy to production
- **[Development Guide](development.md)** - Contributing to BareCMS

### 🛠️ Build Something Amazing

- **Frontend Integration**: Use the public API endpoint `/:siteSlug/data` in your React, Vue, or Next.js app
- **Static Site Generation**: Fetch content at build time for Gatsby, Hugo, or Jekyll
- **Mobile Apps**: Access your content via the REST API

### 🤝 Join the Community

- **[GitHub Discussions](https://github.com/snowztech/barecms/discussions)** - Ask questions and share ideas
- **[Report Issues](https://github.com/snowztech/barecms/issues)** - Help improve BareCMS
- **[Contribute](development.md)** - Join the development

---

**Need help?** Check our [GitHub Discussions](https://github.com/snowztech/barecms/discussions) or [open an issue](https://github.com/snowztech/barecms/issues).
