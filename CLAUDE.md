# CLAUDE.md — Gnymble Astro

## Project Overview

Gnymble is a compliant SMS marketing platform for premium establishments (cigar lounges, whiskey bars, private clubs, wine bars). This repository is its marketing/landing site built with **Astro 5**, **React 19**, and **Tailwind CSS 4**.

## Tech Stack

- **Framework**: Astro 5 (static site generation, file-based routing)
- **UI Library**: React 19 (available via `@astrojs/react` integration, used for interactive islands)
- **Styling**: Tailwind CSS 4 (via `@tailwindcss/vite` plugin)
- **Icons**: Lucide React
- **Class Utilities**: `clsx` + `tailwind-merge` via `cn()` helper in `src/lib/utils.ts`
- **Component Variants**: `class-variance-authority` (CVA)
- **Backend**: Supabase client configured in `src/lib/supabase.ts` (not yet actively used)
- **Language**: TypeScript (strict mode via Astro's tsconfig)

## Directory Structure

```
src/
├── assets/          # Images and brand logos (SVG, PNG)
├── components/      # Reusable Astro components (Navigation.astro, Footer.astro)
├── layouts/         # Page layout wrappers (Layout.astro)
├── lib/             # Utilities and service clients (utils.ts, supabase.ts)
├── pages/           # File-based routes (index.astro, about.astro, contact.astro)
└── styles/          # Global CSS with design system tokens (global.css)
public/              # Static assets served at root (favicons, robots.txt, uploaded images)
```

## Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Start dev server on `localhost:4321` with HMR |
| `npm run build` | Build static site to `./dist/` |
| `npm run preview` | Preview production build locally |
| `npm run astro -- check` | Run TypeScript type checking |

There is no test suite configured. No linter or formatter is configured in `package.json`.

## Architecture & Conventions

### Component Patterns

- **Astro components** (`.astro`) are used for all layout, navigation, and page-level UI. They render server-side with zero client JS by default.
- **React components** (`.tsx`) should be used only when client-side interactivity is needed (Astro island architecture). Add `client:load` or `client:visible` directives when embedding React in Astro pages.
- Component files use **PascalCase** naming (e.g., `Navigation.astro`, `Footer.astro`).
- The `Layout.astro` wrapper accepts a `title` prop and provides the `<head>`, global styles, nav, and footer for all pages.

### Styling

- **Dark-only theme**: Pure black (`#000`) background with burnt orange (`hsl(25, 100%, 40%)`) as the primary accent.
- Design tokens are CSS custom properties defined in `src/styles/global.css` under `:root`.
- Use Tailwind utility classes. For className merging in React/TSX, use the `cn()` helper from `src/lib/utils.ts`.
- Custom component classes (e.g., `bg-gradient-card`, `shadow-elegant`, `transition-smooth`) are defined in `@layer components` in `global.css`.
- The phone mockup component has dedicated CSS classes (`phone-3d`, `phone-screen`, etc.) with 3D transform animations.

### Key Design Tokens

| Token | Value | Usage |
|-------|-------|-------|
| `--background` | `0 0% 0%` | Page background (pure black) |
| `--primary` | `25 100% 40%` | Burnt orange accent, CTAs, links |
| `--primary-glow` | `25 100% 50%` | Hover/glow states |
| `--card` | `0 0% 3%` | Card surfaces |
| `--border` | `0 0% 12%` | Subtle borders |
| `--muted-foreground` | `0 0% 65%` | Secondary text |

### Routing

Astro file-based routing in `src/pages/`:
- `/` → `index.astro` (hero + features)
- `/about` → `about.astro` (company story)
- `/contact` → `contact.astro` (demo request form)

Navigation and Footer reference additional pages not yet created: `/solutions`, `/pricing`, `/testimonials`, `/privacy`, `/terms`, `/sms-privacy-terms`.

### Form Handling

The contact form (`contact.astro`) uses client-side vanilla JS for validation and submission. Form data is currently logged to console — no backend submission is wired up yet. The Supabase client in `src/lib/supabase.ts` is configured but not connected to the form.

## Brand & Content Guidelines

- **Tone**: Premium, sophisticated, compliance-focused. Target audience is upscale venue owners.
- **Industry terms**: TCPA compliance, CAN-SPAM, SMS marketing, VIP reservations, concierge integration.
- **Visual identity**: Dark backgrounds, warm orange accents, elegant typography, minimal aesthetic inspired by Resend's design language.
- **Logos**: SVG logos in `src/assets/` — both Gnymble and PercyTech (parent company) icon and text variants.

## Working with This Codebase

- Always run `npm run build` after changes to verify the build succeeds (no test suite to run).
- Astro pages are `.astro` files mixing HTML-like template syntax with a frontmatter script block (fenced by `---`).
- When adding new pages, follow the existing pattern: import `Layout`, wrap content in `<Layout title="...">`, use the same section/container structure.
- Keep client-side JavaScript minimal — prefer Astro's server-side rendering. Only use React islands for genuinely interactive UI.
- Images in `src/assets/` are processed by Astro's image pipeline. Images in `public/` are served as-is.
