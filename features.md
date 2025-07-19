# Features

BareCMS is designed with minimalism in mind while providing all the essential features you need for content management.

## Core Features

### 🎯 Minimalist Design

Clean, intuitive interface focused on content management. No bloat, no unnecessary complexity - just the tools you need to manage your content effectively.

### ⚡ Fast & Lightweight

Built with performance in mind using Go and React. BareCMS delivers fast response times and minimal resource usage, making it perfect for any deployment environment.

### 🔧 Headless Architecture

Use any frontend framework or static site generator. BareCMS provides your content through a simple API, giving you complete freedom in how you present it.

### 🐳 Docker Ready

Easy deployment with Docker and Docker Compose. Get up and running in minutes with our pre-configured containers and comprehensive deployment guides.

### 🔐 Secure Authentication

JWT-based authentication system ensures your content management is secure while maintaining simplicity and performance.

### 🗃️ Flexible Content Management

Support for sites, collections, and entries provides a hierarchical structure that adapts to any content strategy - from simple blogs to complex multi-site architectures.

### 🚀 Production Ready

Built with scalability and reliability in mind. BareCMS is designed to handle real-world workloads with confidence.

## Content Structure

BareCMS organizes content in a simple, intuitive hierarchy:

- **Sites**: Top-level containers for your projects
- **Collections**: Groups of related content (e.g., blog posts, products, pages)
- **Entries**: Individual pieces of content within collections

## Public API Access

All your content is accessible through a single, simple endpoint:

- **`GET /api/:siteSlug/data`** - Retrieve all site content publicly (no authentication required)

This makes BareCMS perfect for:

- Static site generators
- React/Vue/Svelte applications
- Mobile apps
- Third-party integrations

## Deployment Options

- **Docker Compose**: All-in-one deployment with PostgreSQL
- **Docker Only**: Use with external database
- **Local Development**: Full development environment with hot reload

## What BareCMS is NOT

In keeping with our minimalist philosophy, BareCMS intentionally excludes:

- Complex user roles and permissions
- Built-in theme systems
- Plugin architectures
- Admin dashboard customization
- Complex workflow management

This keeps BareCMS simple, fast, and reliable while giving you the freedom to build exactly what you need.
