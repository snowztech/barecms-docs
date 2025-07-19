# Frontend Integration Examples

Learn how to integrate BareCMS with popular frontend frameworks. All examples use the public data API - no authentication required!

## 🎯 Public Data API

All examples fetch data from:

```
GET https://your-barecms.com/:siteSlug/data
```

This endpoint returns all your site's collections and entries in a single request.

---

## ⚛️ React Example

### Basic React Component

```jsx
import React, { useState, useEffect } from "react";

function BlogPosts() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("https://your-barecms.com/my-blog/data")
      .then((response) => response.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch((error) => {
        console.error("Error:", error);
        setLoading(false);
      });
  }, []);

  if (loading) return <div>Loading...</div>;
  if (!data) return <div>Error loading content</div>;

  // Find the Posts collection
  const postsCollection = data.collections.find((col) => col.slug === "posts");
  const posts = postsCollection?.entries || [];

  return (
    <div>
      <h1>{data.site.name}</h1>
      <p>{data.site.description}</p>

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

export default BlogPosts;
```

### React with TypeScript

```tsx
import React, { useState, useEffect } from "react";

interface Site {
  id: number;
  name: string;
  slug: string;
  description: string;
}

interface Entry {
  id: number;
  title: string;
  content: string;
  slug: string;
  created_at: string;
}

interface Collection {
  id: number;
  name: string;
  slug: string;
  description: string;
  entries: Entry[];
}

interface BareCMSData {
  site: Site;
  collections: Collection[];
}

function TypedBlogPosts() {
  const [data, setData] = useState<BareCMSData | null>(null);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch("https://your-barecms.com/my-blog/data");
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const data: BareCMSData = await response.json();
        setData(data);
      } catch (err) {
        setError(err instanceof Error ? err.message : "An error occurred");
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!data) return <div>No data available</div>;

  const postsCollection = data.collections.find((col) => col.slug === "posts");
  const posts = postsCollection?.entries || [];

  return (
    <div>
      <header>
        <h1>{data.site.name}</h1>
        <p>{data.site.description}</p>
      </header>

      <main>
        {posts.map((post) => (
          <article key={post.id}>
            <h2>{post.title}</h2>
            <div dangerouslySetInnerHTML={{ __html: post.content }} />
            <footer>
              <time dateTime={post.created_at}>
                {new Date(post.created_at).toLocaleDateString()}
              </time>
            </footer>
          </article>
        ))}
      </main>
    </div>
  );
}

export default TypedBlogPosts;
```

### React Custom Hook

```tsx
import { useState, useEffect } from "react";

interface BareCMSData {
  site: {
    id: number;
    name: string;
    slug: string;
    description: string;
  };
  collections: Array<{
    id: number;
    name: string;
    slug: string;
    description: string;
    entries: Array<{
      id: number;
      title: string;
      content: string;
      slug: string;
      created_at: string;
    }>;
  }>;
}

export function useBareCMS(siteSlug: string) {
  const [data, setData] = useState<BareCMSData | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchData = async () => {
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
        setError(err instanceof Error ? err.message : "Unknown error");
        setData(null);
      } finally {
        setLoading(false);
      }
    };

    if (siteSlug) {
      fetchData();
    }
  }, [siteSlug]);

  const getCollection = (slug: string) =>
    data?.collections.find((col) => col.slug === slug);

  const getEntry = (collectionSlug: string, entrySlug: string) =>
    getCollection(collectionSlug)?.entries.find(
      (entry) => entry.slug === entrySlug
    );

  return {
    data,
    loading,
    error,
    site: data?.site || null,
    collections: data?.collections || [],
    getCollection,
    getEntry,
  };
}

// Usage
function MyBlog() {
  const { data, loading, error, getCollection } = useBareCMS("my-blog");

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  const posts = getCollection("posts")?.entries || [];

  return (
    <div>
      <h1>{data?.site.name}</h1>
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

---

## 🟢 Vue.js Example

### Vue 3 Composition API

```vue
<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="error">Error: {{ error }}</div>
    <div v-else-if="data">
      <header>
        <h1>{{ data.site.name }}</h1>
        <p>{{ data.site.description }}</p>
      </header>

      <main>
        <article v-for="post in posts" :key="post.id">
          <h2>{{ post.title }}</h2>
          <div v-html="post.content"></div>
          <time>{{ formatDate(post.created_at) }}</time>
        </article>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from "vue";

interface BareCMSData {
  site: {
    id: number;
    name: string;
    slug: string;
    description: string;
  };
  collections: Array<{
    id: number;
    name: string;
    slug: string;
    description: string;
    entries: Array<{
      id: number;
      title: string;
      content: string;
      slug: string;
      created_at: string;
    }>;
  }>;
}

const data = ref<BareCMSData | null>(null);
const loading = ref(true);
const error = ref<string | null>(null);

const posts = computed(() => {
  const postsCollection = data.value?.collections.find(
    (col) => col.slug === "posts"
  );
  return postsCollection?.entries || [];
});

const fetchData = async () => {
  try {
    loading.value = true;
    const response = await fetch("https://your-barecms.com/my-blog/data");

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    data.value = await response.json();
  } catch (err) {
    error.value = err instanceof Error ? err.message : "An error occurred";
  } finally {
    loading.value = false;
  }
};

const formatDate = (dateString: string) => {
  return new Date(dateString).toLocaleDateString();
};

onMounted(fetchData);
</script>
```

### Vue 3 Options API

```vue
<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="error">Error: {{ error }}</div>
    <div v-else-if="data">
      <h1>{{ data.site.name }}</h1>
      <p>{{ data.site.description }}</p>

      <article v-for="post in posts" :key="post.id">
        <h2>{{ post.title }}</h2>
        <p>{{ post.content }}</p>
        <small>{{ formatDate(post.created_at) }}</small>
      </article>
    </div>
  </div>
</template>

<script>
export default {
  name: "BlogPosts",

  data() {
    return {
      data: null,
      loading: true,
      error: null,
    };
  },

  computed: {
    posts() {
      const postsCollection = this.data?.collections.find(
        (col) => col.slug === "posts"
      );
      return postsCollection?.entries || [];
    },
  },

  async mounted() {
    await this.fetchData();
  },

  methods: {
    async fetchData() {
      try {
        this.loading = true;
        const response = await fetch("https://your-barecms.com/my-blog/data");

        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }

        this.data = await response.json();
      } catch (error) {
        this.error = error.message;
      } finally {
        this.loading = false;
      }
    },

    formatDate(dateString) {
      return new Date(dateString).toLocaleDateString();
    },
  },
};
</script>
```

---

## ⚡ Next.js Examples

### Static Site Generation (SSG)

```tsx
// pages/blog.tsx
import { GetStaticProps } from "next";

interface BlogProps {
  data: {
    site: {
      name: string;
      description: string;
    };
    collections: Array<{
      slug: string;
      entries: Array<{
        id: number;
        title: string;
        content: string;
        slug: string;
        created_at: string;
      }>;
    }>;
  };
}

export default function Blog({ data }: BlogProps) {
  const posts =
    data.collections.find((col) => col.slug === "posts")?.entries || [];

  return (
    <div>
      <h1>{data.site.name}</h1>
      <p>{data.site.description}</p>

      {posts.map((post) => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <div dangerouslySetInnerHTML={{ __html: post.content }} />
          <time>{new Date(post.created_at).toLocaleDateString()}</time>
        </article>
      ))}
    </div>
  );
}

export const getStaticProps: GetStaticProps = async () => {
  const response = await fetch("https://your-barecms.com/my-blog/data");
  const data = await response.json();

  return {
    props: {
      data,
    },
    revalidate: 3600, // Revalidate every hour
  };
};
```

### Server-Side Rendering (SSR)

```tsx
// pages/blog-ssr.tsx
import { GetServerSideProps } from "next";

interface BlogSSRProps {
  data: any;
  error?: string;
}

export default function BlogSSR({ data, error }: BlogSSRProps) {
  if (error) {
    return <div>Error: {error}</div>;
  }

  const posts =
    data.collections.find((col: any) => col.slug === "posts")?.entries || [];

  return (
    <div>
      <h1>{data.site.name}</h1>
      {posts.map((post: any) => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.content}</p>
        </article>
      ))}
    </div>
  );
}

export const getServerSideProps: GetServerSideProps = async () => {
  try {
    const response = await fetch("https://your-barecms.com/my-blog/data");

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();

    return {
      props: {
        data,
      },
    };
  } catch (error) {
    return {
      props: {
        data: null,
        error: error instanceof Error ? error.message : "Unknown error",
      },
    };
  }
};
```

### App Router (Next.js 13+)

```tsx
// app/blog/page.tsx
async function getData() {
  const response = await fetch("https://your-barecms.com/my-blog/data", {
    next: { revalidate: 3600 }, // Revalidate every hour
  });

  if (!response.ok) {
    throw new Error("Failed to fetch data");
  }

  return response.json();
}

export default async function BlogPage() {
  const data = await getData();
  const posts =
    data.collections.find((col: any) => col.slug === "posts")?.entries || [];

  return (
    <div>
      <h1>{data.site.name}</h1>
      <p>{data.site.description}</p>

      {posts.map((post: any) => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <div dangerouslySetInnerHTML={{ __html: post.content }} />
          <time>{new Date(post.created_at).toLocaleDateString()}</time>
        </article>
      ))}
    </div>
  );
}
```

---

## 🎨 Svelte Example

```svelte
<!-- BlogPosts.svelte -->
<script>
  import { onMount } from 'svelte'

  let data = null
  let loading = true
  let error = null

  $: posts = data?.collections.find(col => col.slug === 'posts')?.entries || []

  onMount(async () => {
    try {
      const response = await fetch('https://your-barecms.com/my-blog/data')

      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`)
      }

      data = await response.json()
    } catch (err) {
      error = err.message
    } finally {
      loading = false
    }
  })

  function formatDate(dateString) {
    return new Date(dateString).toLocaleDateString()
  }
</script>

{#if loading}
  <div>Loading...</div>
{:else if error}
  <div>Error: {error}</div>
{:else if data}
  <div>
    <header>
      <h1>{data.site.name}</h1>
      <p>{data.site.description}</p>
    </header>

    <main>
      {#each posts as post (post.id)}
        <article>
          <h2>{post.title}</h2>
          <div>{@html post.content}</div>
          <time>{formatDate(post.created_at)}</time>
        </article>
      {/each}
    </main>
  </div>
{/if}

<style>
  article {
    margin-bottom: 2rem;
    padding: 1rem;
    border: 1px solid #ddd;
    border-radius: 4px;
  }

  h2 {
    margin-top: 0;
    color: #333;
  }

  time {
    color: #666;
    font-size: 0.9rem;
  }
</style>
```

---

## 📱 Vanilla JavaScript

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Blog</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
      }
      article {
        margin-bottom: 2rem;
        padding: 1rem;
        border: 1px solid #ddd;
        border-radius: 4px;
      }
      .loading {
        text-align: center;
        padding: 2rem;
      }
      .error {
        color: red;
        padding: 1rem;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <div class="loading">Loading...</div>
    </div>

    <script>
      async function fetchBlogData() {
        try {
          const response = await fetch("https://your-barecms.com/my-blog/data");

          if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
          }

          const data = await response.json();
          renderBlog(data);
        } catch (error) {
          renderError(error.message);
        }
      }

      function renderBlog(data) {
        const app = document.getElementById("app");
        const postsCollection = data.collections.find(
          (col) => col.slug === "posts"
        );
        const posts = postsCollection?.entries || [];

        app.innerHTML = `
                <header>
                    <h1>${data.site.name}</h1>
                    <p>${data.site.description}</p>
                </header>
                <main>
                    ${posts
                      .map(
                        (post) => `
                        <article>
                            <h2>${post.title}</h2>
                            <div>${post.content}</div>
                            <time>${new Date(post.created_at).toLocaleDateString()}</time>
                        </article>
                    `
                      )
                      .join("")}
                </main>
            `;
      }

      function renderError(message) {
        const app = document.getElementById("app");
        app.innerHTML = `<div class="error">Error: ${message}</div>`;
      }

      // Load blog data when page loads
      document.addEventListener("DOMContentLoaded", fetchBlogData);
    </script>
  </body>
</html>
```

---

## 🔧 Advanced Integration Patterns

### Error Handling & Retry Logic

```javascript
class BareCMSClient {
  constructor(baseUrl, options = {}) {
    this.baseUrl = baseUrl;
    this.retryAttempts = options.retryAttempts || 3;
    this.retryDelay = options.retryDelay || 1000;
  }

  async fetchSiteData(siteSlug, options = {}) {
    const url = `${this.baseUrl}/${siteSlug}/data`;

    for (let attempt = 1; attempt <= this.retryAttempts; attempt++) {
      try {
        const response = await fetch(url, {
          headers: {
            Accept: "application/json",
            ...options.headers,
          },
          ...options,
        });

        if (!response.ok) {
          throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }

        return await response.json();
      } catch (error) {
        if (attempt === this.retryAttempts) {
          throw error;
        }

        // Wait before retrying
        await new Promise((resolve) => setTimeout(resolve, this.retryDelay));
      }
    }
  }

  async getCollection(siteSlug, collectionSlug) {
    const data = await this.fetchSiteData(siteSlug);
    return data.collections.find((col) => col.slug === collectionSlug);
  }

  async getEntry(siteSlug, collectionSlug, entrySlug) {
    const collection = await this.getCollection(siteSlug, collectionSlug);
    return collection?.entries.find((entry) => entry.slug === entrySlug);
  }
}

// Usage
const cms = new BareCMSClient("https://your-barecms.com", {
  retryAttempts: 3,
  retryDelay: 1000,
});

try {
  const blogData = await cms.fetchSiteData("my-blog");
  const aboutPage = await cms.getEntry("my-blog", "pages", "about");
} catch (error) {
  console.error("Failed to fetch data:", error);
}
```

### Caching Strategy

```javascript
class CachedBareCMSClient {
  constructor(baseUrl, cacheTimeout = 5 * 60 * 1000) {
    // 5 minutes default
    this.baseUrl = baseUrl;
    this.cacheTimeout = cacheTimeout;
    this.cache = new Map();
  }

  async fetchSiteData(siteSlug, useCache = true) {
    const cacheKey = `site-${siteSlug}`;

    if (useCache) {
      const cached = this.cache.get(cacheKey);
      if (cached && Date.now() - cached.timestamp < this.cacheTimeout) {
        return cached.data;
      }
    }

    try {
      const response = await fetch(`${this.baseUrl}/${siteSlug}/data`);

      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }

      const data = await response.json();

      // Cache the result
      this.cache.set(cacheKey, {
        data,
        timestamp: Date.now(),
      });

      return data;
    } catch (error) {
      // If fetch fails, try to return stale cache
      const cached = this.cache.get(cacheKey);
      if (cached) {
        console.warn("Using stale cache due to fetch error:", error);
        return cached.data;
      }
      throw error;
    }
  }

  clearCache(siteSlug) {
    if (siteSlug) {
      this.cache.delete(`site-${siteSlug}`);
    } else {
      this.cache.clear();
    }
  }

  getCacheStats() {
    return {
      size: this.cache.size,
      keys: Array.from(this.cache.keys()),
    };
  }
}
```

### Pagination & Filtering

```javascript
// Client-side filtering and pagination helpers
class BareCMSHelpers {
  static filterEntries(entries, filters = {}) {
    return entries.filter((entry) => {
      // Date range filter
      if (filters.dateFrom || filters.dateTo) {
        const entryDate = new Date(entry.created_at);
        if (filters.dateFrom && entryDate < new Date(filters.dateFrom))
          return false;
        if (filters.dateTo && entryDate > new Date(filters.dateTo))
          return false;
      }

      // Search in title and content
      if (filters.search) {
        const searchTerm = filters.search.toLowerCase();
        const searchText = (entry.title + " " + entry.content).toLowerCase();
        if (!searchText.includes(searchTerm)) return false;
      }

      // Custom field filters
      if (filters.custom) {
        for (const [key, value] of Object.entries(filters.custom)) {
          if (entry[key] !== value) return false;
        }
      }

      return true;
    });
  }

  static sortEntries(entries, sortBy = "created_at", order = "desc") {
    return [...entries].sort((a, b) => {
      let aVal = a[sortBy];
      let bVal = b[sortBy];

      // Handle date sorting
      if (sortBy.includes("_at")) {
        aVal = new Date(aVal);
        bVal = new Date(bVal);
      }

      if (order === "desc") {
        return bVal > aVal ? 1 : -1;
      } else {
        return aVal > bVal ? 1 : -1;
      }
    });
  }

  static paginateEntries(entries, page = 1, perPage = 10) {
    const startIndex = (page - 1) * perPage;
    const endIndex = startIndex + perPage;
    const paginatedEntries = entries.slice(startIndex, endIndex);

    return {
      entries: paginatedEntries,
      pagination: {
        currentPage: page,
        perPage,
        totalEntries: entries.length,
        totalPages: Math.ceil(entries.length / perPage),
        hasNextPage: endIndex < entries.length,
        hasPrevPage: page > 1,
      },
    };
  }
}

// Usage example
async function loadBlogPosts(filters = {}, sort = {}, pagination = {}) {
  const data = await cms.fetchSiteData("my-blog");
  const posts =
    data.collections.find((col) => col.slug === "posts")?.entries || [];

  // Filter posts
  let filteredPosts = BareCMSHelpers.filterEntries(posts, filters);

  // Sort posts
  filteredPosts = BareCMSHelpers.sortEntries(
    filteredPosts,
    sort.by || "created_at",
    sort.order || "desc"
  );

  // Paginate posts
  const result = BareCMSHelpers.paginateEntries(
    filteredPosts,
    pagination.page || 1,
    pagination.perPage || 10
  );

  return result;
}
```

---

## 🎯 Best Practices

### Performance Optimization

1. **Cache API responses** client-side to reduce requests
2. **Use pagination** for large collections
3. **Implement lazy loading** for images and content
4. **Optimize bundle size** by importing only what you need

### Error Handling

1. **Always handle network errors** gracefully
2. **Provide fallback content** when API is unavailable
3. **Show meaningful error messages** to users
4. **Implement retry logic** for failed requests

### Security

1. **Validate data** received from the API
2. **Sanitize HTML content** before rendering
3. **Use HTTPS** in production
4. **Implement proper CORS** headers

### User Experience

1. **Show loading states** during data fetching
2. **Implement skeleton screens** for better perceived performance
3. **Provide search and filtering** for large datasets
4. **Ensure accessibility** with proper HTML semantics

---

## 🚀 Ready to Build?

These examples show how easy it is to integrate BareCMS with any frontend framework. The public data API makes it simple to fetch all your content in a single request.

### Next Steps

- **[Deploy BareCMS](../deployment/self-hosting.md)** to your server
- **[Learn the full API](../api/README.md)** for content management
- **[Explore use cases](../guides/use-cases.md)** for inspiration
- **[Join our community](https://github.com/snowztech/barecms/discussions)** for support

---

_Start building your headless CMS project today! 🎯_
