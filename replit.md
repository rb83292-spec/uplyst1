# Uplyst - Community-Driven Rankings Platform

## Overview

Uplyst is a community-driven ranking platform that enables moderators to create curated lists across various categories (music, movies, tech, shopping, etc.) while allowing the community to discover, rate, vote, and discuss content. The platform features a mobile-first design inspired by IMDb, Product Hunt, Letterboxd, Pinterest, and Reddit.

**Core Purpose**: Provide community-validated recommendations through curated lists where users can rate entries (1-10 scale), vote on lists (upvote/downvote), and engage through threaded comments.

**User Roles**:
- **Anonymous Users**: Browse lists, view entries and ratings, leave one reaction per entry (stored locally), post one guest comment with nickname (goes to moderation)
- **Registered Users**: All anonymous capabilities plus unlimited rating, voting, and commenting
- **Moderators**: All registered capabilities plus list creation and management

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework**: React with TypeScript using Vite as the build tool

**Routing**: Wouter (lightweight client-side routing)
- Home page (`/`)
- List detail view (`/list/:id`)
- Category view (`/category/:category`)

**State Management**:
- TanStack Query (React Query) for server state management
- Local component state with React hooks for UI state
- Custom query client with credential-based authentication

**UI Component System**:
- shadcn/ui components (Radix UI primitives with Tailwind styling)
- "New York" style variant with custom theme configuration
- Comprehensive component library including forms, dialogs, cards, navigation
- Design system based on spacing primitives (2, 4, 8, 12, 16, 24) and responsive grid patterns
- Custom UpArrow SVG component for voting with pink/coral color (#FFA8A8)
- Logo using uploaded image with yellow star and three pink upward arrows

**Styling Approach**:
- Tailwind CSS with custom configuration
- CSS custom properties for theming (light/dark mode support)
- Hover/active elevation effects via utility classes (`hover-elevate`, `active-elevate-2`)
- HSL-based color system with alpha channel support

**Design Philosophy**:
- Mobile-first responsive design
- Reference-based design drawing from IMDb (ratings), Product Hunt (voting), Letterboxd (grids), Pinterest (masonry), and Reddit (threading)
- Typography hierarchy using Inter or Roboto fonts
- Consistent spacing and layout patterns across all views

### Backend Architecture

**Server Framework**: Express.js with TypeScript (ESM modules)

**API Structure**:
- RESTful API pattern with `/api` prefix
- Routes registered via `registerRoutes` function
- Request/response logging middleware
- JSON body parsing with raw body preservation for webhook support

**Development Setup**:
- Vite middleware integration for HMR in development
- Custom error overlay plugin
- Replit-specific development tooling (cartographer, dev banner)

**Storage Layer**:
- Interface-based storage abstraction (`IStorage`)
- In-memory storage implementation (`MemStorage`) for development
- CRUD operations through storage interface
- Designed to be swapped with database implementation

**Current Schema**:
- Users (id, username, password)
- Lists (id, title, description, listType, category, coverImage, createdBy, votes, createdAt)
- List types (enforced via PostgreSQL enum): "Top 50", "Top 20", "Top 10", "Top Rated", "Most Loved"
- List entries (id, listId, rank, title, description, imageUrl, externalUrl, embedUrl, averageRating, totalRatings, createdAt)
- Entry reactions (id, entryId, userId, reactionType, createdAt)
- Reaction types (enforced via PostgreSQL enum): "trending", "underrated", "loved", "overhyped"
- Entry comments (id, entryId, userId, comment, createdAt)
- Categories: Music, Movies, Artists, Streaming, Shopping, Tech

**List Creation**:
- CreateListModal component provides form interface for moderators
- Dropdown selection for predefined list types
- Category selection from available categories
- Optional cover image URL input for list artwork
- Form validation using Zod schemas
- Database-level enforcement of list types via pgEnum

**Entry Engagement**:
- Visual star ratings (1-10 scale displayed as 5 stars with partial fills)
- User reactions: Trending (🔥), Underrated (💎), Loved it (❤️), Overhyped (💡)
- One-line comments for quick feedback
- Reaction counts displayed on entry cards
- Smart embed system for music/media content:
  - Thumbnail display by default (fast, clean)
  - Hover reveals play button overlay
  - Click/tap reveals embedded player (Spotify/YouTube/SoundCloud)
  - External link icon for full platform access

### External Dependencies

**Database**: 
- Drizzle ORM configured for PostgreSQL
- Neon Serverless PostgreSQL driver
- WebSocket support for serverless connections
- Schema-first approach with Zod validation

**UI Libraries**:
- Radix UI primitives (20+ component packages for accessible UI patterns)
- Lucide React (icon library)
- class-variance-authority (CVA for component variants)
- tailwind-merge and clsx for className management

**Form Handling**:
- React Hook Form with Zod resolvers
- Type-safe form validation

**Date Handling**:
- date-fns for date formatting and manipulation

**Development Tools**:
- TypeScript with strict mode enabled
- Path aliases (@/, @shared/, @assets/)
- ESBuild for production bundling
- Drizzle Kit for database migrations

**Asset Management**:
- Static assets in `attached_assets/` directory
- Generated category background images
- Vite asset resolution with `@assets` alias

**Authentication** (Planned):
- Session-based approach indicated by connect-pg-simple dependency
- User/password authentication in schema

**Notes**:
- Database migrations stored in `./migrations` directory
- Environment variable `DATABASE_URL` required for database connection
- Production build outputs to `dist/` (client to `dist/public`, server to `dist/`)
- Mock data currently used throughout application (marked with `//todo: remove mock functionality`)

## Recent Changes

**November 6, 2025**:
- Updated logo to use uploaded brand image (yellow star with three pink upward arrows)
  - Logo image: `attached_assets/7af7ffe0-5148-4136-9184-b3f66ca2a0ac (1)_1762438554571.png`
  - Custom UpArrow component now uses pink/coral color (#FFA8A8) matching logo arrows
  - Upvote buttons throughout app use consistent pink/coral arrow color
- Implemented list creation functionality with predefined list types
  - Added CreateListModal component with form validation
  - List types enforced at database level via PostgreSQL enum
  - Only allowed types: "Top 50", "Top 20", "Top 10", "Top Rated", "Most Loved"
  - Create List button added to Header (visible when authenticated)
  - Updated all mock data to use new list type terminology
- Implemented major UI improvements based on user feedback:
  - **Cover Banners**: Added ListCoverBanner component with category-specific gradients (purple-pink-orange for Music, blue-indigo-purple for Movies, etc.) and dark gradient overlay for better text readability
  - **Typography**: Switched global font from Open Sans to Inter for modern editorial feel with high contrast
  - **Entry Cards**: Enhanced with subtle hover effects (card lift, shadow, color transitions on rank and title)
  - **Visual Ratings**: Replaced numeric-only ratings with 5-star system showing filled/partial/empty stars alongside numeric value (e.g., 9.4/10 = 4.7/5 stars)
  - **Reaction System**: Added EntryReactions component with two reaction types - Underrated (💎), Overhyped (💡) - displayed as interactive badges with counts
  - **Schema Enhancements**: 
    - Added list_entries table with numeric(3,1) for fractional ratings
    - Added entry_reactions table with enum-enforced reaction types
    - Added entry_comments table for one-line feedback
    - Added coverImage field to lists table
- Implemented smart embed system for music/media content:
  - **Smart Embed Strategy**: Hybrid approach balancing engagement and performance
  - **EmbeddedPlayer Component**: Auto-detects platform (Spotify/YouTube/SoundCloud) and renders appropriate iframe
  - **Entry Display**: Shows thumbnails by default with play button overlay on hover
  - **Interaction Flow**: Click/tap reveals embedded player below entry content
  - **External Links**: Clean icon in top-right corner provides escape hatch to full platform
  - **Schema Support**: Added externalUrl and embedUrl fields to list_entries table
- Added cover image support for lists:
  - **List Cards**: Display cover images (album art style) on home page
  - **Create Modal**: Optional cover image URL input field for list creators
  - **Visual Design**: 16:9 aspect ratio images with rounded corners
  - **Layout**: Cover image appears at top of list card above title and description
- Implemented guest browsing with limited engagement features:
  - **Guest Reactions**: Anonymous users can leave one reaction per entry (Underrated, Overhyped)
    - Reactions stored in browser localStorage via useGuestTracking hook
    - Each entry allows one reaction; clicking same reaction removes it
    - Guest reactions display with same visual treatment as authenticated user reactions
  - **Guest Comments**: Anonymous users can post one guest comment with nickname
    - GuestCommentForm component with nickname (max 30 chars) and comment (max 200 chars) inputs
    - Comments go to moderation (status: "pending") before appearing publicly
    - Form disables after submission with message encouraging signup
  - **Schema Support**: Added guestReactions and guestComments tables with moderation_status enum
  - **Progressive Engagement**: Toast notifications encourage signup after guest interactions
  - **Authentication Integration**: Replit Auth blueprint integrated for social login (Google, GitHub, etc.)
- Simplified reaction system based on user feedback:
  - **Reaction Types**: Reduced from 4 types to 2 types only - Underrated (💎) and Overhyped (💡)
  - **Removed**: Trending and Loved it reactions for cleaner, more focused engagement
  - **Schema**: Updated reactionTypeEnum to only include "underrated" and "overhyped"
  - **Components**: EntryReactions and EntryItem updated to only render two reaction badges
  - **Guest Tracking**: Continues to work seamlessly with simplified reaction set
- Implemented community suggestions UI (schema and components complete, backend pending):
  - **SuggestEntryForm**: Modal for logged-in users to suggest new entries to lists
  - **SuggestionItem**: Display component showing suggested entries with metadata
  - **SuggestionVoteButtons**: Upvote/downvote controls for community voting
  - **Community Suggestions Section**: Integrated into ListDetail page between entries and comments
  - **Schema**: Suggestions table with status flow (pending → community_review → approved/rejected)
  - **Note**: UI complete with mock data; backend integration still needed for persistence and auto-promotion logic
- Implemented search functionality and navigation:
  - **Search Button**: Added search button in header (desktop and mobile) with Enter key support
  - **Search Results Page**: Displays filtered lists matching search query with result count
  - **Category Pages**: Created dynamic category pages for all 6 categories (Music, Movies, Artists, Streaming, Shopping, Tech)
  - **Static Pages**: Created About, Guidelines, Moderators, FAQ, Terms of Service, Privacy Policy, and Contact pages
  - **Footer Navigation**: All footer links now functional and navigate to correct pages
  - **Contact Form**: Working contact form with validation and toast notifications