# BareCMS Documentation

<div align="center">

![BareCMS Logo](https://raw.githubusercontent.com/snowztech/barecms/main/assets/logo.svg ":size=120")

**Complete developer documentation for BareCMS**

_A lightweight, open-source headless CMS designed with bare minimalism in mind_

[![GitHub](https://img.shields.io/badge/GitHub-BareCMS-333333?style=flat&logo=github&logoColor=white)](https://github.com/snowztech/barecms)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://github.com/snowztech/barecms/blob/main/LICENSE)

[🚀 **Get Started**](getting-started/) • [🔌 **API Reference**](api/) • [🐳 **Deploy**](deployment/self-hosting.md)

</div>

---

## 🎯 What is BareCMS?

BareCMS is a **headless CMS** built with bare minimalism in mind. Perfect for developers who want:

- **🎯 API-first approach** - Clean REST API for content management
- **⚡ Fast & lightweight** - Built with Go and React
- **🔧 Framework agnostic** - Use with any frontend
- **🐳 Docker ready** - Deploy anywhere in minutes
- **🔐 Secure by default** - JWT authentication

---

## 🚀 Quick Start

### 1. Install BareCMS

```bash
git clone https://github.com/snowztech/barecms.git
cd barecms
cp .env.example .env
# Edit JWT_SECRET in .env
make up
```

### 2. Create Content

```bash
# Register user
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@example.com","password":"password"}'

# Create site
curl -X POST http://localhost:8080/api/sites \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"name":"My Blog","slug":"my-blog"}'
```

### 3. Access Data Publicly

```bash
# Get all site data (no auth needed!)
curl http://localhost:8080/my-blog/data
```

🎉 **That's it!** You now have a working headless CMS.

---

## 📚 Documentation

### 🏁 Getting Started

- [Installation](getting-started/installation.md) - Get running in 5 minutes
- [Quick Start](getting-started/quick-start.md) - Complete tutorial

### 🔌 API Reference

- [API Overview](api/README.md) - All endpoints and examples
- [Public Data API](api/public-data.md) - Key headless endpoint

### 🐳 Deployment

- [Self-Hosting](deployment/self-hosting.md) - Deploy with Docker

### 🛠️ Development

- [Contributing](development/contributing.md) - How to contribute

---

## 🆘 Need Help?

- **🐛 Found a Bug?** [Report on GitHub Issues](https://github.com/snowztech/barecms/issues)
- **💬 Have Questions?** [Ask in Discussions](https://github.com/snowztech/barecms/discussions)

---

**Ready to build something amazing?** [**🚀 Get started now →**](getting-started/)

<div style="text-align: center; margin: 2rem 0; padding: 1rem; background: #f8f9fa; border-radius: 8px;">
<strong>BareCMS</strong> • Built with ❤️ by <a href="https://github.com/snowztech" target="_blank">SnowzTech</a> • Keep it simple
</div>
