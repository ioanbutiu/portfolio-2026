# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Next.js 16 portfolio frontend application with App Router that connects to a Sanity CMS backend. The Sanity Studio v5 project is maintained in a separate repository.

The frontend features real-time visual editing through Sanity's Presentation Tool, Live Content API for dynamic updates, and support for a drag-and-drop page builder system.

**Sanity Backend Repository**: Located separately from this frontend repo. Schema definitions and content management are handled there.

## Commands

### Development
```bash
# Start frontend development server (runs on localhost:3000)
npm run dev
```

### Building and Deployment
```bash
# Build frontend (auto-generates types first via prebuild)
npm run build

# Generate Sanity types from the backend schema
npm run typegen
```

### Code Quality
```bash
# Lint code
npm run lint

# Type check
npm run type-check

# Format code with Prettier
npm run format
```

## Architecture

### Frontend Architecture
- **Framework**: Next.js 16 App Router with React 19
- **Sanity Integration**:
  - `sanity/lib/client.ts` - Sanity client with Stega encoding for visual editing
  - `sanity/lib/live.ts` - `sanityFetch` wrapper for Live Content API with automatic revalidation
  - `sanity/lib/queries.ts` - GROQ queries for content fetching
  - Type generation via `sanity typegen generate` into `sanity.types.ts`

### Page Builder System
The `page` document type (defined in the Sanity backend) includes a `pageBuilder` array field that accepts two block types:
- `callToAction` - CTA sections with buttons and links
- `infoSection` - Content sections with text and images

**Key components**:
- `app/components/PageBuilder.tsx` - Main page builder renderer using `useOptimistic` hook for real-time updates during visual editing
- `app/components/BlockRenderer.tsx` - Renders individual page builder blocks with proper data attributes for Sanity overlays
- `app/[slug]/page.tsx` - Dynamic page route that fetches and renders pages with the page builder

The page builder supports drag-and-drop reordering in Sanity Studio with visual editing overlays on the frontend.

### Visual Editing & Real-time Updates
- **Draft Mode**: Enabled via `/api/draft-mode/enable` route for preview functionality
- **Data Attributes**: Components use `dataAttr()` utility (from `sanity/lib/utils.ts`) to add `data-sanity` attributes for Sanity overlay clickability
- **Optimistic Updates**: `useOptimistic` hook in PageBuilder reconciles references during drag-and-drop operations
- **Live Content**: `SanityLive` component wraps the app to enable automatic content refreshing

### Environment Variables
Required environment variables are documented in `.env.example`:

- `NEXT_PUBLIC_SANITY_PROJECT_ID` - Sanity project ID
- `NEXT_PUBLIC_SANITY_DATASET` - Dataset name
- `NEXT_PUBLIC_SANITY_API_VERSION` - API version (e.g., "2025-09-25")
- `NEXT_PUBLIC_SANITY_STUDIO_URL` - Studio URL (for linking to the backend)
- `SANITY_API_READ_TOKEN` - Read token for private datasets and draft content

### Content Schema
The content schema is defined in the separate Sanity backend repository. Document types include:

1. **Documents** - Top-level content types:
   - `page` - Custom pages with name, slug, heading, subheading, and pageBuilder array
   - `post` - Blog posts with title, slug, content (blockContent), excerpt, coverImage, date, and author reference
   - `person` - Authors/people with firstName, lastName, image, and bio

2. **Singletons** - Single-instance documents:
   - `settings` - Global site settings

3. **Objects** - Reusable schema components:
   - `blockContent` - Rich text with images, embeds, and custom marks
   - `blockContentTextOnly` - Text-only rich text variant
   - `button` - Button with text and link
   - `link` - Internal/external link object
   - `callToAction` - CTA block for page builder
   - `infoSection` - Info section block for page builder

### TypeScript & Type Generation
- Frontend types are auto-generated from Sanity schemas using `sanity typegen generate`
- Type generation runs automatically before dev/build via `predev` and `prebuild` scripts
- Generated types are located in `sanity.types.ts`

## Important Patterns

### Adding New Page Builder Blocks
To add new blocks to the page builder:
1. Add schema in the Sanity backend repository
2. Add to `page.ts` pageBuilder field's `of` array in the backend
3. Create React component in `app/components/`
4. Add rendering logic to `BlockRenderer.tsx`
5. Generate new types with `npm run typegen`

### GROQ Queries
- All GROQ queries are centralized in `sanity/lib/queries.ts`
- Use `sanityFetch` from `sanity/lib/live.ts` instead of direct client calls for automatic revalidation
- Queries should select only needed fields to optimize performance
- Use projections to shape data and include references with `->` operator

### Visual Editing Data Attributes
Components that should be editable in Sanity Presentation Tool must include `data-sanity` attributes:
```tsx
data-sanity={dataAttr({
  id: document._id,
  type: document._type,
  path: 'fieldName'
}).toString()}
```

### Working with References
- References use `reference` type in schemas with `to: [{type: 'documentType'}]` (defined in Sanity backend)
- In GROQ queries, dereference with `->` operator: `author->{firstName, lastName}`
- For multiple references, use array syntax: `authors[]->`

## Key Files Reference

- `next.config.ts` - Next.js configuration with Sanity image domains
- `sanity/lib/client.ts` - Sanity client configuration
- `sanity/lib/live.ts` - Live Content API setup
- `sanity/lib/queries.ts` - Centralized GROQ queries
- `app/components/PageBuilder.tsx` - Page builder renderer with optimistic updates
- `app/components/BlockRenderer.tsx` - Individual block renderer with Sanity data attributes
