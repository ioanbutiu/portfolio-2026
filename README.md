# Portfolio Frontend

A modern [Next.js](https://nextjs.org/) 16 portfolio frontend application connected to a [Sanity](https://www.sanity.io/) CMS backend. This repository contains only the frontend code—the Sanity Studio project is maintained separately.

## Features

- **Next.js 16 for Performance:** Leverage the power of Next.js 16 App Router for blazing-fast performance and SEO-friendly static sites.
- **Real-time Visual Editing:** Edit content live with Sanity's [Presentation Tool](https://www.sanity.io/docs/presentation) and see updates in real time.
- **Live Content:** The [Live Content API](https://www.sanity.io/live) delivers live, dynamic experiences without complexity or scalability challenges.
- **Customizable Pages with Drag-and-Drop:** Render pages using a drag-and-drop page builder system powered by the Sanity backend.
- **Dynamic Content Rendering:** Components automatically update when content changes in Sanity.
- **Visual Editing Overlays:** Components support [Sanity Presentation Tool](https://www.sanity.io/visual-editing-for-structured-content) for inline editing.
- **Draft Preview Mode:** View draft content before publishing via the draft mode API route.
- **Optimized Image Handling:** Sanity-optimized images with automatic alt text and responsive sizing.

## Getting Started

### Prerequisites

- Node.js 18+ and npm
- A Sanity project and dataset (in the separate Sanity backend repository)
- Access to the Sanity project ID and dataset name

### Installation

1. Clone this repository:
```shell
git clone <your-repo-url>
cd portfolio-2026
```

2. Install dependencies:
```shell
npm install
```

3. Set up environment variables by copying `.env.example` to `.env.local` and filling in your Sanity credentials:
```shell
cp .env.example .env.local
```

4. Start the development server:
```shell
npm run dev
```

The app will be available at [http://localhost:3000](http://localhost:3000).

### Environment Variables

Create a `.env.local` file with the following variables:

```
NEXT_PUBLIC_SANITY_PROJECT_ID=your_project_id
NEXT_PUBLIC_SANITY_DATASET=production
NEXT_PUBLIC_SANITY_API_VERSION=2025-09-25
NEXT_PUBLIC_SANITY_STUDIO_URL=https://your-studio.sanity.studio
SANITY_API_READ_TOKEN=your_read_token
```

## Development

### Commands

```bash
# Start development server
npm run dev

# Build for production
npm run build

# Type check
npm run type-check

# Lint code
npm run lint

# Format code with Prettier
npm run format

# Generate types from Sanity schema
npm run typegen
```

## Architecture

This is a Next.js 16 App Router application with:

- **Sanity Integration**: Client-side configuration for fetching content
- **Live Content API**: Real-time updates without page refreshes
- **Page Builder System**: Dynamic page rendering based on Sanity content
- **Visual Editing Support**: Data attributes for Sanity Presentation Tool integration
- **Draft Mode**: API route for preview functionality

See [CLAUDE.md](./CLAUDE.md) for detailed architecture documentation.

## Content Management

This frontend connects to a separate Sanity CMS backend. To manage content, schemas, and the Studio:

1. Navigate to the separate Sanity backend repository
2. Modify schemas in the `studio/src/schemaTypes/` directory
3. Deploy schema changes with `npx sanity schema deploy`
4. Access the Studio at your configured `NEXT_PUBLIC_SANITY_STUDIO_URL`

## Deployment

### Deploy to Vercel

1. Create a new project on [Vercel](https://vercel.com)
2. Connect your GitHub repository
3. Set environment variables in Vercel project settings
4. Deploy

Vercel will automatically build and deploy on every push to your main branch.

### Other Hosting Providers

You can deploy to any Node.js-compatible hosting provider:

1. Build the project: `npm run build`
2. Start the server: `npm run start`
3. Ensure all environment variables are configured on your hosting platform

## Resources

- [Next.js documentation](https://nextjs.org/docs)
- [Sanity documentation](https://www.sanity.io/docs)
- [Sanity Presentation Tool](https://www.sanity.io/docs/presentation)
- [Sanity Live Content API](https://www.sanity.io/live)
- [Join the Sanity Community](https://slack.sanity.io)
