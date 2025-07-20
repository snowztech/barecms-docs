# Quick Example

Here's a simple example showing how to fetch and display content from BareCMS using JavaScript. This demonstrates the core headless functionality.

## Public API Usage

Once you have BareCMS running and have created a site with some content, you can access all site data publicly without authentication.

### Basic Fetch Example

```javascript
const barecmsHost = "http://localhost:8080";

// Fetch all data for a site
async function fetchSiteData(siteSlug) {
  try {
    const response = await fetch(`${barecmsHost}/api/${siteSlug}/data`);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Error fetching site data:", error);
  }
}

// Usage
fetchSiteData("my-blog").then((data) => {
  console.log(data.data.articles); // Access your articles
  console.log(data.data.products); // Access your products
});
```

### Display Blog Posts

```javascript
async function displayBlogPosts(siteSlug) {
  const data = await fetchSiteData(siteSlug);

  if (data && data.data && data.data.articles) {
    const postsContainer = document.getElementById("posts");

    data.data.articles.forEach((post) => {
      const postElement = document.createElement("article");
      postElement.innerHTML = `
        <h2>${post.title}</h2>
        <p><small>Published: ${post.published || "No date"}</small></p>
        <div>${post.content}</div>
        <hr>
      `;
      postsContainer.appendChild(postElement);
    });
  }
}
```

### Complete HTML Example

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Blog - Powered by BareCMS</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
      }
      article {
        margin-bottom: 30px;
      }
      h1 {
        color: #333;
      }
      h2 {
        color: #666;
      }
      hr {
        border: 1px solid #eee;
      }
    </style>
  </head>
  <body>
    <header>
      <h1 id="site-title">Loading...</h1>
      <p id="site-description"></p>
    </header>

    <main id="posts">
      <p>Loading posts...</p>
    </main>

    <script>
      const barecmsHost = "http://localhost:8080";

      async function fetchSiteData(siteSlug) {
        try {
          const response = await fetch(`${barecmsHost}/api/${siteSlug}/data`);
          return await response.json();
        } catch (error) {
          console.error("Error fetching site data:", error);
          return null;
        }
      }

      async function loadSite() {
        const data = await fetchSiteData("my-blog"); // Replace with your site slug

        if (!data) {
          document.getElementById("posts").innerHTML =
            "<p>Error loading content.</p>";
          return;
        }

        // Update site info
        document.getElementById("site-title").textContent = data.name;
        document.getElementById("site-description").textContent =
          data.description || "";

        // Find and display posts
        const postsContainer = document.getElementById("posts");

        if (data.data && data.data.articles && data.data.articles.length > 0) {
          postsContainer.innerHTML = "";

          data.data.articles.forEach((post) => {
            const article = document.createElement("article");
            article.innerHTML = `
              <h2>${post.title}</h2>
              <p><small>Published: ${post.published || "No date"}</small></p>
              <div>${post.content}</div>
              <hr>
            `;
            postsContainer.appendChild(article);
          });
        } else {
          postsContainer.innerHTML = "<p>No posts found.</p>";
        }
      }

      // Load the site when page loads
      loadSite();
    </script>
  </body>
</html>
```

## Expected Response Format

When you call `GET /api/my-blog/data`, you'll receive a response like this:

```json
{
  "id": "44394f36-daa3-451c-970f-59238c46ce36",
  "name": "myblog",
  "slug": "myblog",
  "data": {
    "articles": [
      {
        "content": "this is my article post content",
        "draft": "false",
        "published": "2025-07-21",
        "title": "my sample article"
      }
    ],
    "products": [
      {
        "name": "Sample Product",
        "price": "29.99",
        "description": "A great product for everyone"
      }
    ]
  }
}
```

## Key Features of the New Response Structure

- **Simple Access**: Collections are directly accessible as keys in the `data` object
- **Direct Field Values**: Entry fields are directly accessible without nested objects
- **Easy Iteration**: Each collection contains an array of entries ready to use
- **Clean Structure**: No complex nesting - just `data.collectionName[index].fieldName`

## Framework Integration

This same approach works with any frontend framework:

- **React**: Use `useEffect` with fetch or a library like SWR
- **Vue**: Use `mounted()` lifecycle or Composition API
- **Svelte**: Use `onMount()`
- **Static Site Generators**: Fetch during build time

### React Example

```javascript
import { useEffect, useState } from "react";

function BlogPosts({ siteSlug }) {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(`http://localhost:8080/api/${siteSlug}/data`)
      .then((res) => res.json())
      .then((data) => {
        setPosts(data.data.articles || []);
        setLoading(false);
      });
  }, [siteSlug]);

  if (loading) return <div>Loading...</div>;

  return (
    <div>
      {posts.map((post, index) => (
        <article key={index}>
          <h2>{post.title}</h2>
          <p>{post.published}</p>
          <div dangerouslySetInnerHTML={{ __html: post.content }} />
        </article>
      ))}
    </div>
  );
}
```

The beauty of BareCMS is its simplicity - one endpoint gives you everything you need!
