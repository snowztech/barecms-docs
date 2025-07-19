# Development Guide

Welcome to the BareCMS development guide! This document covers everything you need to know about contributing to BareCMS.

## 📋 Table of Contents

- [🔧 Prerequisites](#-prerequisites)
- [🚀 Development Setup](#-development-setup)
- [📁 Project Structure](#-project-structure)
- [💡 Available Commands](#-available-commands)
- [🤝 Contributing Workflow](#-contributing-workflow)
- [📝 Pull Request Guidelines](#-pull-request-guidelines)
- [🧪 Testing](#-testing)
- [📖 Documentation](#-documentation)
- [🆘 Getting Help](#-getting-help)

---

## 🔧 Prerequisites

For local development you'll need:

- **[Go 1.21+](https://golang.org/)** - Backend development
- **[Node.js 18+](https://nodejs.org/)** - Frontend development
- **[Docker & Docker Compose](https://docs.docker.com/get-docker/)** - Containerization
- **[Git](https://git-scm.com/)** - Version control
- **[Make](https://www.gnu.org/software/make/)** - Build automation (optional but recommended)

### Quick Prerequisites Check

```bash
# Check installed versions
go version    # Should be 1.21+
node --version    # Should be 18+
docker --version
docker compose version
git --version
make --version
```

---

## 🚀 Development Setup

### 1. Fork and Clone

```bash
# Fork the repository on GitHub first, then:
git clone https://github.com/YOUR_USERNAME/barecms.git
cd barecms
```

### 2. Set Up Upstream Remote

```bash
git remote add upstream https://github.com/snowztech/barecms.git
git remote -v
```

### 3. Environment Configuration

```bash
# Copy environment file
cp .env.example .env

# Generate a secure JWT secret
openssl rand -base64 32

# Edit .env and add your JWT secret
nano .env
```

**Essential environment variables for development:**

```env
# Security
JWT_SECRET=your-generated-secret-here

# Application
PORT=8080
DEBUG=true
LOG_LEVEL=debug

# Database (handled by docker-compose)
POSTGRES_USER=barecms_user
POSTGRES_PASSWORD=dev_password
POSTGRES_DB=barecms_dev
DATABASE_URL=postgresql://barecms_user:dev_password@localhost:5432/barecms_dev
```

### 4. Start Development Environment

```bash
# Start all services (database + application)
make up

# View logs
make logs

# Verify everything is running
curl http://localhost:8080/health
```

Your development environment is now ready! 🎉

- **Application**: http://localhost:8080
- **API**: http://localhost:8080/api
- **Database**: localhost:5432

---

## 📁 Project Structure

Understanding the codebase organization:

```
barecms/
├── cmd/                 # Application entry point
│   └── main.go         # Main application file
├── internal/            # Private application code
│   ├── handlers/        # HTTP request handlers
│   ├── middlewares/     # HTTP middlewares
│   ├── models/          # Data models and structures
│   ├── services/        # Business logic layer
│   └── storage/         # Database access layer
├── ui/                  # Frontend React application
│   ├── src/            # React source code
│   ├── public/         # Static assets
│   └── package.json    # Frontend dependencies
├── configs/             # Configuration files
├── docs/                # Documentation
├── scripts/             # Development scripts
├── .github/             # GitHub workflows
├── docker-compose.yml   # Development environment
├── Dockerfile          # Production image
├── Makefile            # Development commands
├── go.mod              # Go dependencies
└── README.md           # Project overview
```

### Key Directories Explained

- **`cmd/`**: Application entry points and main functions
- **`internal/`**: Private application code that can't be imported by other projects
- **`internal/handlers/`**: HTTP request handlers (API endpoints)
- **`internal/models/`**: Data structures and database models
- **`internal/services/`**: Business logic and core functionality
- **`ui/`**: React frontend application

---

## 💡 Available Commands

### Make Commands (Recommended)

```bash
# Development
make up          # Start development environment
make down        # Stop all services
make restart     # Restart all services
make logs        # View application logs
make logs-db     # View database logs

# Building
make ui          # Build frontend
make build       # Build backend
make build-all   # Build both frontend and backend

# Maintenance
make clean       # Stop and cleanup containers
make reset       # Reset development environment
make backup      # Backup development database
make help        # Show all available commands
```

### Direct Commands

```bash
# Backend development
go run cmd/main.go                # Run backend directly
go build -o barecms cmd/main.go   # Build binary
go test ./...                     # Run tests

# Frontend development
cd ui
npm install       # Install dependencies
npm start         # Start development server
npm run build     # Build for production
npm test          # Run frontend tests

# Database operations
docker compose exec postgres psql -U barecms_user -d barecms_dev
```

---

## 🤝 Contributing Workflow

### 1. Create Feature Branch

```bash
# Ensure you're on main and up to date
git checkout main
git pull upstream main

# Create and switch to feature branch
git checkout -b feature/your-feature-name
```

**Branch Naming Conventions:**

- `feature/feature-name` - New features
- `fix/bug-description` - Bug fixes
- `docs/update-description` - Documentation updates
- `refactor/component-name` - Code refactoring

### 2. Make Your Changes

- **Follow existing code style** and patterns
- **Write tests** for new functionality
- **Update documentation** if needed
- **Test your changes** thoroughly

### 3. Code Quality Checks

```bash
# Backend checks
go fmt ./...                # Format code
go vet ./...               # Static analysis
go test ./...              # Run tests

# Frontend checks
cd ui
npm run lint               # ESLint
npm run type-check         # TypeScript check
npm test                   # Jest tests
```

### 4. Commit Changes

```bash
git add --all
git commit -m "feat: Add description of your feature"
```

**Commit Message Guidelines:**

- `feat:` - New features
- `fix:` - Bug fixes
- `docs:` - Documentation changes
- `style:` - Code style changes (formatting, etc.)
- `refactor:` - Code refactoring
- `test:` - Adding or updating tests
- `chore:` - Maintenance tasks

**Examples:**

- `feat: Add user authentication system`
- `fix: Resolve database connection timeout`
- `docs: Update API documentation for collections`
- `refactor: Simplify entry validation logic`

### 5. Keep Branch Updated

```bash
# Fetch latest changes
git fetch upstream

# Rebase your branch
git rebase upstream/main
```

### 6. Push and Create PR

```bash
git push origin feature/your-feature-name
```

Then open a Pull Request on GitHub with:

- **Clear title** describing the change
- **Detailed description** explaining what and why
- **Screenshots** if UI changes are involved
- **Testing instructions** for reviewers

---

## 📝 Pull Request Guidelines

### Before Submitting

- [ ] **Tested locally** - All changes work as expected
- [ ] **Tests pass** - All existing and new tests pass
- [ ] **Code formatted** - Go and JavaScript code properly formatted
- [ ] **Documentation updated** - If your changes affect usage
- [ ] **Branch up to date** - Rebased with latest main

### PR Best Practices

- **Keep PRs focused** - One feature or fix per PR
- **Write clear descriptions** - Explain what and why
- **Include examples** - Show how to use new features
- **Be responsive** - Address feedback promptly
- **Update documentation** - Keep docs in sync with code

### PR Template

```markdown
## Description

Brief description of what this PR does.

## Type of Change

- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Code refactoring

## Testing

- [ ] Tested locally
- [ ] All tests pass
- [ ] Added new tests if applicable

## Screenshots (if applicable)

Include screenshots for UI changes.
```

---

## 🧪 Testing

### Backend Testing

```bash
# Run all tests
go test ./...

# Run tests with coverage
go test -cover ./...

# Run specific package tests
go test ./internal/handlers

# Run tests in verbose mode
go test -v ./...
```

### Frontend Testing

```bash
cd ui

# Run all tests
npm test

# Run tests in watch mode
npm test --watch

# Run tests with coverage
npm test --coverage
```

### Integration Testing

```bash
# Start test environment
make test-env

# Run integration tests
make test-integration

# Cleanup test environment
make test-clean
```

---

## 📖 Documentation

### Types of Documentation

1. **Code Comments** - Inline documentation for complex logic
2. **API Documentation** - OpenAPI/Swagger specs
3. **User Documentation** - Guides and tutorials
4. **Developer Documentation** - Architecture and contributing guides

### Documentation Guidelines

- **Keep it current** - Update docs with code changes
- **Be clear and concise** - Easy to understand
- **Include examples** - Show real usage
- **Use proper formatting** - Markdown with proper headers

### Building Documentation

```bash
# Generate API documentation
make docs-api

# Build user documentation site
make docs-build

# Serve documentation locally
make docs-serve
```

---

## 🆘 Getting Help

### Community Resources

- **[GitHub Issues](https://github.com/snowztech/barecms/issues)** - Report bugs or request features
- **[GitHub Discussions](https://github.com/snowztech/barecms/discussions)** - Ask questions and share ideas
- **[Discord Server](https://discord.gg/barecms)** - Real-time community chat

### Development Questions

When asking for help, please include:

1. **What you're trying to do** - Your goal
2. **What you've tried** - Steps taken
3. **What happened** - Actual behavior
4. **What you expected** - Expected behavior
5. **Environment details** - OS, versions, etc.

### Code Review Process

1. **Automated checks** run first (tests, linting, etc.)
2. **Maintainer review** for code quality and design
3. **Community feedback** from other contributors
4. **Final approval** and merge by maintainers

---

## 🎯 Development Tips

### Performance Guidelines

- **Database queries** - Use indexes, avoid N+1 queries
- **API responses** - Include only necessary data
- **Frontend** - Optimize bundle size, use lazy loading
- **Caching** - Implement appropriate caching strategies

### Security Considerations

- **Input validation** - Validate all user inputs
- **Authentication** - Secure JWT implementation
- **Authorization** - Proper access controls
- **SQL injection** - Use parameterized queries
- **XSS protection** - Sanitize outputs

### Code Style

- **Go**: Follow standard Go conventions and `gofmt`
- **JavaScript/TypeScript**: Use Prettier and ESLint
- **CSS**: Follow BEM methodology
- **Git**: Use conventional commits

---

**Thank you for contributing to BareCMS! 🙏**

Your contributions help make BareCMS better for everyone. Whether it's code, documentation, bug reports, or feature suggestions, every contribution is valuable and appreciated.
