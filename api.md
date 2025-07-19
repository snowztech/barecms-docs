# BareCMS API Documentation

## Overview

BareCMS provides a RESTful API for content management and public data access. All authenticated endpoints require a JWT token in the Authorization header.

## 📋 Table of Contents

- [🌐 Base URL](#-base-url)
- [🔐 Authentication](#-authentication)
- [🌍 Public Endpoints](#-public-endpoints)
- [🔑 Authentication Endpoints](#-authentication-endpoints)
- [🏢 Sites Management](#-sites-management)
- [📚 Collections Management](#-collections-management)
- [📝 Entries Management](#-entries-management)
- [❌ Error Responses](#-error-responses)
- [💡 Usage Examples](#-usage-examples)

---

## 🌐 Base URL

```
http://localhost:8080
```

For production deployments, replace `localhost:8080` with your actual domain.

---

## 🔐 Authentication

Include the JWT token in the Authorization header for all authenticated endpoints:

```
Authorization: Bearer <your-jwt-token>
```

**Important**: The JWT token expires after 24 hours. You'll need to login again to get a new token.

---

## 🌍 Public Endpoints

### Get Site Data

Retrieve all site content publicly without authentication.

**Endpoint:** `GET /:siteSlug/data`

**Description:** Returns all collections and entries for a site using its slug. This is the primary endpoint for headless usage.

**Parameters:**

- `siteSlug` (path, required) - The unique slug of the site

**Example Request:**

```bash
curl -X GET http://localhost:8080/my-blog/data
```

**Example Response:**

```json
{
  "site": {
    "id": 1,
    "name": "My Blog",
    "slug": "my-blog",
    "description": "A simple blog site"
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
          "content": "This is my first blog post using BareCMS...",
          "slug": "welcome-to-barecms",
          "created_at": "2024-01-15T10:30:00Z",
          "updated_at": "2024-01-15T10:30:00Z"
        },
        {
          "id": 2,
          "title": "Getting Started Guide",
          "content": "Here's how to get started with BareCMS...",
          "slug": "getting-started-guide",
          "created_at": "2024-01-16T14:22:00Z",
          "updated_at": "2024-01-16T14:22:00Z"
        }
      ]
    },
    {
      "id": 2,
      "name": "Pages",
      "slug": "pages",
      "description": "Static pages collection",
      "entries": [
        {
          "id": 3,
          "title": "About",
          "content": "Learn more about our mission...",
          "slug": "about",
          "created_at": "2024-01-15T11:00:00Z",
          "updated_at": "2024-01-15T11:00:00Z"
        }
      ]
    }
  ]
}
```

---

## 🔑 Authentication Endpoints

### Register User

Create a new user account.

**Endpoint:** `POST /api/auth/register`

**Request Body:**

```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response:**

```json
{
  "message": "User registered successfully",
  "user": {
    "id": 1,
    "email": "user@example.com"
  }
}
```

### Login User

Authenticate and receive a JWT token.

**Endpoint:** `POST /api/auth/login`

**Request Body:**

```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response:**

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "email": "user@example.com"
  }
}
```

### Logout User

Invalidate the current session.

**Endpoint:** `POST /api/auth/logout`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "message": "Logged out successfully"
}
```

### Get Current User

Get details of the currently authenticated user.

**Endpoint:** `GET /api/auth/user`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "user": {
    "id": 1,
    "email": "user@example.com",
    "created_at": "2024-01-15T10:00:00Z"
  }
}
```

---

## 🏢 Sites Management

### List Sites

Get all sites for the authenticated user.

**Endpoint:** `GET /api/sites`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "sites": [
    {
      "id": 1,
      "name": "My Blog",
      "slug": "my-blog",
      "description": "A simple blog site",
      "created_at": "2024-01-15T10:00:00Z",
      "updated_at": "2024-01-15T10:00:00Z"
    }
  ]
}
```

### Create Site

Create a new site.

**Endpoint:** `POST /api/sites`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Request Body:**

```json
{
  "name": "My Portfolio",
  "slug": "my-portfolio",
  "description": "Personal portfolio website"
}
```

**Response:**

```json
{
  "site": {
    "id": 2,
    "name": "My Portfolio",
    "slug": "my-portfolio",
    "description": "Personal portfolio website",
    "created_at": "2024-01-16T09:00:00Z",
    "updated_at": "2024-01-16T09:00:00Z"
  }
}
```

### Get Site Details

Retrieve details of a specific site.

**Endpoint:** `GET /api/sites/:id`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "site": {
    "id": 1,
    "name": "My Blog",
    "slug": "my-blog",
    "description": "A simple blog site",
    "created_at": "2024-01-15T10:00:00Z",
    "updated_at": "2024-01-15T10:00:00Z"
  }
}
```

### Get Site with Collections

Retrieve a site with all its collections.

**Endpoint:** `GET /api/sites/:id/with-collections`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "site": {
    "id": 1,
    "name": "My Blog",
    "slug": "my-blog",
    "description": "A simple blog site",
    "collections": [
      {
        "id": 1,
        "name": "Posts",
        "slug": "posts",
        "description": "Blog posts collection"
      }
    ]
  }
}
```

### Update Site

Update an existing site.

**Endpoint:** `PUT /api/sites/:id`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Request Body:**

```json
{
  "name": "My Updated Blog",
  "description": "Updated description"
}
```

**Response:**

```json
{
  "site": {
    "id": 1,
    "name": "My Updated Blog",
    "slug": "my-blog",
    "description": "Updated description",
    "updated_at": "2024-01-16T15:30:00Z"
  }
}
```

### Delete Site

Delete a site and all its collections/entries.

**Endpoint:** `DELETE /api/sites/:id`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "message": "Site deleted successfully"
}
```

---

## 📚 Collections Management

### List Collections

Get all collections for a specific site.

**Endpoint:** `GET /api/sites/:siteId/collections`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "collections": [
    {
      "id": 1,
      "name": "Posts",
      "slug": "posts",
      "description": "Blog posts collection",
      "site_id": 1,
      "created_at": "2024-01-15T10:15:00Z",
      "updated_at": "2024-01-15T10:15:00Z"
    }
  ]
}
```

### Create Collection

Create a new collection within a site.

**Endpoint:** `POST /api/sites/:siteId/collections`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Request Body:**

```json
{
  "name": "Products",
  "slug": "products",
  "description": "Product catalog collection"
}
```

**Response:**

```json
{
  "collection": {
    "id": 2,
    "name": "Products",
    "slug": "products",
    "description": "Product catalog collection",
    "site_id": 1,
    "created_at": "2024-01-16T10:00:00Z",
    "updated_at": "2024-01-16T10:00:00Z"
  }
}
```

### Get Collection Details

Retrieve details of a specific collection.

**Endpoint:** `GET /api/collections/:id`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
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
    "created_at": "2024-01-15T10:15:00Z",
    "updated_at": "2024-01-15T10:15:00Z"
  }
}
```

### Update Collection

Update an existing collection.

**Endpoint:** `PUT /api/collections/:id`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Request Body:**

```json
{
  "name": "Updated Collection Name",
  "description": "Updated description"
}
```

### Delete Collection

Delete a collection and all its entries.

**Endpoint:** `DELETE /api/collections/:id`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "message": "Collection deleted successfully"
}
```

---

## 📝 Entries Management

### List Entries

Get all entries for a specific collection.

**Endpoint:** `GET /api/collections/:collectionId/entries`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "entries": [
    {
      "id": 1,
      "title": "Welcome to BareCMS",
      "content": "This is my first blog post...",
      "slug": "welcome-to-barecms",
      "collection_id": 1,
      "created_at": "2024-01-15T10:30:00Z",
      "updated_at": "2024-01-15T10:30:00Z"
    }
  ]
}
```

### Create Entry

Create a new entry within a collection.

**Endpoint:** `POST /api/collections/:collectionId/entries`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Request Body:**

```json
{
  "title": "New Blog Post",
  "content": "Content of the blog post...",
  "slug": "new-blog-post"
}
```

**Response:**

```json
{
  "entry": {
    "id": 2,
    "title": "New Blog Post",
    "content": "Content of the blog post...",
    "slug": "new-blog-post",
    "collection_id": 1,
    "created_at": "2024-01-16T11:00:00Z",
    "updated_at": "2024-01-16T11:00:00Z"
  }
}
```

### Get Entry Details

Retrieve details of a specific entry.

**Endpoint:** `GET /api/entries/:id`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "entry": {
    "id": 1,
    "title": "Welcome to BareCMS",
    "content": "This is my first blog post...",
    "slug": "welcome-to-barecms",
    "collection_id": 1,
    "created_at": "2024-01-15T10:30:00Z",
    "updated_at": "2024-01-15T10:30:00Z"
  }
}
```

### Update Entry

Update an existing entry.

**Endpoint:** `PUT /api/entries/:id`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Request Body:**

```json
{
  "title": "Updated Blog Post Title",
  "content": "Updated content..."
}
```

**Response:**

```json
{
  "entry": {
    "id": 1,
    "title": "Updated Blog Post Title",
    "content": "Updated content...",
    "slug": "welcome-to-barecms",
    "collection_id": 1,
    "updated_at": "2024-01-16T12:00:00Z"
  }
}
```

### Delete Entry

Delete an entry.

**Endpoint:** `DELETE /api/entries/:id`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

**Response:**

```json
{
  "message": "Entry deleted successfully"
}
```

---

## ❌ Error Responses

All endpoints may return the following error responses:

### 400 Bad Request

```json
{
  "error": "Invalid request data",
  "details": "Specific validation error details"
}
```

### 401 Unauthorized

```json
{
  "error": "Authentication required"
}
```

### 403 Forbidden

```json
{
  "error": "Access denied"
}
```

### 404 Not Found

```json
{
  "error": "Resource not found"
}
```

### 409 Conflict

```json
{
  "error": "Resource already exists",
  "details": "Slug already taken"
}
```

### 500 Internal Server Error

```json
{
  "error": "Internal server error"
}
```

---

## 💡 Usage Examples

### Complete Headless CMS Workflow

This example demonstrates the complete workflow from setup to public data access:

#### 1. **Register and Login**

```bash
# Register a new user
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "securepassword123"
  }'

# Login to get JWT token
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "securepassword123"
  }'

# Save the returned token for use in subsequent requests
TOKEN="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

#### 2. **Create Site Structure**

```bash
# Create a new site
curl -X POST http://localhost:8080/api/sites \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My Blog",
    "slug": "my-blog",
    "description": "Personal blog powered by BareCMS"
  }'

# Create a collection for blog posts
curl -X POST http://localhost:8080/api/sites/1/collections \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Posts",
    "slug": "posts",
    "description": "Blog posts collection"
  }'

# Create another collection for pages
curl -X POST http://localhost:8080/api/sites/1/collections \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Pages",
    "slug": "pages",
    "description": "Static pages collection"
  }'
```

#### 3. **Add Content**

```bash
# Create a blog post
curl -X POST http://localhost:8080/api/collections/1/entries \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Welcome to My Blog",
    "content": "This is my first blog post using BareCMS. It's incredibly simple yet powerful!",
    "slug": "welcome-to-my-blog"
  }'

# Create an about page
curl -X POST http://localhost:8080/api/collections/2/entries \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "About Me",
    "content": "I'm a developer who loves simple, effective tools. BareCMS is perfect for my headless content needs.",
    "slug": "about"
  }'
```

#### 4. **Access Data Publicly**

```bash
# Get all site data (no authentication needed!)
curl -X GET http://localhost:8080/my-blog/data
```

This will return all your content in a structured format ready for consumption by any frontend application.

### Frontend Integration Examples

#### React/Next.js Example

```javascript
// pages/blog.js
import { useState, useEffect } from "react";

export default function Blog() {
  const [siteData, setSiteData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("https://your-barecms.com/my-blog/data")
      .then((res) => res.json())
      .then((data) => {
        setSiteData(data);
        setLoading(false);
      });
  }, []);

  if (loading) return <div>Loading...</div>;

  const posts =
    siteData.collections.find((c) => c.slug === "posts")?.entries || [];

  return (
    <div>
      <h1>{siteData.site.name}</h1>
      <p>{siteData.site.description}</p>

      {posts.map((post) => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.content}</p>
          <time>{new Date(post.created_at).toLocaleDateString()}</time>
        </article>
      ))}
    </div>
  );
}
```

#### Static Site Generation (Gatsby)

```javascript
// gatsby-node.js
exports.sourceNodes = async ({
  actions,
  createNodeId,
  createContentDigest,
}) => {
  const { createNode } = actions;

  const response = await fetch("https://your-barecms.com/my-blog/data");
  const data = await response.json();

  // Create nodes for each entry
  data.collections.forEach((collection) => {
    collection.entries.forEach((entry) => {
      createNode({
        ...entry,
        id: createNodeId(`BareCMS-${entry.id}`),
        internal: {
          type: "BareCMSEntry",
          contentDigest: createContentDigest(entry),
        },
      });
    });
  });
};
```

This workflow demonstrates the core concept: **use the authenticated API to manage content, then access it publicly via the site slug for your frontend applications**.

---

**📚 Need more help?** Check out our [GitHub Discussions](https://github.com/snowztech/barecms/discussions) or explore the [complete documentation](/).
