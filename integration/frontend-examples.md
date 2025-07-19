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

      <div className="posts">
        {posts.map((post) => (
          <article key={post.id}>
            <h2>{post.title}</h2>
            <p>{post.content}</p>
            <small>
              Published: {new Date(post.created_at).toLocaleDateString()}
            </small>
          </article>
        ))}
      </div>
    </div>
  );
}

export default BlogPosts;
```

### Custom Hook for BareCMS

```jsx
// hooks/useBareCMS.js
import { useState, useEffect } from "react";

export function useBareCMS(siteSlug) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(
          `https://your-barecms.com/${siteSlug}/data`
        );
        if (!response.ok) {
          throw new Error("Failed to fetch data");
        }
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [siteSlug]);

  // Helper functions
  const getCollection = (slug) => {
    return data?.collections.find((col) => col.slug === slug);
  };

  const getEntry = (collectionSlug, entrySlug) => {
    const collection = getCollection(collectionSlug);
    return collection?.entries.find((entry) => entry.slug === entrySlug);
  };

  return {
    data,
    loading,
    error,
    site: data?.site,
    collections: data?.collections || [],
    getCollection,
    getEntry,
  };
}

// Usage in component
function Blog() {
  const { site, getCollection, loading, error } = useBareCMS("my-blog");

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  const posts = getCollection("posts")?.entries || [];

  return (
    <div>
      <h1>{site.name}</h1>
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

## 🖖 Vue.js Example

### Vue 3 Composition API

```vue
<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="error">Error: {{ error }}</div>
    <div v-else>
      <h1>{{ site.name }}</h1>
      <p>{{ site.description }}</p>

      <article v-for="post in posts" :key="post.id" class="post">
        <h2>{{ post.title }}</h2>
        <p>{{ post.content }}</p>
        <small>{{ formatDate(post.created_at) }}</small>
      </article>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";

const data = ref(null);
const loading = ref(true);
const error = ref(null);

const site = computed(() => data.value?.site);
const posts = computed(() => {
  const postsCollection = data.value?.collections.find(
    (col) => col.slug === "posts"
  );
  return postsCollection?.entries || [];
});

onMounted(async () => {
  try {
    const response = await fetch("https://your-barecms.com/my-blog/data");
    if (!response.ok) throw new Error("Failed to fetch");
    data.value = await response.json();
  } catch (err) {
    error.value = err.message;
  } finally {
    loading.value = false;
  }
});

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleDateString();
};
</script>
```

### Vue Composable

```js
// composables/useBareCMS.js
import { ref, onMounted } from "vue";

export function useBareCMS(siteSlug) {
  const data = ref(null);
  const loading = ref(true);
  const error = ref(null);

  const fetchData = async () => {
    try {
      const response = await fetch(`https://your-barecms.com/${siteSlug}/data`);
      if (!response.ok) throw new Error("Failed to fetch data");
      data.value = await response.json();
    } catch (err) {
      error.value = err.message;
    } finally {
      loading.value = false;
    }
  };

  const getCollection = (slug) => {
    return data.value?.collections.find((col) => col.slug === slug);
  };

  const getEntry = (collectionSlug, entrySlug) => {
    const collection = getCollection(collectionSlug);
    return collection?.entries.find((entry) => entry.slug === entrySlug);
  };

  onMounted(fetchData);

  return {
    data,
    loading,
    error,
    site: computed(() => data.value?.site),
    collections: computed(() => data.value?.collections || []),
    getCollection,
    getEntry,
    refetch: fetchData,
  };
}
```

---

## 🔺 Next.js Example

### Static Generation (SSG)

```jsx
// pages/blog/index.js
import { GetStaticProps } from "next";

export default function BlogIndex({ site, posts }) {
  return (
    <div>
      <h1>{site.name}</h1>
      <p>{site.description}</p>

      <div className="posts">
        {posts.map((post) => (
          <article key={post.id}>
            <h2>
              <Link href={`/blog/${post.slug}`}>{post.title}</Link>
            </h2>
            <p>{post.content.substring(0, 150)}...</p>
            <small>{new Date(post.created_at).toLocaleDateString()}</small>
          </article>
        ))}
      </div>
    </div>
  );
}

export const getStaticProps = async () => {
  const response = await fetch("https://your-barecms.com/my-blog/data");
  const data = await response.json();

  const postsCollection = data.collections.find((col) => col.slug === "posts");
  const posts = postsCollection?.entries || [];

  return {
    props: {
      site: data.site,
      posts,
    },
    revalidate: 60, // Revalidate every minute
  };
};
```

### Individual Post Pages

```jsx
// pages/blog/[slug].js
import { GetStaticPaths, GetStaticProps } from "next";

export default function BlogPost({ site, post }) {
  if (!post) return <div>Post not found</div>;

  return (
    <article>
      <h1>{post.title}</h1>
      <div dangerouslySetInnerHTML={{ __html: post.content }} />
      <small>Published: {new Date(post.created_at).toLocaleDateString()}</small>
    </article>
  );
}

export const getStaticPaths = async () => {
  const response = await fetch("https://your-barecms.com/my-blog/data");
  const data = await response.json();

  const postsCollection = data.collections.find((col) => col.slug === "posts");
  const posts = postsCollection?.entries || [];

  const paths = posts.map((post) => ({
    params: { slug: post.slug },
  }));

  return {
    paths,
    fallback: "blocking",
  };
};

export const getStaticProps = async ({ params }) => {
  const response = await fetch("https://your-barecms.com/my-blog/data");
  const data = await response.json();

  const postsCollection = data.collections.find((col) => col.slug === "posts");
  const post = postsCollection?.entries.find(
    (entry) => entry.slug === params.slug
  );

  return {
    props: {
      site: data.site,
      post: post || null,
    },
    revalidate: 60,
  };
};
```

---

## 🌊 Svelte Example

```svelte
<!-- BlogPosts.svelte -->
<script>
  import { onMount } from 'svelte';

  let data = null;
  let loading = true;
  let error = null;

  $: site = data?.site;
  $: posts = data?.collections.find(col => col.slug === 'posts')?.entries || [];

  onMount(async () => {
    try {
      const response = await fetch('https://your-barecms.com/my-blog/data');
      if (!response.ok) throw new Error('Failed to fetch');
      data = await response.json();
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  });

  function formatDate(dateString) {
    return new Date(dateString).toLocaleDateString();
  }
</script>

{#if loading}
  <p>Loading...</p>
{:else if error}
  <p>Error: {error}</p>
{:else}
  <h1>{site.name}</h1>
  <p>{site.description}</p>

  <div class="posts">
    {#each posts as post (post.id)}
      <article class="post">
        <h2>{post.title}</h2>
        <p>{post.content}</p>
        <small>Published: {formatDate(post.created_at)}</small>
      </article>
    {/each}
  </div>
{/if}

<style>
  .posts {
    display: grid;
    gap: 2rem;
  }

  .post {
    padding: 1rem;
    border: 1px solid #ddd;
    border-radius: 8px;
  }
</style>
```

---

## 🔧 Vanilla JavaScript Example

```html
<!doctype html>
<html>
  <head>
    <title>My Blog</title>
  </head>
  <body>
    <div id="app">
      <div id="loading">Loading...</div>
    </div>

    <script>
      class BareCMSClient {
        constructor(baseUrl) {
          this.baseUrl = baseUrl;
        }

        async getSiteData(siteSlug) {
          const response = await fetch(`${this.baseUrl}/${siteSlug}/data`);
          if (!response.ok) {
            throw new Error("Failed to fetch data");
          }
          return response.json();
        }

        getCollection(data, slug) {
          return data.collections.find((col) => col.slug === slug);
        }

        getEntry(collection, slug) {
          return collection?.entries.find((entry) => entry.slug === slug);
        }
      }

      // Initialize
      const cms = new BareCMSClient("https://your-barecms.com");
      const app = document.getElementById("app");

      async function loadBlog() {
        try {
          const data = await cms.getSiteData("my-blog");
          const postsCollection = cms.getCollection(data, "posts");
          const posts = postsCollection?.entries || [];

          app.innerHTML = `
          <h1>${data.site.name}</h1>
          <p>${data.site.description}</p>
          <div class="posts">
            ${posts
              .map(
                (post) => `
              <article>
                <h2>${post.title}</h2>
                <p>${post.content}</p>
                <small>Published: ${new Date(post.created_at).toLocaleDateString()}</small>
              </article>
            `
              )
              .join("")}
          </div>
        `;
        } catch (error) {
          app.innerHTML = `<p>Error: ${error.message}</p>`;
        }
      }

      loadBlog();
    </script>
  </body>
</html>
```

---

## 🚀 Static Site Generators

### Gatsby

```jsx
// gatsby-node.js
exports.createPages = async ({ actions }) => {
  const { createPage } = actions;

  // Fetch data from BareCMS
  const response = await fetch("https://your-barecms.com/my-blog/data");
  const data = await response.json();

  const postsCollection = data.collections.find((col) => col.slug === "posts");
  const posts = postsCollection?.entries || [];

  // Create pages for each post
  posts.forEach((post) => {
    createPage({
      path: `/blog/${post.slug}`,
      component: require.resolve("./src/templates/blog-post.js"),
      context: {
        post,
        site: data.site,
      },
    });
  });
};
```

### Nuxt.js

```js
// nuxt.config.js
export default {
  async generate() {
    const response = await fetch("https://your-barecms.com/my-blog/data");
    const data = await response.json();

    const postsCollection = data.collections.find(
      (col) => col.slug === "posts"
    );
    const posts = postsCollection?.entries || [];

    const routes = posts.map((post) => `/blog/${post.slug}`);

    return {
      routes,
    };
  },
};
```

---

## 🎯 Best Practices

### 1. Error Handling

```javascript
async function fetchSiteData(siteSlug) {
  try {
    const response = await fetch(`/api/${siteSlug}/data`);

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    return await response.json();
  } catch (error) {
    console.error("Failed to fetch site data:", error);
    // Show user-friendly error message
    return null;
  }
}
```

### 2. Loading States

```jsx
function LoadingState() {
  return (
    <div className="animate-pulse">
      <div className="h-8 bg-gray-200 rounded mb-4"></div>
      <div className="h-4 bg-gray-200 rounded mb-2"></div>
      <div className="h-4 bg-gray-200 rounded mb-2"></div>
    </div>
  );
}
```

### 3. Caching

```javascript
// Cache data for 5 minutes
const cache = new Map();
const CACHE_DURATION = 5 * 60 * 1000; // 5 minutes

async function getCachedSiteData(siteSlug) {
  const cacheKey = siteSlug;
  const cached = cache.get(cacheKey);

  if (cached && Date.now() - cached.timestamp < CACHE_DURATION) {
    return cached.data;
  }

  const data = await fetchSiteData(siteSlug);
  cache.set(cacheKey, { data, timestamp: Date.now() });

  return data;
}
```

### 4. TypeScript Types

```typescript
interface BareCMSEntry {
  id: number;
  title: string;
  content: string;
  slug: string;
  created_at: string;
  updated_at: string;
}

interface BareCMSCollection {
  id: number;
  name: string;
  slug: string;
  description: string;
  entries: BareCMSEntry[];
}

interface BareCMSSite {
  id: number;
  name: string;
  slug: string;
  description: string;
}

interface BareCMSData {
  site: BareCMSSite;
  collections: BareCMSCollection[];
}
```

---

## 🔗 Next Steps

- **[Deploy Your Frontend →](../deployment/frontend-deployment.md)**
- **[Performance Optimization →](../guides/performance.md)**
- **[SEO Best Practices →](../guides/seo.md)**
- **[Advanced Patterns →](advanced-patterns.md)**

---

_Build amazing frontends with BareCMS! 🚀_
