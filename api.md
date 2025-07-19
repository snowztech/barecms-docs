# API Reference

BareCMS provides a RESTful API for content management and public data access.

## Base URL

```
http://localhost:8080
```

## Authentication

Include the JWT token in the Authorization header for authenticated endpoints:

```
Authorization: Bearer <your-jwt-token>
```

---

## 🌐 Public Endpoints

### Get Site Data

Retrieve all site content publicly without authentication.

**Endpoint:** `GET /:siteSlug/data`

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
        }
      ]
    }
  ]
}
```

---

## 🔐 Authentication Endpoints

### Register User

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

### Get User

**Endpoint:** `GET /api/auth/user`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

### Delete User

**Endpoint:** `DELETE /api/auth/user/:userId`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

---

## 🏢 Sites Management

### List Sites

**Endpoint:** `GET /api/sites`

**Headers:**

```
Authorization: Bearer <your-jwt-token>
```

### Create Site

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

### Get Site

**Endpoint:** `GET /api/sites/:id`

### Get Site with Collections

**Endpoint:** `GET /api/sites/:id/with-collections`

### Delete Site

**Endpoint:** `DELETE /api/sites/:id`

### Get Site Collections

**Endpoint:** `GET /api/sites/:id/collections`

---

## 📚 Collections Management

### Create Collection

**Endpoint:** `POST /api/collections`

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

### Get Collection

**Endpoint:** `GET /api/collections/:id`

### Delete Collection

**Endpoint:** `DELETE /api/collections/:id`

### Get Collection Entries

**Endpoint:** `GET /api/collections/:id/entries`

---

## 📝 Entries Management

### Create Entry

**Endpoint:** `POST /api/entries`

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

### Get Entry

**Endpoint:** `GET /api/entries/:id`

### Delete Entry

**Endpoint:** `DELETE /api/entries/:id`

---

## Error Responses

### 400 Bad Request

```json
{
  "error": "Invalid request data"
}
```

### 401 Unauthorized

```json
{
  "error": "Authentication required"
}
```

### 404 Not Found

```json
{
  "error": "Resource not found"
}
```

### 500 Internal Server Error

```json
{
  "error": "Internal server error"
}
```

---

## Usage Example

### Complete Workflow

1. **Register and Login**

```bash
# Register
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "password123"}'

# Login
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "password123"}'
```

2. **Create Content Structure**

```bash
# Create site
curl -X POST http://localhost:8080/api/sites \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name": "My Blog", "slug": "my-blog", "description": "Personal blog"}'

# Create collection
curl -X POST http://localhost:8080/api/collections \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name": "Posts", "slug": "posts", "description": "Blog posts"}'

# Create entry
curl -X POST http://localhost:8080/api/entries \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title": "Hello World", "content": "My first post!", "slug": "hello-world"}'
```

3. **Access Data Publicly**

```bash
# Get all site data (no authentication needed)
curl -X GET http://localhost:8080/my-blog/data
```

This demonstrates the core concept: use authenticated API to manage content, then access it publicly for frontend applications.
