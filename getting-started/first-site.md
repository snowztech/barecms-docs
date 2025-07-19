# Build Your First Complete Site

In this tutorial, we'll create a complete portfolio website with multiple collections, demonstrating the full power of BareCMS.

## 🎯 What We'll Build

A professional portfolio site with:

- 📄 **About Page** - Personal information
- 💼 **Projects Collection** - Portfolio projects
- 📝 **Blog Posts** - Technical articles
- 📞 **Contact Info** - Contact details
- 🌐 **Public API** - All content accessible via API

---

## 📋 Project Structure

```
Portfolio Site (my-portfolio)
├── Pages Collection
│   ├── About
│   └── Contact
├── Projects Collection
│   ├── Project 1
│   ├── Project 2
│   └── Project 3
└── Blog Collection
    ├── Article 1
    ├── Article 2
    └── Article 3
```

---

## 🚀 Step 1: Create the Portfolio Site

```bash
curl -X POST http://localhost:8080/api/sites \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe Portfolio",
    "slug": "my-portfolio",
    "description": "Full-stack developer portfolio showcasing projects and articles"
  }'
```

**Response:**

```json
{
  "id": 2,
  "name": "John Doe Portfolio",
  "slug": "my-portfolio",
  "description": "Full-stack developer portfolio showcasing projects and articles",
  "created_at": "2024-01-15T11:00:00Z"
}
```

---

## 📄 Step 2: Create Pages Collection

First, let's create a collection for static pages:

```bash
curl -X POST http://localhost:8080/api/sites/2/collections \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Pages",
    "slug": "pages",
    "description": "Static pages like About and Contact"
  }'
```

### Add About Page

```bash
curl -X POST http://localhost:8080/api/collections/2/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "About Me",
    "content": "Hi, I'\''m John Doe, a passionate full-stack developer with 5 years of experience building web applications. I specialize in React, Node.js, and cloud technologies. When I'\''m not coding, you can find me hiking or contributing to open source projects.",
    "slug": "about"
  }'
```

### Add Contact Page

```bash
curl -X POST http://localhost:8080/api/collections/2/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Contact",
    "content": "Get in touch with me:\n\nEmail: john@example.com\nLinkedIn: linkedin.com/in/johndoe\nGitHub: github.com/johndoe\nLocation: San Francisco, CA",
    "slug": "contact"
  }'
```

---

## 💼 Step 3: Create Projects Collection

```bash
curl -X POST http://localhost:8080/api/sites/2/collections \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Projects",
    "slug": "projects",
    "description": "Portfolio projects and case studies"
  }'
```

### Add Project 1: E-commerce Platform

```bash
curl -X POST http://localhost:8080/api/collections/3/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "E-commerce Platform",
    "content": "Built a full-stack e-commerce platform using React, Node.js, and PostgreSQL. Features include user authentication, payment processing with Stripe, and admin dashboard.\n\nTechnologies: React, Node.js, PostgreSQL, Stripe API, AWS\nGitHub: github.com/johndoe/ecommerce-platform\nLive Demo: https://demo-ecommerce.com",
    "slug": "ecommerce-platform"
  }'
```

### Add Project 2: Task Management App

```bash
curl -X POST http://localhost:8080/api/collections/3/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Task Management App",
    "content": "A collaborative task management application with real-time updates using WebSockets. Users can create teams, assign tasks, and track progress with beautiful analytics.\n\nTechnologies: Vue.js, Express.js, Socket.io, MongoDB\nGitHub: github.com/johndoe/task-manager\nLive Demo: https://taskmanager-demo.com",
    "slug": "task-management-app"
  }'
```

### Add Project 3: Weather Dashboard

```bash
curl -X POST http://localhost:8080/api/collections/3/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Weather Dashboard",
    "content": "A responsive weather dashboard that displays current conditions and forecasts for multiple cities. Features include geolocation, search, and data visualization with charts.\n\nTechnologies: TypeScript, Next.js, Chart.js, OpenWeather API\nGitHub: github.com/johndoe/weather-dashboard\nLive Demo: https://weather-dashboard-demo.com",
    "slug": "weather-dashboard"
  }'
```

---

## 📝 Step 4: Create Blog Collection

```bash
curl -X POST http://localhost:8080/api/sites/2/collections \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Blog",
    "slug": "blog",
    "description": "Technical articles and tutorials"
  }'
```

### Add Blog Post 1

```bash
curl -X POST http://localhost:8080/api/collections/4/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Building Scalable React Applications",
    "content": "In this article, I share my experience building large-scale React applications. We'\''ll cover code organization, state management patterns, and performance optimization techniques that I'\''ve learned working on production apps.\n\nKey topics covered:\n- Component architecture\n- State management with Redux Toolkit\n- Code splitting and lazy loading\n- Performance monitoring\n\nRead more about my approach to building maintainable React codebases.",
    "slug": "building-scalable-react-applications"
  }'
```

### Add Blog Post 2

```bash
curl -X POST http://localhost:8080/api/collections/4/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "API Design Best Practices",
    "content": "Designing good APIs is crucial for successful applications. In this post, I outline the best practices I follow when designing RESTful APIs, including proper HTTP status codes, consistent naming conventions, and effective error handling.\n\nTopics covered:\n- RESTful design principles\n- HTTP status codes\n- Request/response patterns\n- Authentication strategies\n- Documentation with OpenAPI\n\nLearn how to create APIs that developers love to use.",
    "slug": "api-design-best-practices"
  }'
```

### Add Blog Post 3

```bash
curl -X POST http://localhost:8080/api/collections/4/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Docker for Frontend Developers",
    "content": "Docker isn'\''t just for backend developers! In this tutorial, I show how frontend developers can use Docker to create consistent development environments, simplify deployment, and improve team collaboration.\n\nWhat you'\''ll learn:\n- Containerizing React/Vue/Angular apps\n- Multi-stage builds for production\n- Docker Compose for full-stack development\n- Deployment strategies\n\nStop saying '\''it works on my machine'\'' and start using Docker!",
    "slug": "docker-for-frontend-developers"
  }'
```

---

## 🌐 Step 5: Access Your Complete Portfolio

Now let's see all our content with a single API call:

```bash
curl http://localhost:8080/my-portfolio/data
```

**Response:**

```json
{
  "site": {
    "id": 2,
    "name": "John Doe Portfolio",
    "slug": "my-portfolio",
    "description": "Full-stack developer portfolio showcasing projects and articles"
  },
  "collections": [
    {
      "id": 2,
      "name": "Pages",
      "slug": "pages",
      "description": "Static pages like About and Contact",
      "entries": [
        {
          "id": 4,
          "title": "About Me",
          "content": "Hi, I'm John Doe, a passionate full-stack developer...",
          "slug": "about",
          "created_at": "2024-01-15T11:10:00Z"
        },
        {
          "id": 5,
          "title": "Contact",
          "content": "Get in touch with me:\n\nEmail: john@example.com...",
          "slug": "contact",
          "created_at": "2024-01-15T11:12:00Z"
        }
      ]
    },
    {
      "id": 3,
      "name": "Projects",
      "slug": "projects",
      "description": "Portfolio projects and case studies",
      "entries": [
        {
          "id": 6,
          "title": "E-commerce Platform",
          "content": "Built a full-stack e-commerce platform using React...",
          "slug": "ecommerce-platform",
          "created_at": "2024-01-15T11:15:00Z"
        },
        {
          "id": 7,
          "title": "Task Management App",
          "content": "A collaborative task management application...",
          "slug": "task-management-app",
          "created_at": "2024-01-15T11:17:00Z"
        },
        {
          "id": 8,
          "title": "Weather Dashboard",
          "content": "A responsive weather dashboard that displays...",
          "slug": "weather-dashboard",
          "created_at": "2024-01-15T11:19:00Z"
        }
      ]
    },
    {
      "id": 4,
      "name": "Blog",
      "slug": "blog",
      "description": "Technical articles and tutorials",
      "entries": [
        {
          "id": 9,
          "title": "Building Scalable React Applications",
          "content": "In this article, I share my experience building...",
          "slug": "building-scalable-react-applications",
          "created_at": "2024-01-15T11:22:00Z"
        },
        {
          "id": 10,
          "title": "API Design Best Practices",
          "content": "Designing good APIs is crucial for successful...",
          "slug": "api-design-best-practices",
          "created_at": "2024-01-15T11:24:00Z"
        },
        {
          "id": 11,
          "title": "Docker for Frontend Developers",
          "content": "Docker isn't just for backend developers!...",
          "slug": "docker-for-frontend-developers",
          "created_at": "2024-01-15T11:26:00Z"
        }
      ]
    }
  ]
}
```

---

## 💻 Step 6: Build the Frontend

Here's how to create a React portfolio site using this data:

```jsx
// Portfolio.jsx
import React, { useState, useEffect } from "react";
import "./Portfolio.css";

function Portfolio() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("http://localhost:8080/my-portfolio/data")
      .then((res) => res.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch((err) => {
        console.error("Error fetching portfolio data:", err);
        setLoading(false);
      });
  }, []);

  if (loading) return <div className="loading">Loading portfolio...</div>;
  if (!data) return <div className="error">Failed to load portfolio</div>;

  // Helper functions to get specific collections
  const getCollection = (slug) =>
    data.collections.find((collection) => collection.slug === slug);

  const getEntry = (collection, slug) =>
    collection?.entries.find((entry) => entry.slug === slug);

  const pages = getCollection("pages");
  const projects = getCollection("projects");
  const blog = getCollection("blog");

  const aboutPage = getEntry(pages, "about");
  const contactPage = getEntry(pages, "contact");

  return (
    <div className="portfolio">
      {/* Header */}
      <header className="header">
        <h1>{data.site.name}</h1>
        <p>{data.site.description}</p>
        <nav>
          <a href="#about">About</a>
          <a href="#projects">Projects</a>
          <a href="#blog">Blog</a>
          <a href="#contact">Contact</a>
        </nav>
      </header>

      {/* About Section */}
      {aboutPage && (
        <section id="about" className="about">
          <h2>{aboutPage.title}</h2>
          <p>{aboutPage.content}</p>
        </section>
      )}

      {/* Projects Section */}
      {projects && (
        <section id="projects" className="projects">
          <h2>Projects</h2>
          <div className="projects-grid">
            {projects.entries.map((project) => (
              <div key={project.id} className="project-card">
                <h3>{project.title}</h3>
                <p>{project.content}</p>
                <small>
                  Created: {new Date(project.created_at).toLocaleDateString()}
                </small>
              </div>
            ))}
          </div>
        </section>
      )}

      {/* Blog Section */}
      {blog && (
        <section id="blog" className="blog">
          <h2>Latest Articles</h2>
          <div className="blog-posts">
            {blog.entries.map((post) => (
              <article key={post.id} className="blog-post">
                <h3>{post.title}</h3>
                <p>{post.content.substring(0, 200)}...</p>
                <small>
                  Published: {new Date(post.created_at).toLocaleDateString()}
                </small>
                <a href={`/blog/${post.slug}`} className="read-more">
                  Read More
                </a>
              </article>
            ))}
          </div>
        </section>
      )}

      {/* Contact Section */}
      {contactPage && (
        <section id="contact" className="contact">
          <h2>{contactPage.title}</h2>
          <div className="contact-info">
            {contactPage.content.split("\n").map((line, index) => (
              <p key={index}>{line}</p>
            ))}
          </div>
        </section>
      )}

      {/* Footer */}
      <footer className="footer">
        <p>&copy; 2024 {data.site.name}. Built with BareCMS.</p>
      </footer>
    </div>
  );
}

export default Portfolio;
```

---

## 🎨 Step 7: Add Styling

```css
/* Portfolio.css */
.portfolio {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  font-family: "Inter", sans-serif;
  line-height: 1.6;
  color: #333;
}

.header {
  text-align: center;
  padding: 60px 0;
  border-bottom: 1px solid #eee;
}

.header h1 {
  font-size: 3rem;
  margin-bottom: 10px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.header nav {
  margin-top: 30px;
}

.header nav a {
  margin: 0 20px;
  text-decoration: none;
  color: #666;
  font-weight: 500;
  transition: color 0.3s;
}

.header nav a:hover {
  color: #667eea;
}

.about,
.projects,
.blog,
.contact {
  padding: 80px 0;
}

.about h2,
.projects h2,
.blog h2,
.contact h2 {
  font-size: 2.5rem;
  margin-bottom: 30px;
  text-align: center;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 30px;
  margin-top: 40px;
}

.project-card {
  background: #f8f9fa;
  padding: 30px;
  border-radius: 10px;
  border: 1px solid #e9ecef;
  transition:
    transform 0.3s,
    box-shadow 0.3s;
}

.project-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
}

.project-card h3 {
  color: #667eea;
  margin-bottom: 15px;
}

.blog-posts {
  display: grid;
  gap: 30px;
  margin-top: 40px;
}

.blog-post {
  background: #f8f9fa;
  padding: 30px;
  border-radius: 10px;
  border-left: 4px solid #667eea;
}

.blog-post h3 {
  color: #333;
  margin-bottom: 15px;
}

.read-more {
  color: #667eea;
  text-decoration: none;
  font-weight: 500;
  margin-left: 10px;
}

.contact-info {
  text-align: center;
  background: #f8f9fa;
  padding: 40px;
  border-radius: 10px;
  margin-top: 30px;
}

.footer {
  text-align: center;
  padding: 40px 0;
  border-top: 1px solid #eee;
  color: #666;
}

.loading,
.error {
  text-align: center;
  padding: 100px 20px;
  font-size: 1.2rem;
}

@media (max-width: 768px) {
  .header h1 {
    font-size: 2rem;
  }

  .header nav a {
    display: block;
    margin: 10px 0;
  }

  .projects-grid {
    grid-template-columns: 1fr;
  }
}
```

---

## 🔄 Step 8: Advanced Operations

### Update a Project

```bash
curl -X PUT http://localhost:8080/api/entries/6 \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "E-commerce Platform v2.0",
    "content": "Updated e-commerce platform with new features including wishlist, reviews, and advanced search. Built with React, Node.js, and PostgreSQL.\n\nNew Features:\n- User wishlist\n- Product reviews\n- Advanced search and filtering\n- Improved admin dashboard\n\nTechnologies: React, Node.js, PostgreSQL, Stripe API, AWS, Elasticsearch"
  }'
```

### Add More Collections

```bash
# Add Skills collection
curl -X POST http://localhost:8080/api/sites/2/collections \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Skills",
    "slug": "skills",
    "description": "Technical skills and expertise"
  }'

# Add skill entries
curl -X POST http://localhost:8080/api/collections/5/entries \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Frontend Development",
    "content": "Expert level: React, Vue.js, TypeScript, HTML5, CSS3, SASS\nIntermediate: Angular, Svelte, WebGL",
    "slug": "frontend-development"
  }'
```

---

## ✅ What You've Built

Congratulations! You now have a complete portfolio website featuring:

- ✅ **Professional site structure** with meaningful organization
- ✅ **Multiple content types** (pages, projects, blog posts)
- ✅ **Rich content** with detailed descriptions and metadata
- ✅ **Responsive React frontend** that consumes the API
- ✅ **Production-ready architecture** ready for deployment
- ✅ **Extensible structure** for adding more content types

---

## 🚀 Next Steps

Now that you have a complete site:

1. **[Deploy to Production →](../deployment/self-hosting.md)** - Make it live
2. **[Learn Advanced API Features →](../api/README.md)** - Explore all endpoints
3. **[See More Integration Examples →](../integration/frontend-examples.md)** - Different frameworks
4. **[Optimize for Performance →](../guides/performance.md)** - Make it fast
5. **[Add Security Features →](../guides/security.md)** - Keep it secure

---

## 💡 Pro Tips

- **Plan your content structure** before creating collections
- **Use descriptive slugs** for SEO-friendly URLs
- **Keep content atomic** - one concept per entry
- **Leverage the public API** for fast, cacheable content delivery
- **Think mobile-first** when designing your frontend
- **Use semantic HTML** for better accessibility

---

_Ready to deploy your portfolio to the world? Let's make it live! 🌍_
