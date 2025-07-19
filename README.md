<div align="center">

<img src="https://github.com/snowztech/barecms/blob/main/assets/logo.svg" alt="BareCMS Logo" width="120" height="120">

<h1>BareCMS</h1>

<h3><em>A lightweight, open-source headless CMS designed with bare minimalism in mind</em></h3>

<img src="https://img.shields.io/github/contributors/snowztech/barecms?style=plastic" alt="Contributors">
<img src="https://img.shields.io/github/forks/snowztech/barecms" alt="Forks">
<img src="https://img.shields.io/github/stars/snowztech/barecms" alt="Stars">
<img src="https://img.shields.io/github/issues/snowztech/barecms" alt="Issues">
<img src="https://img.shields.io/github/repo-size/snowztech/barecms" alt="Repository Size">
<a href="LICENSE">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="MIT License">
</a>

<a href="https://github.com/sponsors/lucasnevespereira">
  <img src="https://img.shields.io/badge/Sponsor-GitHub-333333?style=flat&logo=github&logoColor=white" alt="Sponsor">
</a>

</div>

---

## 🎬 Demo

_See BareCMS in action - create sites, manage collections, and publish content with ease._

Try it yourself by following the [Quick Start Guide](getting-started.md)

---

## ✨ Features

- **🎯 Minimalist Design**: Clean, intuitive interface focused on content management
- **⚡ Fast & Lightweight**: Built with performance in mind using Go and React
- **🔧 Headless Architecture**: Use any frontend framework or static site generator
- **🐳 Docker Ready**: Easy deployment with Docker and Docker Compose
- **🔐 Secure Authentication**: JWT-based authentication system
- **🗃️ Flexible Content Management**: Support for sites, collections, and entries
- **🚀 Production Ready**: Built with scalability and reliability in mind

---

## 🌐 Public Data Access

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

---

## 🗺️ Roadmap

### 🔄 Current Focus

- [ ] Enhanced documentation
- [ ] Improve auth flow
- [ ] Content import/export

_Keep it simple. [Suggest features](https://github.com/snowztech/barecms/issues) that align with our minimal philosophy._

---

## 💬 Community & Support

- **🐛 Found a bug?** [Report it here](https://github.com/snowztech/barecms/issues)
- **💡 Have an idea?** [Start a discussion](https://github.com/snowztech/barecms/discussions)
- **❤️ Love BareCMS?** [Sponsor the project](https://github.com/sponsors/lucasnevespereira)

---

<div align="center">

**Built with ❤️ by [SnowzTech](https://github.com/snowztech)**

_Simple, lightweight, and built for developers._

</div>
