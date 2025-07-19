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
