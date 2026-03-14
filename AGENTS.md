# AGENTS.md

Repository-specific instructions for agentic coding agents working on this codebase.

## Project Overview

This is a personal portfolio/bio website built with Vue 3, Vite, and Tailwind CSS v4. The project follows a minimalist design philosophy with clean, semantic markup and modern CSS.

## Build/Lint/Test Commands

### Development

```bash
npm run dev          # Start development server on http://localhost:5173
npm run build        # Production build to dist/
npm run preview      # Preview production build locally
```

### Linting & Formatting

```bash
npm run lint         # Run ESLint on .vue, .js, .jsx, .cjs, .mjs files (auto-fix enabled)
npm run format       # Run Prettier on src/ directory
```

### Testing

This project currently has no automated tests configured. If tests are added in the future:

- Look for test scripts in package.json
- Check for vitest or jest configuration files

## Code Style Guidelines

### File Naming & Organization

- Vue components: PascalCase (e.g., `App.vue`, `MyComponent.vue`)
- JS/TS files: camelCase (e.g., `main.js`)
- CSS files: kebab-case (e.g., `base.css`, `main.css`)

### Imports

```javascript
// 1. Node.js built-in imports (use node: prefix)
import { fileURLToPath, URL } from 'node:url'

// 2. Third-party packages
import { createApp } from 'vue'
import { defineConfig } from 'vite'

// 3. Local imports (use @ alias for src/)
import App from './App.vue'
import MyComponent from '@/components/MyComponent.vue'
```

### Prettier Configuration

The project uses Prettier with these settings:

- **semi**: false (no semicolons)
- **singleQuote**: true (use single quotes)
- **tabWidth**: 2 (2-space indentation)
- **printWidth**: 100
- **trailingComma**: 'none' (no trailing commas)

### ESLint Rules

- Extends `plugin:vue/vue3-essential` and `eslint:recommended`
- Vue 3 essential rules are enforced
- Uses `@rushstack/eslint-patch` for modern module resolution

### Vue Component Style

```vue
<script setup lang="ts">
// Use <script setup> with TypeScript
// Keep script section minimal for static components
</script>

<template>
  <!-- Use semantic HTML and Tailwind utility classes -->
  <div class="min-h-screen bg-white text-gray-900">
    <!-- Content -->
  </div>
</template>

<style scoped>
/* Use scoped styles for component-specific CSS */
/* Prefer Tailwind utilities; use scoped CSS for complex styles */
</style>
```

### TypeScript Usage

- The project uses `<script setup lang="ts">` in Vue components
- `jsconfig.json` configures path aliases (`@/*` maps to `./src/*`)
- If TypeScript files are added, consider migrating to `tsconfig.json`

### CSS & Styling

- **Primary approach**: Tailwind CSS utility classes in templates
- **Tailwind v4**: Imports via `@import "tailwindcss"` in main.css
- **Scoped CSS**: Use for component-specific styles not achievable with Tailwind
- **CSS custom properties**: Define in base.css for theming (see `:root` variables)

### Tailwind CSS Conventions

```html
<!-- Order: layout → spacing → typography → colors → effects -->
<div class="min-h-screen bg-white text-gray-900 font-sans">
  <!-- Use responsive prefixes (sm:, md:, lg:) -->
  <div class="px-6 py-20 md:py-32">
    <!-- Use semantic classes where possible -->
    <h1 class="text-4xl font-bold tracking-tight">Title</h1>
  </div>
</div>
```

### Error Handling

- In `<script setup>`, use try/catch for async operations
- Display user-friendly error messages in the UI
- Log errors to console for debugging during development

### Semantic HTML

- Use appropriate heading levels (h1, h2, h3)
- Use semantic elements: `<header>`, `<main>`, `<footer>`, `<nav>`, `<section>`
- Ensure accessibility with ARIA attributes where needed

### Git Commit Guidelines

This project does not have formal commit conventions defined. Use conventional commit format:

- `feat:` - New features
- `fix:` - Bug fixes
- `style:` - CSS/styling changes
- `refactor:` - Code refactoring
- `docs:` - Documentation updates

## Development Workflow

1. **Start development**: `npm run dev`
2. **Make changes**: Follow code style guidelines above
3. **Lint code**: `npm run lint` (auto-fixes common issues)
4. **Format code**: `npm run format`
5. **Build for production**: `npm run build`

## Key Files

| File                  | Purpose                                         |
| --------------------- | ----------------------------------------------- |
| `vite.config.js`      | Vite configuration with Vue plugin and Tailwind |
| `src/main.js`         | Application entry point                         |
| `src/App.vue`         | Main application component                      |
| `src/assets/main.css` | Tailwind CSS imports                            |
| `src/assets/base.css` | Base styles and CSS custom properties           |
| `jsconfig.json`       | Path alias configuration                        |

## Notes

- The project is a static personal bio page with minimal JavaScript logic
- EmailJS is configured for potential contact form functionality
- Deployment is configured for GitHub Pages (`npm run deploy`)
