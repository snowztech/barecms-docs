# Quick Start Tutorial

This tutorial will guide you through creating your first site, collections, and entries with BareCMS in just 15 minutes.

## 🎯 What You'll Build

By the end of this tutorial, you'll have:

- ✅ A running BareCMS instance
- ✅ A "My Blog" site with "Posts" collection
- ✅ Sample blog posts
- ✅ Public API access to your content
- ✅ Knowledge to integrate with any frontend

---

## 📋 Prerequisites

- BareCMS installed and running ([Installation Guide](installation.md))
- Basic understanding of APIs and JSON
- A tool to make HTTP requests (curl, Postman, or browser)

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

### Via Web Interface (Recommended)

1. Open [http://localhost:8080](http://localhost:8080)
2. Click "Register"
3. Fill in your details:
   - **Email**: `admin@example.com`
   - **Password**: `secure_password123`
4. Click "Register"

### Via API

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
  "id": 1,
  "name": "My Blog",
  "slug": "my-blog",
  "description": "A simple blog built with BareCMS",
  "created_at": "2024-01-15T10:30:00Z"
}
```

✅ **Success!** Your site is created with slug `my-blog`.

---

## 📚 Step 4: Add a Collection

Collections group related content. Let's create a "Posts" collection:

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
  "id": 1,
  "name": "Posts",
  "slug": "posts",
  "description": "Blog posts collection",
  "site_id": 1,
  "created_at": "2024-01-15T10:35:00Z"
}
```

---

## ✍️ Step 5: Create Your First Entry

Let's add your first blog post:

```bash
curl -X POST http://localhost:8080/api/collections/1/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Welcome to My Blog",
    "content": "This is my first blog post using BareCMS! I can manage content through the API and display it on any frontend I choose.",
    "slug": "welcome-to-my-blog"
  }'
```

**Response:**

```json
{
  "id": 1,
  "title": "Welcome to My Blog",
  "content": "This is my first blog post using BareCMS! I can manage content through the API and display it on any frontend I choose.",
  "slug": "welcome-to-my-blog",
  "collection_id": 1,
  "created_at": "2024-01-15T10:40:00Z"
}
```

---

## 📝 Step 6: Add More Content

Let's add a few more posts to make it interesting:

```bash
# Second post
curl -X POST http://localhost:8080/api/collections/1/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Why I Chose BareCMS",
    "content": "BareCMS is incredibly simple yet powerful. The API-first approach means I can use any frontend framework I want, and the public data endpoint makes content consumption effortless.",
    "slug": "why-i-chose-barecms"
  }'

# Third post
curl -X POST http://localhost:8080/api/collections/1/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Getting Started with Headless CMS",
    "content": "Headless CMS architecture separates content management from presentation. This gives developers ultimate flexibility in how they display content.",
    "slug": "getting-started-headless-cms"
  }'
```

---

## 🌐 Step 7: Access Your Content Publicly

Here's the magic! Get all your site's content with a single API call - **no authentication required**:

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
          "title": "Welcome to My Blog",
          "content": "This is my first blog post using BareCMS! I can manage content through the API and display it on any frontend I choose.",
          "slug": "welcome-to-my-blog",
          "created_at": "2024-01-15T10:40:00Z"
        },
        {
          "id": 2,
          "title": "Why I Chose BareCMS",
          "content": "BareCMS is incredibly simple yet powerful. The API-first approach means I can use any frontend framework I want, and the public data endpoint makes content consumption effortless.",
          "slug": "why-i-chose-barecms",
          "created_at": "2024-01-15T10:42:00Z"
        },
        {
          "id": 3,
          "title": "Getting Started with Headless CMS",
          "content": "Headless CMS architecture separates content management from presentation. This gives developers ultimate flexibility in how they display content.",
          "slug": "getting-started-headless-cms",
          "created_at": "2024-01-15T10:44:00Z"
        }
      ]
    }
  ]
}
```

🎉 **Congratulations!** You now have a fully functional headless CMS with public content access.

---

## 💻 Step 8: Use in Your Frontend

Here's how you'd consume this data in different frameworks:

### JavaScript (Vanilla)

```javascript
// Fetch and display blog posts
async function loadBlog() {
  const response = await fetch("http://localhost:8080/my-blog/data");
  const { site, collections } = await response.json();

  const posts = collections.find((c) => c.slug === "posts").entries;

  posts.forEach((post) => {
    console.log(`${post.title}: ${post.content}`);
  });
}

loadBlog();
```

### React

```jsx
import { useState, useEffect } from "react";

function Blog() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch("http://localhost:8080/my-blog/data")
      .then((res) => res.json())
      .then(setData);
  }, []);

  if (!data) return <div>Loading...</div>;

  const posts = data.collections.find((c) => c.slug === "posts").entries;

  return (
    <div>
      <h1>{data.site.name}</h1>
      <p>{data.site.description}</p>

      {posts.map((post) => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.content}</p>
          <small>{new Date(post.created_at).toLocaleDateString()}</small>
        </article>
      ))}
    </div>
  );
}
```

### Vue.js

```vue
<template>
  <div v-if="data">
    <h1>{{ data.site.name }}</h1>
    <p>{{ data.site.description }}</p>

    <article v-for="post in posts" :key="post.id">
      <h2>{{ post.title }}</h2>
      <p>{{ post.content }}</p>
      <small>{{ new Date(post.created_at).toLocaleDateString() }}</small>
    </article>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data: null,
    };
  },

  async mounted() {
    const response = await fetch("http://localhost:8080/my-blog/data");
    this.data = await response.json();
  },

  computed: {
    posts() {
      return (
        this.data?.collections.find((c) => c.slug === "posts").entries || []
      );
    },
  },
};
</script>
```

---

## 🔄 Step 9: Explore More Operations

### List All Sites

```bash
curl -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  http://localhost:8080/api/sites
```

### Update an Entry

```bash
curl -X PUT http://localhost:8080/api/entries/1 \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Welcome to My Blog - Updated!",
    "content": "This is my updated first blog post using BareCMS!"
  }'
```

### Delete an Entry

```bash
curl -X DELETE http://localhost:8080/api/entries/1 \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### Add Another Collection

```bash
curl -X POST http://localhost:8080/api/sites/1/collections \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Pages",
    "slug": "pages",
    "description": "Static pages"
  }'
```

---

## ✅ What You've Accomplished

In just 15 minutes, you've:

- ✅ **Set up BareCMS** with Docker
- ✅ **Created an admin account** for content management
- ✅ **Built a complete blog structure** (site → collection → entries)
- ✅ **Added multiple blog posts** with rich content
- ✅ **Accessed content publicly** via the data API
- ✅ **Learned integration patterns** for popular frameworks
- ✅ **Explored CRUD operations** for content management

---

## 🚀 Next Steps

Now that you understand the basics:

1. **[Build a Complete Site →](first-site.md)** - Create a more complex example
2. **[Learn the Full API →](../api/README.md)** - Explore all endpoints
3. **[See Integration Examples →](../integration/frontend-examples.md)** - Real-world framework usage
4. **[Deploy to Production →](../deployment/self-hosting.md)** - Make it live
5. **[Explore Use Cases →](../guides/use-cases.md)** - Get inspired

---

## 💡 Pro Tips

- **Use the Web UI**: Visit [http://localhost:8080](http://localhost:8080) for a visual interface
- **Bookmark the Public API**: `http://localhost:8080/{siteSlug}/data` is your main endpoint
- **Plan Your Structure**: Design your sites and collections before adding entries
- **Use Meaningful Slugs**: They become part of your URLs
- **Test Everything**: The API is your source of truth

---

_Ready to build something bigger? Let's create a complete site in the next tutorial! 🎯_
