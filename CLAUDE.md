# CLAUDE.md

## Project Overview

Trove is a browser extension that saves and manages URLs.

Trove is a browser extension built with WXT (Web Extension Tools) and React. WXT is a modern framework for building cross-browser extensions with first-class TypeScript support, hot module reloading, and automated build processes.

## Package Manager

This project uses **Bun** as its package manager and runtime. Bun is managed via mise.

- Always use `bun install` to install dependencies
- Always use `bun run <script>` to run package.json scripts
- Bun version 1.3.1 is pinned via mise.toml

## Development Commands

### Running the Extension

```bash
# Chrome/Edge development (default)
bun run dev
# or
mise run dev

# Firefox development
bun run dev:firefox
# or
mise run dev:firefox
```

### Building

```bash
# Build for Chrome/Edge
bun run build
# or
mise run build

# Build for Firefox
bun run build:firefox
# or
mise run build:firefox
```

### Type Checking

```bash
# Type check without emitting files
bun run compile
# or
mise run compile
```

### Creating Distribution Packages

```bash
# Create zip for Chrome/Edge
bun run zip
# or
mise run zip

# Create zip for Firefox
bun run zip:firefox
# or
mise run zip:firefox
```

## Project Structure

### Entrypoints

WXT uses an entrypoints-based architecture where each file in `entrypoints/` becomes a distinct part of the extension:

- `entrypoints/background.ts` - Background service worker that runs persistently
- `entrypoints/content.ts` - Content script that runs on matching web pages (currently configured for `*://*.google.com/*`)
- `entrypoints/popup/` - Browser action popup UI (React-based)
  - `main.tsx` - React app entry point
  - `App.tsx` - Main popup component
  - `index.html` - Popup HTML template

### Key Directories

- `.wxt/` - Generated WXT configuration and type definitions (do not edit manually)
- `.output/` - Build output directory (gitignored)
- `public/` - Static assets copied directly to output (icons, etc.)
- `assets/` - Assets imported in code (bundled with Vite)

## Architecture Notes

### WXT Framework

- WXT abstracts away manifest.json complexity with convention-based configuration
- Uses Vite for bundling and development server
- Provides special `defineBackground`, `defineContentScript` functions for type-safe extension code
- Automatically handles cross-browser compatibility (Chrome/Firefox)
- The `wxt prepare` postinstall script generates TypeScript types and configuration

### React Integration

- Uses `@wxt-dev/module-react` for React support
- React 19 with TypeScript
- JSX transform configured via tsconfig.json (`"jsx": "react-jsx"`)
- Standard React patterns apply for popup UI development

### TypeScript Configuration

- Base config extends `.wxt/tsconfig.json` (auto-generated)
- `allowImportingTsExtensions` enabled for `.tsx` imports without extensions
- Path alias `@/` maps to root directory for imports

## Browser Extension Specifics

- The `browser` global is available in all extension contexts (background, content scripts, popup)
- Content scripts must declare `matches` patterns to specify where they run
- Background scripts run in a service worker context (no DOM access)
- Popup code runs in a normal browser context with access to extension APIs

## Development Workflow

1. Run `bun install` (or let mise handle it via postinstall hook)
2. Start dev server with `bun run dev` or `mise run dev`
3. Load the unpacked extension from `.output/chrome-mv3/` (or firefox-mv3)
4. Make changes - HMR will reload the extension automatically
5. Type check with `bun run compile` before committing
