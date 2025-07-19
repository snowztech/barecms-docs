# Quick Example

Here's a simple example showing how to fetch and display content from BareCMS using JavaScript. This demonstrates the core headless functionality.

## Public API Usage

Once you have BareCMS running and have created a site with some content, you can access all site data publicly without authentication.

### Basic Fetch Example

```javascript
// Fetch all data for a site
async function fetchSiteData(siteSlug) {
  try {
    const response = await fetch(`http://localhost:8080/${siteSlug}/data`);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Error fetching site data:", error);
  }
}

// Usage
fetchSiteData("my-blog").then((data) => {
  console.log(data);
});
```

### Display Blog Posts

```javascript
async function displayBlogPosts(siteSlug) {
  const data = await fetchSiteData(siteSlug);

  if (data && data.collections) {
    // Find the "posts" collection
    const postsCollection = data.collections.find(
      (collection) => collection.slug === "posts"
    );

    if (postsCollection && postsCollection.entries) {
      const postsContainer = document.getElementById("posts");

      postsCollection.entries.forEach((post) => {
        const postElement = document.createElement("article");
        postElement.innerHTML = `
          <h2>${post.title}</h2>
          <p><small>Published: ${new Date(post.created_at).toLocaleDateString()}</small></p>
          <div>${post.content}</div>
          <hr>
        `;
        postsContainer.appendChild(postElement);
      });
    }
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
      async function fetchSiteData(siteSlug) {
        try {
          const response = await fetch(
            `http://localhost:8080/${siteSlug}/data`
          );
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
        document.getElementById("site-title").textContent = data.site.name;
        document.getElementById("site-description").textContent =
          data.site.description;

        // Find and display posts
        const postsCollection = data.collections.find(
          (c) => c.slug === "posts"
        );
        const postsContainer = document.getElementById("posts");

        if (postsCollection && postsCollection.entries.length > 0) {
          postsContainer.innerHTML = "";

          postsCollection.entries.forEach((post) => {
            const article = document.createElement("article");
            article.innerHTML = `
                        <h2>${post.title}</h2>
                        <p><small>Published: ${new Date(post.created_at).toLocaleDateString()}</small></p>
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

When you call `GET /my-blog/data`, you'll receive a response like this:

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
          "content": "This is my first blog post...",
          "slug": "welcome-to-barecms",
          "created_at": "2024-01-15T10:30:00Z"
        }
      ]
    }
  ]
}
```

## Framework Integration

This same approach works with any frontend framework:

- **React**: Use `useEffect` with fetch or a library like SWR
- **Vue**: Use `mounted()` lifecycle or Composition API
- **Svelte**: Use `onMount()`
- **Static Site Generators**: Fetch during build time

The beauty of BareCMS is its simplicity - one endpoint gives you everything you need!
