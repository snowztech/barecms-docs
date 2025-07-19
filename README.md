# BareCMS Documentation

This is the documentation site for [BareCMS](https://github.com/snowztech/barecms), built with [Docsify](https://docsify.js.org/).

## Prerequisites

You need to have Node.js installed on your system.

### Install Docsify CLI

Install docsify globally using npm:

```bash
npm install -g docsify-cli
```

Or using yarn:

```bash
yarn global add docsify-cli
```

## Making Changes to Documentation

### 1. Understanding the Structure

The documentation consists of several markdown files:

- `index.md` - Homepage content
- `getting-started.md` - Getting started guide
- `features.md` - Features overview
- `concepts.md` - Core concepts
- `api.md` - API documentation
- `self-hosting.md` - Self-hosting guide
- `contributing.md` - Contributing guidelines
- `quick-example.md` - Quick example
- `_sidebar.md` - Navigation sidebar configuration
- `_coverpage.md` - Cover page configuration
- `_footer.md` - Footer content

### 2. Edit Documentation Files

To make changes:

1. **Edit existing content**: Open any `.md` file and make your changes using standard Markdown syntax
2. **Add new pages**: Create new `.md` files in the root directory
3. **Update navigation**: Edit `_sidebar.md` to add new pages to the sidebar navigation
4. **Modify cover page**: Edit `_coverpage.md` to change the homepage appearance

### 3. Markdown Syntax

Use standard Markdown syntax for formatting:

- `# Heading 1`
- `## Heading 2`
- `**bold text**`
- `*italic text*`
- `` `inline code` ``
- Links: `[text](url)`
- Code blocks: ` ```language `

## Running Documentation Locally

### Start the Local Server

From the project root directory, run:

```bash
docsify serve .
```

### View Your Changes

1. Open your browser and go to: `http://localhost:3000`
2. The documentation will automatically reload when you save changes to any markdown files
3. No need to restart the server - just save your files and refresh the browser

### Hot Reload

Docsify automatically watches for file changes and refreshes the browser, so you can see your edits in real-time.

## Tips

- **Live Preview**: Keep the local server running while editing to see changes instantly
- **Sidebar Navigation**: Remember to update `_sidebar.md` when adding new pages
- **Images**: Place images in the `assets/` folder and reference them with relative paths
- **Internal Links**: Use relative paths for linking between documentation pages

## Need Help?

- [Docsify Documentation](https://docsify.js.org/)
- [Markdown Guide](https://www.markdownguide.org/)
- [BareCMS Repository](https://github.com/snowztech/barecms)
