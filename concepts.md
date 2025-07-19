# Concepts

Understanding the core concepts behind BareCMS will help you make the most of its minimalist design and headless architecture.

## Headless CMS Philosophy

BareCMS follows the headless CMS approach, which separates content management from content presentation. This means:

- **Backend**: Manages content creation, storage, and API access
- **Frontend**: Completely separate - use any technology you prefer

This separation provides:

- **Flexibility**: Use React, Vue, Svelte, static site generators, or even mobile apps
- **Performance**: Optimize your frontend without backend constraints
- **Developer Freedom**: Choose the best tools for your specific needs

## Content Hierarchy

BareCMS uses a simple three-tier structure:

```
Site
├── Collection (e.g., "Blog Posts")
│   ├── Entry (e.g., "Welcome Post")
│   ├── Entry (e.g., "About Us")
│   └── Entry (e.g., "Contact")
└── Collection (e.g., "Products")
    ├── Entry (e.g., "Product A")
    └── Entry (e.g., "Product B")
```

### Sites

- Top-level containers for your projects
- Each site has a unique slug for public access
- Can contain multiple collections
- Perfect for managing multiple websites or projects

### Collections

- Groups of related content within a site
- Think "blog posts", "products", "pages", etc.
- Each collection has its own slug
- Provides logical organization for your content

### Entries

- Individual pieces of content within collections
- Contains title, content, slug, and timestamps
- The actual content that gets displayed on your frontend

## API-First Design

BareCMS is built around a simple API principle:

### For Content Management (Authenticated)

```
POST /api/auth/login           # Authentication
GET  /api/sites               # Manage sites
GET  /api/collections         # Manage collections
GET  /api/entries             # Manage entries
```

### For Content Access (Public)

```
GET  /api/status                  # Check server status
GET  /api/:siteSlug/data          # Get all site content
```

The beauty is in the simplicity - one public endpoint gives you everything.

## Workflow

The typical BareCMS workflow follows this pattern:

1. **Setup**: Deploy BareCMS using Docker
2. **Create**: Use the admin interface to create sites, collections, and entries
3. **Develop**: Build your frontend using the public API
4. **Deploy**: Your frontend can be hosted anywhere, completely independent of BareCMS

## Authentication Model

BareCMS uses JWT (JSON Web Tokens) for authentication:

- **Stateless**: No server-side sessions to manage
- **Secure**: Industry-standard token-based authentication
- **Simple**: Easy to implement and understand
- **Scalable**: Perfect for distributed deployments

## Data Access Patterns

### Public Access

```javascript
// No authentication needed
fetch("/my-blog/data")
  .then((response) => response.json())
  .then((data) => {
    // Use data.site, data.collections, data.entries
  });
```

### Authenticated Management

```javascript
// Requires JWT token
fetch("/api/sites", {
  headers: {
    Authorization: `Bearer ${token}`,
  },
});
```

## Deployment Architecture

BareCMS is designed for flexible deployment:

### Single Instance

- BareCMS + PostgreSQL on one server
- Perfect for small to medium projects
- Easy Docker Compose setup

### Distributed

- BareCMS instances behind load balancer
- Shared PostgreSQL database
- Scales horizontally as needed

### Hybrid

- BareCMS for content management
- CDN for static asset delivery
- Separate frontend hosting (Netlify, Vercel, etc.)

## Design Principles

### Minimalism

Every feature in BareCMS serves a clear purpose. We intentionally avoid:

- Complex permission systems
- Theme builders
- Plugin architectures
- Workflow management

### Performance

- Go backend for fast API responses
- React frontend for responsive admin interface
- Minimal database queries
- Efficient JSON responses

### Developer Experience

- Simple API design
- Clear documentation
- Docker-first deployment
- No vendor lock-in

### Self-Hosting Friendly

- Minimal dependencies
- Clear deployment instructions
- Reasonable resource requirements
- Open source and transparent

These concepts work together to create a CMS that's powerful enough for real projects while remaining simple enough to understand and maintain.
