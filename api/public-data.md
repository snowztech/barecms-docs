# Public Data API

The **Public Data API** is the heart of BareCMS's headless architecture. This single endpoint provides all your site's content without authentication - perfect for frontend consumption.

## 🌐 Overview

The public data endpoint is designed for **frontend consumption** and provides:

- ✅ **All site content** in one request
- ✅ **No authentication required**
- ✅ **Fast, cacheable responses**
- ✅ **Consistent JSON structure**
- ✅ **Real-time content updates**

---

## 🚀 The Key Endpoint

### `GET /:siteSlug/data`

This is the **most important endpoint** in BareCMS. It returns everything you need to build your frontend.

**Example Request:**

```bash
curl https://your-barecms.com/my-blog/data
```

**Response Structure:**

```json
{
  "site": {
    "id": 1,
    "name": "My Blog",
    "slug": "my-blog",
    "description": "A simple blog built with BareCMS",
    "created_at": "2024-01-15T10:00:00Z"
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
          "content": "This is my first blog post using BareCMS!",
          "slug": "welcome-to-barecms",
          "created_at": "2024-01-15T10:30:00Z",
          "updated_at": "2024-01-15T10:30:00Z"
        },
        {
          "id": 2,
          "title": "Why I Chose BareCMS",
          "content": "BareCMS is incredibly simple yet powerful...",
          "slug": "why-i-chose-barecms",
          "created_at": "2024-01-15T11:00:00Z",
          "updated_at": "2024-01-15T11:00:00Z"
        }
      ]
    },
    {
      "id": 2,
      "name": "Pages",
      "slug": "pages",
      "description": "Static pages",
      "entries": [
        {
          "id": 3,
          "title": "About",
          "content": "Learn more about our blog and mission...",
          "slug": "about",
          "created_at": "2024-01-15T10:15:00Z",
          "updated_at": "2024-01-15T10:15:00Z"
        }
      ]
    }
  ]
}
```

---

## 💻 Usage Examples

### JavaScript/Fetch

```javascript
async function getSiteData(siteSlug) {
  try {
    const response = await fetch(`https://your-barecms.com/${siteSlug}/data`);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Error fetching site data:", error);
    throw error;
  }
}

// Usage
const blogData = await getSiteData("my-blog");
console.log("Site:", blogData.site.name);
console.log(
  "Posts:",
  blogData.collections.find((c) => c.slug === "posts").entries
);
```

### React Hook

```jsx
import { useState, useEffect } from "react";

function useSiteData(siteSlug) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchData() {
      try {
        setLoading(true);
        const response = await fetch(
          `https://your-barecms.com/${siteSlug}/data`
        );

        if (!response.ok) {
          throw new Error(`Failed to fetch: ${response.status}`);
        }

        const result = await response.json();
        setData(result);
        setError(null);
      } catch (err) {
        setError(err.message);
        setData(null);
      } finally {
        setLoading(false);
      }
    }

    if (siteSlug) {
      fetchData();
    }
  }, [siteSlug]);

  return { data, loading, error };
}

// Usage in component
function BlogList() {
  const { data, loading, error } = useSiteData("my-blog");

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  const posts = data.collections.find((c) => c.slug === "posts")?.entries || [];

  return (
    <div>
      <h1>{data.site.name}</h1>
      {posts.map((post) => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.content}</p>
        </article>
      ))}
    </div>
  );
}
```

### Vue.js Composable

```javascript
import { ref, onMounted } from "vue";

export function useSiteData(siteSlug) {
  const data = ref(null);
  const loading = ref(true);
  const error = ref(null);

  const fetchData = async () => {
    try {
      loading.value = true;
      const response = await fetch(`https://your-barecms.com/${siteSlug}/data`);

      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }

      data.value = await response.json();
    } catch (err) {
      error.value = err.message;
    } finally {
      loading.value = false;
    }
  };

  onMounted(fetchData);

  return {
    data,
    loading,
    error,
    refresh: fetchData,
  };
}
```

### Next.js Static Generation

```javascript
// pages/blog.js
export default function Blog({ siteData }) {
  const posts =
    siteData.collections.find((c) => c.slug === "posts")?.entries || [];

  return (
    <div>
      <h1>{siteData.site.name}</h1>
      {posts.map((post) => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <div dangerouslySetInnerHTML={{ __html: post.content }} />
        </article>
      ))}
    </div>
  );
}

export async function getStaticProps() {
  const response = await fetch("https://your-barecms.com/my-blog/data");
  const siteData = await response.json();

  return {
    props: {
      siteData,
    },
    revalidate: 3600, // Revalidate every hour
  };
}
```

---

## 🛠️ Helper Utilities

### Data Access Helpers

```javascript
class SiteDataHelper {
  constructor(data) {
    this.data = data;
  }

  get site() {
    return this.data.site;
  }

  get collections() {
    return this.data.collections;
  }

  getCollection(slug) {
    return this.collections.find((collection) => collection.slug === slug);
  }

  getEntries(collectionSlug) {
    const collection = this.getCollection(collectionSlug);
    return collection ? collection.entries : [];
  }

  getEntry(collectionSlug, entrySlug) {
    const entries = this.getEntries(collectionSlug);
    return entries.find((entry) => entry.slug === entrySlug);
  }

  searchEntries(query, collectionSlug = null) {
    const searchCollections = collectionSlug
      ? [this.getCollection(collectionSlug)].filter(Boolean)
      : this.collections;

    const results = [];
    const searchTerm = query.toLowerCase();

    searchCollections.forEach((collection) => {
      collection.entries.forEach((entry) => {
        const searchText = `${entry.title} ${entry.content}`.toLowerCase();
        if (searchText.includes(searchTerm)) {
          results.push({
            ...entry,
            collection: collection.slug,
          });
        }
      });
    });

    return results;
  }

  filterEntries(collectionSlug, filterFn) {
    const entries = this.getEntries(collectionSlug);
    return entries.filter(filterFn);
  }

  sortEntries(collectionSlug, sortFn) {
    const entries = this.getEntries(collectionSlug);
    return [...entries].sort(sortFn);
  }
}

// Usage
const helper = new SiteDataHelper(siteData);

// Get specific content
const aboutPage = helper.getEntry("pages", "about");
const latestPosts = helper.getEntries("posts").slice(0, 5);

// Search
const searchResults = helper.searchEntries("BareCMS");

// Filter recent posts
const recentPosts = helper.filterEntries("posts", (entry) => {
  const oneWeekAgo = new Date();
  oneWeekAgo.setDate(oneWeekAgo.getDate() - 7);
  return new Date(entry.created_at) > oneWeekAgo;
});

// Sort by date
const sortedPosts = helper.sortEntries(
  "posts",
  (a, b) => new Date(b.created_at) - new Date(a.created_at)
);
```

### TypeScript Types

```typescript
export interface Site {
  id: number;
  name: string;
  slug: string;
  description: string;
  created_at: string;
}

export interface Entry {
  id: number;
  title: string;
  content: string;
  slug: string;
  created_at: string;
  updated_at: string;
  [key: string]: any; // For custom fields
}

export interface Collection {
  id: number;
  name: string;
  slug: string;
  description: string;
  entries: Entry[];
}

export interface SiteData {
  site: Site;
  collections: Collection[];
}

// Utility type for getting entries from a specific collection
export type CollectionEntries<T extends string> = Entry[];

// Usage
function processBlogData(data: SiteData): void {
  const posts: CollectionEntries<"posts"> =
    data.collections.find((c) => c.slug === "posts")?.entries || [];

  posts.forEach((post) => {
    console.log(`${post.title}: ${post.content.substring(0, 100)}...`);
  });
}
```

---

## ⚡ Performance Optimization

### Caching Strategy

```javascript
class CachedSiteData {
  constructor(cacheTimeout = 5 * 60 * 1000) {
    // 5 minutes
    this.cache = new Map();
    this.cacheTimeout = cacheTimeout;
  }

  async getSiteData(siteSlug, useCache = true) {
    const cacheKey = `site-${siteSlug}`;

    if (useCache) {
      const cached = this.cache.get(cacheKey);
      if (cached && Date.now() - cached.timestamp < this.cacheTimeout) {
        console.log("Cache hit for", siteSlug);
        return cached.data;
      }
    }

    console.log("Fetching fresh data for", siteSlug);
    const data = await this.fetchSiteData(siteSlug);

    // Cache the result
    this.cache.set(cacheKey, {
      data,
      timestamp: Date.now(),
    });

    return data;
  }

  async fetchSiteData(siteSlug) {
    const response = await fetch(`https://your-barecms.com/${siteSlug}/data`);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    return response.json();
  }

  clearCache(siteSlug = null) {
    if (siteSlug) {
      this.cache.delete(`site-${siteSlug}`);
    } else {
      this.cache.clear();
    }
  }
}

// Usage
const cache = new CachedSiteData();
const data = await cache.getSiteData("my-blog"); // Fresh fetch
const data2 = await cache.getSiteData("my-blog"); // Cache hit
```

### Request Optimization

```javascript
// Add headers for better caching and performance
async function fetchSiteDataOptimized(siteSlug, options = {}) {
  const url = `https://your-barecms.com/${siteSlug}/data`;

  const response = await fetch(url, {
    headers: {
      Accept: "application/json",
      "Accept-Encoding": "gzip, deflate, br",
      "Cache-Control": "max-age=300", // 5 minutes
      ...options.headers,
    },
    ...options,
  });

  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }

  return response.json();
}
```

---

## 🔒 Security Considerations

### Data Validation

```javascript
function validateSiteData(data) {
  if (!data || typeof data !== "object") {
    throw new Error("Invalid data structure");
  }

  if (!data.site || !data.site.slug || !data.site.name) {
    throw new Error("Invalid site data");
  }

  if (!Array.isArray(data.collections)) {
    throw new Error("Collections must be an array");
  }

  // Validate each collection
  data.collections.forEach((collection, index) => {
    if (!collection.slug || !collection.name) {
      throw new Error(`Invalid collection at index ${index}`);
    }

    if (!Array.isArray(collection.entries)) {
      throw new Error(
        `Entries must be an array for collection ${collection.slug}`
      );
    }

    // Validate each entry
    collection.entries.forEach((entry, entryIndex) => {
      if (!entry.id || !entry.title || !entry.slug) {
        throw new Error(
          `Invalid entry at index ${entryIndex} in collection ${collection.slug}`
        );
      }
    });
  });

  return true;
}

// Usage
try {
  const data = await fetchSiteData("my-blog");
  validateSiteData(data);
  // Proceed with valid data
} catch (error) {
  console.error("Data validation failed:", error);
  // Handle error appropriately
}
```

### Content Sanitization

```javascript
import DOMPurify from "dompurify";

function sanitizeContent(content) {
  return DOMPurify.sanitize(content, {
    ALLOWED_TAGS: [
      "p",
      "br",
      "strong",
      "em",
      "h1",
      "h2",
      "h3",
      "h4",
      "h5",
      "h6",
      "ul",
      "ol",
      "li",
      "a",
    ],
    ALLOWED_ATTR: ["href", "target"],
  });
}

// Usage in React
function BlogPost({ entry }) {
  const sanitizedContent = sanitizeContent(entry.content);

  return (
    <article>
      <h1>{entry.title}</h1>
      <div dangerouslySetInnerHTML={{ __html: sanitizedContent }} />
    </article>
  );
}
```

---

## 🐛 Error Handling

### Comprehensive Error Handling

```javascript
class SiteDataError extends Error {
  constructor(message, code, originalError = null) {
    super(message);
    this.name = "SiteDataError";
    this.code = code;
    this.originalError = originalError;
  }
}

async function robustFetchSiteData(siteSlug, options = {}) {
  const maxRetries = options.maxRetries || 3;
  const retryDelay = options.retryDelay || 1000;

  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      const response = await fetch(
        `https://your-barecms.com/${siteSlug}/data`,
        {
          timeout: options.timeout || 10000,
          ...options.fetchOptions,
        }
      );

      if (!response.ok) {
        if (response.status === 404) {
          throw new SiteDataError(
            `Site '${siteSlug}' not found`,
            "SITE_NOT_FOUND"
          );
        }

        if (response.status >= 500) {
          throw new SiteDataError("Server error occurred", "SERVER_ERROR");
        }

        throw new SiteDataError(
          `HTTP ${response.status}: ${response.statusText}`,
          "HTTP_ERROR"
        );
      }

      const data = await response.json();
      validateSiteData(data);
      return data;
    } catch (error) {
      if (error instanceof SiteDataError) {
        // Don't retry for client errors
        if (error.code === "SITE_NOT_FOUND") {
          throw error;
        }
      }

      if (attempt === maxRetries) {
        throw new SiteDataError(
          "Failed to fetch site data after multiple attempts",
          "FETCH_FAILED",
          error
        );
      }

      // Wait before retrying
      await new Promise((resolve) => setTimeout(resolve, retryDelay));
    }
  }
}

// Usage with error handling
try {
  const data = await robustFetchSiteData("my-blog");
  // Success - use data
} catch (error) {
  if (error instanceof SiteDataError) {
    switch (error.code) {
      case "SITE_NOT_FOUND":
        console.error("Site not found - check the slug");
        break;
      case "SERVER_ERROR":
        console.error("Server error - try again later");
        break;
      case "FETCH_FAILED":
        console.error("Network error - check connectivity");
        break;
      default:
        console.error("Unknown error:", error.message);
    }
  } else {
    console.error("Unexpected error:", error);
  }
}
```

---

## 📊 Response Format Details

### Site Object

| Field         | Type   | Description              |
| ------------- | ------ | ------------------------ |
| `id`          | number | Unique site identifier   |
| `name`        | string | Display name of the site |
| `slug`        | string | URL-friendly identifier  |
| `description` | string | Site description         |
| `created_at`  | string | ISO 8601 timestamp       |

### Collection Object

| Field         | Type    | Description                         |
| ------------- | ------- | ----------------------------------- |
| `id`          | number  | Unique collection identifier        |
| `name`        | string  | Display name of the collection      |
| `slug`        | string  | URL-friendly identifier             |
| `description` | string  | Collection description              |
| `entries`     | Entry[] | Array of entries in this collection |

### Entry Object

| Field        | Type   | Description                      |
| ------------ | ------ | -------------------------------- |
| `id`         | number | Unique entry identifier          |
| `title`      | string | Entry title                      |
| `content`    | string | Entry content (may contain HTML) |
| `slug`       | string | URL-friendly identifier          |
| `created_at` | string | ISO 8601 timestamp               |
| `updated_at` | string | ISO 8601 timestamp               |

---

## 🌟 Best Practices

### 1. Error Handling

- Always handle network errors gracefully
- Provide meaningful error messages to users
- Implement retry logic for transient failures
- Validate data structure before using

### 2. Performance

- Cache responses to reduce API calls
- Use appropriate HTTP cache headers
- Implement loading states for better UX
- Consider pagination for large datasets

### 3. Security

- Validate and sanitize all content before rendering
- Use HTTPS in production
- Implement proper CORS headers on your server
- Never trust user-generated content

### 4. User Experience

- Show loading states during API calls
- Provide fallback content when API is unavailable
- Implement search and filtering for better navigation
- Use semantic HTML for accessibility

---

## 🔗 Related Resources

- **[Frontend Examples](../integration/frontend-examples.md)** - Complete framework implementations
- **[API Overview](README.md)** - Full API reference
- **[Performance Guide](../guides/performance.md)** - Optimization techniques
- **[Security Guide](../guides/security.md)** - Security best practices

---

**The Public Data API is your gateway to headless content delivery.** Build fast, flexible frontends with any technology you choose! 🚀
