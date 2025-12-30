# CLAUDE.MD

This file provides context for Claude Code when working on this repository.

## Project Overview

This is a personal website for Shen Ting Ang, built using Hugo static site generator with the PaperMod theme. The website features blog posts about data science, bridge, and technology.

- **Live URL**: https://shenting.org/
- **Author**: Shen Ting Ang
- **Hugo Theme**: PaperMod
- **Repository Type**: GitHub Pages site

## Repository Structure

```
.
├── config.toml           # Hugo configuration file
├── content/              # Content directory
│   ├── _index.md        # Homepage content
│   ├── about.md         # About page
│   └── post/            # Blog posts directory
├── layouts/              # Custom layout overrides
├── static/               # Static assets (images, CSS, JS)
├── themes/               # Hugo themes
│   └── PaperMod/        # PaperMod theme (git submodule)
├── .github/              # GitHub workflows
└── CNAME                 # Custom domain configuration
```

## Key Technologies

- **Hugo**: Static site generator
- **PaperMod Theme**: Minimalist Hugo theme focused on readability
- **GitHub Pages**: Hosting platform
- **Markdown**: Content format

## Configuration Highlights

### Site Settings (config.toml)
- Base URL: https://shenting.org/
- Title: "Shen Ting Ang"
- Author: "Shen Ting"
- Theme: PaperMod
- Features enabled:
  - Reading time display
  - Code copy buttons
  - Math rendering (LaTeX support)
  - RSS and JSON feeds

### Navigation Menu
1. About (/about/)
2. Posts (/post/)
3. LinkedIn (external link)

## Content Guidelines

### Blog Posts
- Location: `content/post/`
- Format: Markdown (.md files)
- Front matter should include:
  - `title`: Post title
  - `date`: Publication date
  - `draft`: Set to false for published posts
  - Other metadata as needed (tags, categories, description)

### Existing Post Topics
The site covers various topics including:
- Data science and analytics
- Bridge (card game) rating systems (Elo, Glicko, TrueSkill)
- Technology and programming (Ethereum nodes, Anaconda, RPA)
- Data visualization
- Career and mentorship
- AI adoption challenges

## Development Workflow

### Local Development
```bash
# Start Hugo development server
hugo server -D

# Build static site
hugo

# The built site will be in the public/ directory
```

### Theme Management
PaperMod theme is included as a git submodule. To update:
```bash
git submodule update --remote themes/PaperMod
```

### Creating New Posts
```bash
# Create a new post
hugo new post/post-name.md

# Edit the file at content/post/post-name.md
# Set draft: false when ready to publish
```

## Deployment

This site appears to be deployed via GitHub Pages. Changes pushed to the main branch (master) should trigger automatic deployment.

## Special Features

### Math Rendering
- Enabled via `math = true` in config.toml
- Use LaTeX syntax in markdown for mathematical expressions
- Goldmark renderer has `unsafe = true` to allow raw HTML

### Code Blocks
- Code copy buttons are enabled
- Syntax highlighting available for code blocks

## Important Notes

- The repository uses a git worktree setup
- Main repository: D:\dev\angshenting.github.io
- Current worktree: stoic-kapitsa
- Main branch: master
- Custom domain configured via CNAME file

## Common Tasks

### Adding a New Blog Post
1. Create new markdown file in `content/post/`
2. Add front matter with title, date, and other metadata
3. Write content in markdown
4. Set `draft: false` when ready to publish
5. Commit and push to trigger deployment

### Modifying Site Configuration
- Edit `config.toml` for site-wide settings
- Changes to theme parameters, menu items, or site metadata should be made here

### Customizing Layouts
- Override theme templates by placing files in `layouts/` directory
- Maintain same directory structure as theme

### Adding Static Assets
- Place images, CSS, or JavaScript files in `static/` directory
- Reference in content using `/path/to/file.ext`

## Maintenance

- Keep Hugo updated for security and features
- Periodically update PaperMod theme via git submodule
- Monitor GitHub Actions for deployment status
- Ensure markdown content is properly formatted

## Support Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [PaperMod Theme Documentation](https://github.com/adityatelange/hugo-PaperMod)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
