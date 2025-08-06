# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is Stanley Lin's personal blog and professional portfolio built with Hugo static site generator using the LoveIt theme. Stanley is a Senior Android Engineer with 9+ years of software development experience, currently working at ViewSonic. The blog showcases technical expertise, career journey, and personal insights.

## Architecture

### Core Structure
- **Hugo Static Site**: Built with Hugo using the LoveIt theme in `themes/LoveIt/`
- **Content Organization**: All content in `content/` directory organized by type (posts, about, categories, tags)
- **Configuration**: Main site configuration in `hugo.toml` (Chinese comments, English content)
- **Custom Styling**: Custom CSS in `assets/css/_custom.scss` integrated via theme configuration
- **Static Assets**: Images and favicon files in `assets/images/` and `static/`
- **Post Templates**: Structured templates for different content types in `post-templates/`

### Theme Integration & Customization
- Uses LoveIt Hugo theme with extensive customization
- Custom CSS integration: `assets/css/_custom.scss` configured in hugo.toml
- Theme library CSS configuration: `myCss = "css/_custom.scss"` in params.page.library.css
- Git repository integration: `gitRepo = "https://github.com/stanleyoho/stanleyoho.github.io"`
- Auto theme mode with light/dark support

### Content Strategy
Based on the About page, this is a professional portfolio site featuring:
- **Professional Experience**: 9+ years software development, 6+ years Android development, 3+ years embedded systems
- **Career Journey**: Detailed work history from embedded systems (Nexcom) to current role (ViewSonic)
- **Technical Expertise**: Android, Kotlin, Java, C/C++, Flutter, embedded systems, MDM solutions
- **Leadership**: Team lead experience managing 3-member development team

## Development Commands

### Hugo Development
```bash
# Serve locally for development
hugo server --buildDrafts --disableFastRender

# Build for production
hugo --gc --minify

# Build with drafts for testing
hugo --buildDrafts --gc
```

### Theme Development (if needed)
```bash
# Navigate to theme directory
cd themes/LoveIt

# Install dependencies
npm install

# Compile JavaScript
npm run compile

# Development server for theme
npm run hugo-server
```

## Site Configuration

### Key Configuration Updates in `hugo.toml`
- **Repository Integration**: `gitRepo = "https://github.com/stanleyoho/stanleyoho.github.io"`
- **Custom CSS**: `custom_css = ["assets/css/custom.css"]` and library integration
- **Theme Mode**: `defaultTheme = "auto"` (supports light/dark mode switching)
- **Professional Focus**: Site description as "personal life blogger" but content is career-focused

### Content Features
- Professional portfolio layout with detailed work experience
- Custom styling for career timeline and experience sections
- Markdown with extended syntax (FontAwesome icons, ruby annotations)
- Code syntax highlighting with copy functionality
- Mathematical formulas via KaTeX
- Light gallery for images
- Social sharing and professional networking integration

## Development Workflow

1. **Adding New Posts**: Create markdown files in `content/posts/` using appropriate templates from `post-templates/`
2. **Updating Portfolio**: Edit `content/about/index.md` for professional experience updates
3. **Custom Styling**: Modify `assets/css/_custom.scss` for theme customizations
4. **Local Development**: Use `hugo server --buildDrafts` to preview changes
5. **Deployment**: Site deployed to GitHub Pages at https://stanleyoho.github.io/

## Content Guidelines

Based on Stanley's professional background:
- **Technical Focus**: Android development, embedded systems, MDM solutions, cross-platform development
- **Career Content**: Leadership experiences, team management insights, technical challenges
- **Professional Development**: Learning journeys, technology adoption, industry trends
- **Mixed Languages**: Configuration in Chinese, content primarily in English
- **Target Audience**: Software engineers, Android developers, tech professionals

## Professional Profile Summary

Stanley Lin is a Senior Android Engineer with extensive experience in:
- **Mobile Development**: 6+ years Android application development
- **Embedded Systems**: 3+ years Windows embedded systems development
- **Leadership**: Team lead managing development teams and establishing coding standards
- **Industries**: Display technology (ViewSonic, Optoma), gaming/streaming, fintech
- **Technical Stack**: Android, Kotlin, Java, C/C++, Flutter, Azure DevOps, MDM systems