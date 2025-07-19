# Development

This guide covers contributing to BareCMS development.

## Prerequisites

For local development you'll need:

- [Go 1.21+](https://golang.org/)
- [Node.js 18+](https://nodejs.org/)
- [Docker & Docker Compose](https://docs.docker.com/get-docker/)
- [Git](https://git-scm.com/)

## Development Setup

### 1. Fork and Clone

```bash
git clone https://github.com/YOUR_USERNAME/barecms.git
cd barecms
```

### 2. Set Up Upstream Remote

```bash
git remote add upstream https://github.com/snowztech/barecms.git
git remote -v
```

### 3. Development Environment

```bash
# Copy environment file
cp .env.example .env

# Start development environment
make up

# View logs
make logs
```

## Project Structure

```
barecms/
├── cmd/                 # Application entry point
├── internal/            # Private application code
│   ├── handlers/        # HTTP handlers
│   ├── middlewares/     # HTTP middlewares
│   ├── models/          # Data models
│   ├── services/        # Business logic
│   └── storage/         # Database layer
├── ui/                  # Frontend React app
├── configs/             # Configuration
├── docs/                # Documentation
└── Makefile            # Development commands
```

## Available Commands

```bash
make up       # Start development environment
make ui       # Build UI (frontend)
make clean    # Stop and cleanup containers
make logs     # Show container logs
make help     # Show help message
```

## Contributing Workflow

### 1. Create Feature Branch

```bash
git checkout main
git pull upstream main
git checkout -b feature/your-feature-name
```

### 2. Make Changes

- Follow existing code style
- Update documentation if needed
- Test your changes

### 3. Commit Changes

```bash
git add --all
git commit -m "feat: Add description of your feature"
```

**Commit Message Guidelines:**

- `feat:` - New features
- `fix:` - Bug fixes
- `docs:` - Documentation changes
- `refactor:` - Code refactoring

### 4. Keep Branch Updated

```bash
git fetch upstream
git rebase upstream/main
```

### 5. Push and Create PR

```bash
git push origin feature/your-feature-name
```

Then open a Pull Request on GitHub.

## Pull Request Guidelines

- **Keep PRs focused** - One feature or fix per PR
- **Write clear descriptions** - Explain what and why
- **Test your changes** - Ensure everything works
- **Update documentation** - If your changes affect usage
- **Be responsive** - Address feedback promptly

## Getting Help

- **GitHub Issues**: [Report bugs or request features](https://github.com/snowztech/barecms/issues)
- **GitHub Discussions**: [Ask questions](https://github.com/snowztech/barecms/discussions)

---

Thank you for contributing to BareCMS!
