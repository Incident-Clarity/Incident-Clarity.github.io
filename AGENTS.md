# Agent Guidelines for Incident-Clarity.github.io

This document outlines the conventions and commands for agentic coding in this Hugo-based blog repository.

## 1. Build/Lint/Test Commands

- **Build Site:** `hugo --gc --minify`
  - This command compiles the Hugo site, garbage collects unused resources, and minifies output.
- **Single Test:** For content, testing involves manual review for accuracy and formatting. For theme-related code (CSS/JS), specific linting/testing commands would be defined within the theme if applicable (none found currently).

## 2. Code Style Guidelines

- **General:** Adhere strictly to existing project conventions and patterns.
- **Markdown Content:**
    - Use consistent heading levels (e.g., `#` for main title, `##` for sections).
    - Employ clear, concise, and grammatically correct language.
    - Use proper Markdown syntax for lists, links, images, and code blocks.
- **Hugo Templates (HTML):**
    - Ensure well-formed and semantic HTML.
    - Maintain consistent indentation (2 spaces).
    - Use Hugo's built-in functions and partials idiomatically.
- **File Naming:** Use `kebab-case` for all content and template filenames (e.g., `my-new-article.md`).
- **Error Handling:** For any custom scripts or theme modifications, implement robust error handling.
- **Imports/Types/Formatting:** Follow the established patterns within the `blowfish` theme's CSS/JS files if making modifications.

## 3. Cursor/Copilot Rules

No specific Cursor or Copilot rules were found in this repository. Agents should follow the general guidelines above.
