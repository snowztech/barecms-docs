# Getting Started

This guide will help you set up BareCMS locally for development.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/snowztech/barecms.git
cd barecms
```

### 2. Environment Setup

Create a `.env` file from the example:

```bash
cp .env.example .env
```

Edit the `.env` file with your configuration. The key variables include:

- `JWT_SECRET` - JWT secret key (generate with `openssl rand -base64 32`)
- `POSTGRES_USER` - Database username
- `POSTGRES_PASSWORD` - Database password
- `POSTGRES_DB` - Database name
- `PORT` - Application port (default: 8080)

### 3. Start BareCMS

Start the development environment:

```bash
make up
```

This will:

- Start PostgreSQL database with health checks
- Build and start the BareCMS application
- Make the application available at `http://localhost:8080`

## Available Commands

The project includes a Makefile with these commands:

```bash
make up       # Start the development environment
make ui       # Build UI (frontend)
make clean    # Stop and cleanup containers
make logs     # Show container logs
make help     # Show help message
```

## First Steps

Once BareCMS is running:

### 1. Register a User

```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email": "admin@example.com", "password": "password123"}'
```

### 2. Login to Get Token

```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "admin@example.com", "password": "password123"}'
```

Save the returned JWT token for authenticated requests.

### 3. Create Your First Site

```bash
curl -X POST http://localhost:8080/api/sites \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name": "My Site", "slug": "my-site", "description": "My first site"}'
```

### 4. Access Your Data Publicly

```bash
# Get all site data (no authentication needed)
curl -X GET http://localhost:8080/my-site/data
```

## Next Steps

- [**API Reference**](api.md) - Learn about all available endpoints
- [**Self-Hosting**](self-hosting.md) - Deploy to production
- [**Development**](development.md) - Contributing to the project

## Troubleshooting

### Check Container Status

```bash
make logs
```

### Restart Services

```bash
make clean
make up
```
