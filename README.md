# BareCMS Documentation

BareCMS is a lightweight headless CMS built with Go and React.

## What is BareCMS?

BareCMS provides a simple API-first content management system with:

- **Sites** - Content containers for your projects
- **Collections** - Groups of related content (e.g., "Posts", "Pages")
- **Entries** - Individual pieces of content within collections
- **Public API** - Access your content without authentication

## Quick Overview

### Architecture

- **Backend**: Go with Gin framework
- **Frontend**: React application
- **Database**: PostgreSQL
- **Container**: Docker support included

### Core Concept

1. **Manage content** through authenticated API endpoints
2. **Access content publicly** via the site data endpoint
3. **Build your frontend** using any technology to consume the public API

### Public Data Access

The key endpoint for headless usage:

```
GET /:siteSlug/data
```

This returns all content for a site without requiring authentication, making it perfect for frontend applications.

## Getting Started

### 🚀 Local Development

Quick setup for development:

```bash
git clone https://github.com/snowztech/barecms.git
cd barecms
cp .env.example .env
make up
```

Access at `http://localhost:8080`

### 🐳 Production Deployment

Deploy to your own server:

```bash
# On your server
git clone https://github.com/snowztech/barecms.git
cd barecms
cp .env.example .env
# Edit .env with your settings
docker compose up -d
```

## Next Steps

- [**🚀 Getting Started**](getting-started.md) - Set up BareCMS locally
- [**🔌 API Reference**](api.md) - Complete API documentation
- [**🐳 Self-Hosting**](self-hosting.md) - Deploy to production
- [**⚙️ Development**](development.md) - Contributing to BareCMS

---

_Simple, lightweight, and built for developers._
