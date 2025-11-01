# Trove UI

Shared React component library for Trove apps, built with shadcn/ui components.

## Installation

This package is already configured in the workspace. Apps can import components using the `ui/` path alias.

## Usage

Import components from the package using the `ui/` prefix:

```tsx
import { Button } from 'ui/components/button'
import { cn } from 'ui/lib/utils'

function MyComponent() {
  return (
    <Button variant="default" size="lg">
      Click me
    </Button>
  )
}
```

## Available Components

- **Button** - A flexible button component with multiple variants (default, destructive, outline, secondary, ghost, link) and sizes (default, sm, lg, icon)

## Adding New shadcn Components

To add new shadcn components:

1. Navigate to the `packages/trove-ui` directory
2. Use the shadcn CLI to add components:
   ```bash
   bunx shadcn@latest add button
   ```
3. Components will be added to `src/components/`
4. Export them in `src/index.ts` for use in apps

## Development

Type check the package:

```bash
bun run type-check
```

## Configuration

- **TypeScript**: Configured with strict mode and React 19 JSX
- **Path Aliases**: `@/` maps to `src/` within the package
- **Exports**: Components are exported via wildcard pattern `ui/*` → `src/*`

## Dependencies

- **React**: ^19.1.1
- **class-variance-authority**: For component variants
- **clsx** & **tailwind-merge**: For className utilities
- **lucide-react**: Icon library (used by shadcn components)
