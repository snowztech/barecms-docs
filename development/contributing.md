# Contributing to BareCMS

We welcome and appreciate contributions to BareCMS! Whether it's a bug fix, a new feature, documentation improvements, or simply reporting an issue, your help makes BareCMS better for everyone.

Please take a moment to review this guide before making your first contribution.

## 📋 Table of Contents

- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [How to Contribute](#how-to-contribute)
- [Development Workflow](#development-workflow)
- [Code Guidelines](#code-guidelines)
- [Submitting Changes](#submitting-changes)
- [Community Guidelines](#community-guidelines)

---

## Getting Started

### Ways to Contribute

- 🐛 **Report bugs** - Help us find and fix issues
- ✨ **Suggest features** - Share ideas for improvements
- 📝 **Improve documentation** - Make guides clearer
- 💻 **Write code** - Implement features and fixes
- 🧪 **Write tests** - Improve code coverage
- 🎨 **Improve UI/UX** - Enhance user experience

### Before You Start

- Check existing [issues](https://github.com/snowztech/barecms/issues) and [pull requests](https://github.com/snowztech/barecms/pulls)
- Read our [Code of Conduct](https://github.com/snowztech/barecms/blob/main/CODE_OF_CONDUCT.md)
- Join our [discussions](https://github.com/snowztech/barecms/discussions) for questions

---

## Development Setup

### Prerequisites

- **Go 1.21+** - [Install Go](https://golang.org/doc/install)
- **Node.js 18+** - [Install Node.js](https://nodejs.org/)
- **Docker & Docker Compose** - [Install Docker](https://docs.docker.com/get-docker/)
- **PostgreSQL** - For database (or use Docker)
- **Git** - Version control

### Local Setup

```bash
# 1. Fork and clone the repository
git clone https://github.com/YOUR_USERNAME/barecms.git
cd barecms

# 2. Set up upstream remote
git remote add upstream https://github.com/snowztech/barecms.git

# 3. Install backend dependencies
go mod tidy

# 4. Install frontend dependencies
cd ui
npm install
cd ..

# 5. Set up environment
cp .env.example .env
# Edit .env with your settings

# 6. Start development environment
make dev
```

### Project Structure

```
barecms/
├── cmd/                 # Application entry points
├── internal/            # Private application code
│   ├── handlers/        # HTTP handlers
│   ├── middlewares/     # HTTP middlewares
│   ├── models/          # Data models
│   ├── services/        # Business logic
│   └── storage/         # Database layer
├── ui/                  # Frontend React app
│   ├── src/
│   └── public/
├── configs/             # Configuration files
├── docs/                # Documentation
└── Makefile            # Development commands
```

---

## How to Contribute

### 1. Fork the Repository

- Go to the [BareCMS GitHub repository](https://github.com/snowztech/barecms)
- Click the "Fork" button in the top right corner
- This creates a copy under your GitHub account

### 2. Clone Your Fork

```bash
git clone https://github.com/YOUR_GITHUB_USERNAME/barecms.git
cd barecms
```

### 3. Set Up the Upstream Remote

```bash
git remote add upstream https://github.com/snowztech/barecms.git
git remote -v
# You should see 'origin' pointing to your fork and 'upstream' pointing to the main repo
```

### 4. Create a New Branch

Choose a descriptive name for your branch:

```bash
git checkout main
git pull upstream main  # Ensure your local main is up-to-date
git checkout -b feature/your-feature-name
```

**Branch Naming Conventions:**

- `feature/add-user-roles` - New features
- `fix/login-redirect-bug` - Bug fixes
- `docs/update-api-guide` - Documentation
- `refactor/cleanup-handlers` - Code refactoring
- `test/add-auth-tests` - Adding tests

---

## Development Workflow

### Backend Development

```bash
# Run backend only
go run cmd/main.go

# Run with hot reload
make dev-backend

# Run tests
make test

# Check linting
make lint
```

### Frontend Development

```bash
# Navigate to UI directory
cd ui

# Start development server
npm run dev

# Build for production
npm run build

# Run tests
npm test
```

### Full Stack Development

```bash
# Start everything with Docker
make up

# Start development mode (with hot reload)
make dev

# View logs
make logs

# Clean up
make clean
```

---

## Code Guidelines

### Go Code Style

- Follow standard Go conventions
- Use `gofmt` to format code
- Write meaningful variable and function names
- Add comments for exported functions
- Handle errors properly

```go
// Good
func GetUserByID(id int) (*User, error) {
    if id <= 0 {
        return nil, fmt.Errorf("invalid user ID: %d", id)
    }

    user, err := db.FindUser(id)
    if err != nil {
        return nil, fmt.Errorf("failed to find user: %w", err)
    }

    return user, nil
}
```

### React/TypeScript Style

- Use TypeScript for type safety
- Follow React hooks patterns
- Use meaningful component names
- Extract reusable logic into custom hooks
- Write JSX with proper accessibility

```tsx
// Good
interface UserProfileProps {
  user: User;
  onUpdate: (user: User) => void;
}

export function UserProfile({ user, onUpdate }: UserProfileProps) {
  const [isEditing, setIsEditing] = useState(false);

  const handleSave = useCallback(
    (updatedUser: User) => {
      onUpdate(updatedUser);
      setIsEditing(false);
    },
    [onUpdate]
  );

  return (
    <div className="user-profile">
      <h2>{user.name}</h2>
      {/* Component content */}
    </div>
  );
}
```

### Database Guidelines

- Use proper migrations for schema changes
- Write efficient queries with proper indexes
- Handle database errors gracefully
- Use transactions for multi-step operations

---

## Submitting Changes

### 1. Keep Your Branch Updated

Before committing, rebase with the latest upstream changes:

```bash
git fetch upstream
git rebase upstream/main
```

**Resolve Conflicts:** If there are conflicts, Git will pause for resolution:

```bash
# Fix conflicts in your editor
git add .
git rebase --continue
```

### 2. Commit Your Changes

Write clear, descriptive commit messages:

```bash
git add .
git commit -m "feat: add user role management API"
```

**Commit Message Format:**

- `feat:` - New features
- `fix:` - Bug fixes
- `docs:` - Documentation changes
- `style:` - Code formatting (no functional changes)
- `refactor:` - Code refactoring
- `test:` - Adding or fixing tests
- `chore:` - Maintenance tasks

### 3. Push to Your Fork

```bash
git push origin feature/your-feature-name
```

**If you've rebased after pushing:**

```bash
git push --force-with-lease origin feature/your-feature-name
```

### 4. Open a Pull Request

1. Go to the original BareCMS repository
2. GitHub will usually prompt you to open a PR
3. Provide a clear title and description
4. Reference any related issues (e.g., `Closes #123`)
5. Be prepared for feedback and discussions

---

## Community Guidelines

### Pull Request Guidelines

- **Keep PRs focused** - One feature or fix per PR
- **Write good descriptions** - Explain what and why
- **Add tests** - For new features and bug fixes
- **Update documentation** - If your changes affect usage
- **Be responsive** - Address feedback promptly

### Review Process

1. **Automated checks** - CI/CD must pass
2. **Code review** - Maintainers will review your code
3. **Testing** - Verify functionality works as expected
4. **Documentation** - Ensure docs are updated if needed
5. **Merge** - Approved PRs will be merged

### Getting Help

- **GitHub Issues** - For bug reports and feature requests
- **GitHub Discussions** - For questions and general discussion
- **Documentation** - Check existing guides first
- **Community** - Be respectful and helpful

---

## Development Commands

```bash
# Development
make dev          # Start development environment
make dev-backend  # Start backend only
make dev-frontend # Start frontend only

# Testing
make test         # Run all tests
make test-backend # Run Go tests
make test-frontend # Run React tests

# Code Quality
make lint         # Run linters
make format       # Format code

# Database
make migrate      # Run database migrations
make seed         # Seed database with sample data

# Docker
make up           # Start with Docker
make down         # Stop Docker containers
make clean        # Clean up everything

# Building
make build        # Build for production
make ui           # Build frontend only

# Utilities
make logs         # View application logs
make help         # Show all available commands
```

---

## Need Help?

### Resources

- **[API Documentation](../api/README.md)** - Complete API reference
- **[Architecture Guide](architecture.md)** - Understanding the codebase
- **[Local Setup Guide](local-setup.md)** - Detailed setup instructions
- **[Testing Guide](testing.md)** - Writing and running tests

### Community

- **GitHub Issues**: [Report bugs or request features](https://github.com/snowztech/barecms/issues)
- **GitHub Discussions**: [Ask questions and share ideas](https://github.com/snowztech/barecms/discussions)
- **Pull Requests**: [Review existing contributions](https://github.com/snowztech/barecms/pulls)

---

**Thank you for contributing to BareCMS!** 🎉

Every contribution, no matter how small, helps make BareCMS better for everyone. We appreciate your time and effort in improving this project.

---

_Happy coding! 🚀_
