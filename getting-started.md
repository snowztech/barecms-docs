# 🚀 Getting Started

Get up and running with BareCMS in minutes. This guide covers everything from basic usage to your first deployment.

---

## 💡 Example Usage

The key endpoint for headless usage:

```http
GET /:siteSlug/data
```

This returns all content for a site without requiring authentication, making it perfect for frontend applications.

**Example usage:**

```javascript
// Fetch public site data
const response = await fetch("https://your-cms.com/my-site/data");
const siteData = await response.json();

// Access collections and entries
const posts = siteData.collections.posts;
const pages = siteData.collections.pages;
```

**Response structure:**

```json
{
  "site": {
    "id": 1,
    "name": "My Blog",
    "slug": "my-blog"
  },
  "collections": [
    {
      "id": 1,
      "name": "Posts",
      "slug": "posts",
      "entries": [
        {
          "id": 1,
          "title": "Welcome to BareCMS",
          "content": "...",
          "slug": "welcome"
        }
      ]
    }
  ]
}
```

---

## 📦 Quick Start

### Prerequisites

**For Quick Deployment:**

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

**For Local Development:**

- [Node.js](https://nodejs.org/) (v18+ recommended)
- [Go](https://golang.org/) (v1.21+ recommended)

### Installation

#### 1. Clone the Repository

```bash
git clone https://github.com/snowztech/barecms.git
cd barecms
```

#### 2. Set Up Environment Variables

```bash
cp .env.example .env
```

Edit `.env` and update the `JWT_SECRET` with a strong, random string:

```bash
# Generate a secure JWT secret
openssl rand -base64 32
```

#### 3. Start the Application

```bash
make up
```

#### 4. Access BareCMS

Open your browser and navigate to [http://localhost:8080](http://localhost:8080)

### Development Commands

The project includes a Makefile with these commands:

```bash
make up       # Start development environment
make ui       # Build UI (frontend)
make clean    # Stop and cleanup containers
make logs     # Show container logs
make help     # Show all available commands
```

### First Steps

Once BareCMS is running:

#### 1. Register a User

```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email": "admin@example.com", "password": "password123"}'
```

#### 2. Login to Get Token

```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "admin@example.com", "password": "password123"}'
```

Save the returned JWT token for authenticated requests.

#### 3. Create Your First Site

```bash
curl -X POST http://localhost:8080/api/sites \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name": "My Site", "slug": "my-site", "description": "My first site"}'
```

#### 4. Access Your Data Publicly

```bash
# Get all site data (no authentication needed)
curl -X GET http://localhost:8080/my-site/data
```

### Troubleshooting

#### Check Container Status

```bash
make logs
```

#### Restart Services

```bash
make clean
make up
```

---

## Next Steps

- [**API Reference**](api.md) - Learn about all available endpoints
- [**Self-Hosting**](self-hosting.md) - Deploy to production
- [**Development**](development.md) - Contributing to the project
