# Getting Started with BareCMS

Welcome to BareCMS! This guide will help you get up and running quickly with your first headless CMS setup.

## 📋 What You'll Learn

- [**Installation**](installation.md) - Get BareCMS running locally in 5 minutes
- [**Quick Start**](quick-start.md) - Create your first site and content

---

## 🚀 Installation

### Prerequisites

- **Docker & Docker Compose** - [Install Docker](https://docs.docker.com/get-docker/)
- **Git** - [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- **5 minutes of your time**

### Quick Installation

```bash
# Clone the repository
git clone https://github.com/snowztech/barecms.git
cd barecms

# Set up environment
cp .env.example .env

# Generate a secure JWT secret
openssl rand -base64 32
# Copy this to your .env file as JWT_SECRET

# Start BareCMS
make up
```

### Access Your CMS

- **Web Interface**: [http://localhost:8080](http://localhost:8080)
- **API Base URL**: `http://localhost:8080/api`

---

## ⚡ Quick Start

### 1. Create Your Account

Register your first admin account:

```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@example.com","password":"secure_password"}'
```

### 2. Create Your First Site

```bash
curl -X POST http://localhost:8080/api/sites \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"My Blog","slug":"my-blog","description":"My awesome blog"}'
```

### 3. Add a Collection

```bash
curl -X POST http://localhost:8080/api/sites/1/collections \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"Posts","slug":"posts","description":"Blog posts"}'
```

### 4. Create Your First Entry

```bash
curl -X POST http://localhost:8080/api/collections/1/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title":"Welcome to BareCMS","content":"This is my first blog post using BareCMS!","slug":"welcome-to-barecms"}'
```

### 5. Access Your Data Publicly

```bash
# No authentication needed!
curl http://localhost:8080/my-blog/data
```

**Response:**

```json
{
  "site": {
    "id": 1,
    "name": "My Blog",
    "slug": "my-blog",
    "description": "My awesome blog"
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
          "content": "This is my first blog post using BareCMS!",
          "slug": "welcome-to-barecms",
          "created_at": "2024-01-15T10:30:00Z"
        }
      ]
    }
  ]
}
```

🎉 **Congratulations!** You now have a working headless CMS with public API access.

---

## 🎯 Key Concepts

### Sites

A **Site** is a container for your content. Think of it as your project or website.

### Collections

**Collections** are groups of similar content within a site (e.g., "Posts", "Pages", "Products").

### Entries

**Entries** are individual pieces of content within a collection (e.g., a blog post, a page, a product).

### Public Data API

The **Public Data API** (`GET /:siteSlug/data`) provides all your site's content without authentication - perfect for frontend consumption.

---

## 🚀 Next Steps

Now that you have BareCMS running:

- **[Learn the API →](../api/README.md)**
- **[Deploy to Production →](../deployment/self-hosting.md)**
- **[See Integration Examples →](../integration/frontend-examples.md)**

---

## 🆘 Need Help?

- **GitHub Issues**: [Report bugs or request features](https://github.com/snowztech/barecms/issues)
- **GitHub Discussions**: [Ask questions and share ideas](https://github.com/snowztech/barecms/discussions)

---

_Ready to build something awesome with BareCMS? Let's go! 🚀_
