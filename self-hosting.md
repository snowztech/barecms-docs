# Self-Hosting BareCMS

Deploy BareCMS on your own server using Docker.

## Prerequisites

- Linux server or VPS (Ubuntu 20.04+ recommended)
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## Quick Start

### 1. Connect to Your Server

```bash
ssh your-user@your-server-ip
```

### 2. Clone the Repository

```bash
mkdir -p ~/apps
cd ~/apps
git clone https://github.com/snowztech/barecms.git
cd barecms
```

### 3. Configure Environment

```bash
cp .env.example .env
nano .env
```

**Essential Environment Variables:**

```env
# Security Configuration (Required)
JWT_SECRET=your-super-secret-jwt-key-here

# Application Configuration
PORT=8080

# Database Configuration
POSTGRES_USER=barecms_user
POSTGRES_PASSWORD=your-secure-password
POSTGRES_DB=barecms_db
```

💡 **Generate a secure JWT secret:**

```bash
openssl rand -base64 32
```

### 4. Deploy

```bash
docker compose up -d
```

### 5. Access

Visit `http://your-server-ip:8080`

## Management Commands

### Docker Compose Commands

```bash
# Start/Stop services
docker compose up -d
docker compose down

# View logs
docker compose logs -f barecms
docker compose logs -f postgres

# Update and restart
git pull
docker compose pull
docker compose up -d

# Backup database
docker compose exec postgres pg_dump -U barecms_user barecms_db > backup_$(date +%Y%m%d_%H%M%S).sql

# Restore database
docker compose exec -T postgres psql -U barecms_user -d barecms_db < backup_file.sql
```

## Domain & HTTPS Setup

### 1. Point Your Domain

1. Log in to your domain registrar
2. Create an **A record** for `yourdomain.com` pointing to your server's IP
3. (Optional) Create a **CNAME record** for `www` pointing to `yourdomain.com`

### 2. Set Up HTTPS with Caddy

**Install Caddy:**

```bash
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/caddy-stable-archive-keyring.gpg] https://dl.cloudsmith.io/public/caddy/stable/deb/debian all main" | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

**Configure Caddy:**

```bash
sudo nano /etc/caddy/Caddyfile
```

Add:

```
yourdomain.com {
    reverse_proxy localhost:8080
}
```

**Reload Caddy:**

```bash
sudo systemctl reload caddy
```

✅ **Caddy will automatically:**

- Request and install an SSL certificate via Let's Encrypt
- Renew it automatically
- Proxy requests to your app running on port 8080

## Troubleshooting

### Application Won't Start

```bash
# Check logs
docker compose logs barecms

# Verify environment variables
docker compose exec barecms env | grep DATABASE_URL
docker compose exec barecms env | grep JWT_SECRET
```

### Database Connection Issues

```bash
# Test database connectivity
docker compose exec postgres psql -U barecms_user -d barecms_db -c "SELECT 1;"

# Check database status
docker compose ps postgres

# Check database logs
docker compose logs postgres
```

### Port Already in Use

```bash
# Check what's using port 8080
sudo lsof -i :8080

# Change port in docker-compose.yml if needed
# ports:
#   - "8081:8080"  # Use port 8081 instead
```

### JWT Secret Issues

```bash
# Ensure JWT_SECRET is set and not empty
docker compose exec barecms env | grep JWT_SECRET

# Generate a new secret if needed
openssl rand -base64 32
```

## Security Best Practices

- **Use strong passwords** for database and JWT secret
- **Keep your system updated**: `sudo apt update && sudo apt upgrade`
- **Use a firewall**: Configure UFW or iptables
- **Regular backups**: Set up automated database backups
- **Monitor logs**: Check application and system logs regularly

## Getting Help

If you encounter issues:

1. **Check the logs**: Use `docker compose logs`
2. **Verify configuration**: Ensure all environment variables are set correctly
3. **Search existing issues** or create a new one on [GitHub](https://github.com/snowztech/barecms/issues)
