# Quick Start Tutorial

Create your first site with BareCMS in 15 minutes.

## 🎯 What You'll Build

By the end of this tutorial, you'll have:

- ✅ A running BareCMS instance
- ✅ A "My Blog" site with posts
- ✅ Sample blog content
- ✅ Public API access to your content

---

## 📋 Prerequisites

- BareCMS installed and running ([Installation Guide](installation.md))
- Basic understanding of APIs and JSON
- A tool to make HTTP requests (curl or similar)

---

## 🚀 Step 1: Start BareCMS

If you haven't already, start BareCMS:

```bash
cd barecms
make up

# Verify it's running
curl http://localhost:8080/api/health
# Should return: {"status": "ok", "database": "connected"}
```

---

## 👤 Step 2: Create Your Admin Account

Register your first admin account:

```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@example.com",
    "password": "secure_password123"
  }'
```

**Response:**

```json
{
  "message": "User registered successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "email": "admin@example.com"
  }
}
```

💡 **Save your JWT token** - you'll need it for authenticated requests!

---

## 🏠 Step 3: Create Your First Site

```bash
curl -X POST http://localhost:8080/api/sites \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My Blog",
    "slug": "my-blog",
    "description": "A simple blog built with BareCMS"
  }'
```

**Response:**

```json
{
  "site": {
    "id": 1,
    "name": "My Blog",
    "slug": "my-blog",
    "description": "A simple blog built with BareCMS",
    "created_at": "2024-01-15T10:30:00Z"
  }
}
```

---

## 📚 Step 4: Create a Collection

Collections group similar content. Let's create a "Posts" collection:

```bash
curl -X POST http://localhost:8080/api/sites/1/collections \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Posts",
    "slug": "posts",
    "description": "Blog posts collection"
  }'
```

**Response:**

```json
{
  "collection": {
    "id": 1,
    "name": "Posts",
    "slug": "posts",
    "description": "Blog posts collection",
    "site_id": 1,
    "created_at": "2024-01-15T10:35:00Z"
  }
}
```

---

## 📝 Step 5: Add Your First Entry

Now let's create your first blog post:

```bash
curl -X POST http://localhost:8080/api/collections/1/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Welcome to BareCMS",
    "content": "This is my first blog post using BareCMS! It'\''s incredibly simple and powerful.",
    "slug": "welcome-to-barecms"
  }'
```

**Response:**

```json
{
  "entry": {
    "id": 1,
    "title": "Welcome to BareCMS",
    "content": "This is my first blog post using BareCMS! It's incredibly simple and powerful.",
    "slug": "welcome-to-barecms",
    "collection_id": 1,
    "created_at": "2024-01-15T10:40:00Z"
  }
}
```

---

## 🌐 Step 6: Access Your Data Publicly

Now for the magic - access all your content with a single public API call:

```bash
curl http://localhost:8080/my-blog/data
```

**Response:**

```json
{
  "site": {
    "id": 1,
    "name": "My Blog",
    "slug": "my-blog",
    "description": "A simple blog built with BareCMS"
  },
  "collections": [
    {
      "id": 1,
      "name": "Posts",
      "slug": "posts",
      "description": "Blog posts collection",
      "entries": [
        {
          "id": 1,
          "title": "Welcome to BareCMS",
          "content": "This is my first blog post using BareCMS! It's incredibly simple and powerful.",
          "slug": "welcome-to-barecms",
          "created_at": "2024-01-15T10:40:00Z"
        }
      ]
    }
  ]
}
```

🎉 **Congratulations!** You now have a working headless CMS with public API access.

---

## 💻 Step 7: Use in Your Frontend

Here's how to consume this data in a simple HTML page:

```html
<!doctype html>
<html>
  <head>
    <title>My Blog</title>
  </head>
  <body>
    <div id="blog"></div>

    <script>
      fetch("http://localhost:8080/my-blog/data")
        .then((response) => response.json())
        .then((data) => {
          const blogDiv = document.getElementById("blog");

          // Display site title
          blogDiv.innerHTML = `<h1>${data.site.name}</h1>`;

          // Display posts
          const posts = data.collections.find((c) => c.slug === "posts");
          if (posts) {
            posts.entries.forEach((post) => {
              blogDiv.innerHTML += `
                            <article>
                                <h2>${post.title}</h2>
                                <p>${post.content}</p>
                                <small>Published: ${new Date(post.created_at).toLocaleDateString()}</small>
                            </article>
                        `;
            });
          }
        });
    </script>
  </body>
</html>
```

---

## 🔄 Step 8: Add More Content

Let's add another blog post:

```bash
curl -X POST http://localhost:8080/api/collections/1/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Getting Started with Headless CMS",
    "content": "Headless CMS separates content from presentation, giving developers complete freedom in how they display content.",
    "slug": "getting-started-headless-cms"
  }'
```

Now when you call the public API again, you'll see both posts!

---

## ✅ What You've Accomplished

You've successfully:

- ✅ **Set up BareCMS** with Docker
- ✅ **Created your first site** with authentication
- ✅ **Organized content** with collections
- ✅ **Added content** with entries
- ✅ **Accessed data publicly** via REST API
- ✅ **Built a simple frontend** to display content

---

## 🚀 Next Steps

Now that you have the basics:

1. **[Learn the Complete API →](../api/README.md)** - Explore all endpoints
2. **[Deploy to Production →](../deployment/self-hosting.md)** - Make it live
3. **[Check Frontend Examples →](../integration/frontend-examples.md)** - See React, Vue examples

---

## 💡 Key Concepts

- **Sites**: Containers for your projects
- **Collections**: Groups of similar content (posts, pages, products)
- **Entries**: Individual pieces of content
- **Public API**: `GET /:siteSlug/data` - No auth needed!
- **Admin API**: CRUD operations with JWT authentication

---

_Ready to build something amazing? The API is your playground! 🎯_
