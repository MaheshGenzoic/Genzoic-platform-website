# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Genzoic Platform Website - A React-based marketing website showcasing AI agents and automation solutions. Built with Vite, React 18, TypeScript, and Tailwind CSS with shadcn/ui components.

## Development Commands

```bash
# Install dependencies
npm install
# or with bun
bun install

# Start development server (runs on http://localhost:8080)
npm run dev
# or
bun dev

# Build for production
npm run build

# Build for development (includes source maps)
npm run build:dev

# Preview production build
npm run preview

# Lint code
npm run lint
```

## Architecture

### Routing Structure

Routing is defined in `src/App.tsx` using React Router DOM. The app uses client-side routing with the following patterns:

- `/` - Homepage (Index.tsx)
- `/about` - About page
- `/services` - Services page
- `/all-agents` - Agent marketplace/listing
- `/agent/:slug` - Dynamic agent detail pages (uses slug from agentData)
- `/blogs` - Blog listing page
- `/blog/:id` - Dynamic blog detail pages (uses numeric IDs)
- `/contact` - Contact form
- `*` - 404 Not Found page

**Note:** Some legacy routes exist for specific agents (e.g., `/omnichannel-support-agent`, `/order-verification-agent`) that may need consolidation with the dynamic `/agent/:slug` pattern.

### Data Architecture

This is a **static site with no database**. All data is TypeScript/JavaScript files:

- **Agent Data:** `/src/utils/agentData.ts` - Array of agent objects with id, name, slug, description, tags, icon (Lucide React), image (Unsplash URL), howItWorks, keyCapabilities, and useCases
- **Blog Content:** `/src/content/blog[N]-sections.ts` - Individual files for each blog post with structured sections
- **Blog Loading:** `BlogDetail.tsx` uses dynamic imports to load blog content based on ID from URL params

### Image Strategy

**Critical:** This project uses a specific image strategy to minimize bundle size and local asset management:

- **Local Images (in `/public/lovable-uploads/`):** ONLY for logos, favicons, and essential UI assets
- **Content Images:** ALL blog images, agent images, and marketing photos use external Unsplash URLs
- **Unsplash URLs:** Include size/format parameters (e.g., `?w=600&h=400&fit=crop&auto=format`)

When adding new content, **always use Unsplash URLs** for images, not local files.

### Component Architecture

- **Page Components:** `/src/pages/` - Each route has a dedicated page component
- **Shared Components:** `/src/components/` - Header, Footer, ReadyToTransform (CTA section)
- **UI Components:** `/src/components/ui/` - shadcn/ui components (DO NOT modify these directly; regenerate using shadcn CLI if needed)
- **Path Aliases:** `@/` maps to `/src/` (configured in vite.config.ts and tsconfig.json)

### Styling

- **Tailwind CSS:** Primary styling approach - use utility classes
- **shadcn/ui:** Component library configured in `components.json` with default style, slate base color, and CSS variables enabled
- **Theme:** Uses next-themes for dark mode support (configured in the app)
- **Global Styles:** `src/index.css` contains Tailwind directives and CSS variables

## Adding New Content

### Adding a New Agent

1. **Add agent data** to `/src/utils/agentData.ts`:
   ```typescript
   {
     id: 26, // Increment from last ID
     name: "Agent Name",
     slug: "agent-name-slug", // Used in URL
     description: "Brief description",
     tags: ["Tag1", "Tag2"],
     icon: LucideIcon, // Import from lucide-react
     image: "https://images.unsplash.com/...", // Unsplash URL with params
     howItWorks: ["Step 1", "Step 2", "Step 3"],
     keyCapabilities: ["Capability 1", "Capability 2"],
     useCases: ["Use case 1", "Use case 2"]
   }
   ```

2. **Test** at `/agent/agent-name-slug` (no route changes needed; handled by AgentDetails.tsx)

### Adding a New Blog Post

1. **Create content file** at `/src/content/blog[N]-sections.ts` (increment N from last blog):
   ```typescript
   export const blog[N]Sections = {
     hero: {
       title: "Blog Title",
       subtitle: "Subtitle",
       author: "Author Name",
       date: "Month Day, Year",
       readTime: "X min read",
       image: "https://images.unsplash.com/...", // Unsplash URL
       category: "Category"
     },
     sections: [
       {
         title: "Section Title",
         content: "Section content...",
         image: "https://images.unsplash.com/..." // Optional
       }
     ]
   };
   ```

2. **Update BlogDetail.tsx** to import and map the new blog (check the existing blog loading logic around line 20-50)

3. **Add to blog listing** in `Blogs.tsx` if not automatically included

### Adding a New Page

1. **Create page component** in `/src/pages/PageName.tsx`
2. **Add route** in `/src/App.tsx`:
   ```typescript
   import PageName from "./pages/PageName";
   // In Routes:
   <Route path="/page-path" element={<PageName />} />
   ```
3. **Update navigation** in `/src/components/Header.tsx` if needed

## Technical Notes

### Vite Configuration

- **Dev Server:** Runs on port 8080 with IPv6 host (`::`), accessible at http://localhost:8080
- **Plugins:** React SWC for fast refresh, lovable-tagger for component development tracking (dev mode only)
- **Aliases:** `@/` resolves to `./src/`

### TypeScript

- **Config:** Uses `tsconfig.json` (base), `tsconfig.app.json` (app code), and `tsconfig.node.json` (Vite config)
- **Strict Mode:** TypeScript strict mode is enabled

### Dependencies

- **React:** 19.2.1 (React 19 with new features)
- **React Router DOM:** 7.10.1 (latest v7 with new routing APIs)
- **Lucide React:** 0.556.0 (icon library - all agent icons come from here)
- **shadcn/ui:** Multiple @radix-ui packages for accessible components
- **Form Handling:** react-hook-form + @hookform/resolvers + zod for validation
- **Analytics:** @vercel/analytics for tracking

### Deployment

- **Platform:** Vercel (configured in vercel.json)
- **Build Command:** `vite build`
- **Output Directory:** `dist/`
- **Preview:** Production builds can be previewed locally with `npm run preview`

## Code Conventions

### File Naming

- **Components:** PascalCase (e.g., `Header.tsx`, `ReadyToTransform.tsx`)
- **Pages:** PascalCase (e.g., `Index.tsx`, `AgentDetails.tsx`)
- **Utilities:** camelCase (e.g., `agentData.ts`, `googleSheets.ts`)
- **Content:** kebab-case with numeric suffix (e.g., `blog1-sections.ts`)

### Import Paths

Always use path aliases for internal imports:
```typescript
import { Button } from "@/components/ui/button";
import { agentsData } from "@/utils/agentData";
```

### Component Structure

Pages follow this pattern:
1. Import statements
2. Component definition with TypeScript types
3. State and hooks
4. Helper functions
5. Return JSX with Header, main content, Footer, and ScrollToTop

### Styling Patterns

- Use Tailwind utility classes directly in JSX
- Follow existing spacing patterns (consistent padding/margins)
- Maintain responsive design with Tailwind breakpoints (sm:, md:, lg:, xl:)
- Use shadcn/ui components for complex UI elements

## Known Issues

- **Security Vulnerabilities:** Moderate vulnerabilities exist in esbuild/vite (development dependencies only, not in production builds)
- **Legacy Routes:** Some hardcoded agent routes exist alongside the dynamic `/agent/:slug` pattern (potential for consolidation)
- **Blog Loading:** BlogDetail.tsx uses manual dynamic imports rather than a registry pattern (could be refactored for scalability)
