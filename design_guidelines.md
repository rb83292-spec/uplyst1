# Uplyst Web Design Guidelines

## Design Approach

**Reference-Based Design** drawing inspiration from:
- **IMDb**: Rating display patterns and detail layouts
- **Product Hunt**: Voting mechanics and list card design
- **Letterboxd**: Grid-based content discovery and visual list aesthetics
- **Pinterest**: Masonry-style browsing for category exploration
- **Reddit**: Comment threading and engagement patterns

## Typography System

**Font Family**: Inter or Roboto (Google Fonts CDN)

**Hierarchy**:
- Hero/Page Titles: text-4xl md:text-5xl, font-bold
- List Titles: text-2xl md:text-3xl, font-bold
- Section Headers: text-xl md:text-2xl, font-semibold
- Entry Titles: text-lg, font-semibold
- Body Text: text-base, font-normal
- Metadata (votes, dates): text-sm, font-medium
- Captions/Labels: text-xs, font-medium, uppercase tracking-wide

## Layout System

**Spacing Primitives**: Use Tailwind units of **2, 4, 8, 12, 16, 24** consistently throughout (p-2, m-4, gap-8, py-12, etc.)

**Container Strategy**:
- Max width: max-w-7xl for main content areas
- List grids: max-w-6xl
- Detail views: max-w-4xl for optimal readability
- Full-width sections for category browsing

**Grid Patterns**:
- List Cards: grid-cols-1 md:grid-cols-2 lg:grid-cols-3 with gap-8
- Category Tiles: grid-cols-2 md:grid-cols-3 lg:grid-cols-4 with gap-4
- Entry Items: Single column with generous vertical spacing (space-y-6)

## Component Library

### Navigation Header
- Sticky top navigation with shadow
- Logo/brand on left, search bar center, user actions right
- Height: h-16, padding: px-4 md:px-8
- Tab navigation (Trending/New/Top) below main header with border-b separator

### List Cards
- Rounded corners: rounded-xl
- Shadow: shadow-md with hover:shadow-lg transition
- Padding: p-6
- Structure: Thumbnail (aspect-square, rounded-lg) + metadata stack
- Footer: Flex row with vote count, rating badge, entry count
- Trending badge: Absolute positioned top-right corner, rounded-full px-3 py-1 text-xs

### Entry Display (List Detail)
- Rank badge: Absolute positioned, large bold number (text-3xl), top-left
- Horizontal layout: flex gap-6
- Thumbnail: w-20 h-20 md:w-24 md:h-24, rounded-lg, flex-shrink-0
- Content: flex-1 with title, description, rating row
- Rating display: Inline flex with star icon, numeric value (text-xl font-bold), vote count in parentheses
- Comment toggle: Collapsible section with smooth transitions

### Rating Interface
- Interactive slider: Full-width with 1-10 markers
- Large preview of selected rating: text-4xl font-bold centered above slider
- Star icon array: 10 filled/outlined stars corresponding to rating
- Submit button: Full-width, rounded-lg, py-3, font-semibold

### Voting Buttons
- Icon buttons: Rounded-full with p-3
- Upvote/downvote stacked vertically in column
- Net vote count between buttons: text-lg font-bold
- Active state: Filled background vs outlined

### Comment Thread
- Nested indentation: pl-8 for each reply level (max 3 levels)
- User avatar: w-8 h-8 rounded-full
- Timestamp: text-xs with relative time
- Reply button: text-sm underlined
- Moderator badge: Inline badge next to username

### Category Browser
- Large clickable tiles: aspect-video or aspect-square
- Background image with gradient overlay
- Category name: Centered, text-2xl font-bold
- Entry count badge: Absolute bottom-right

### Search & Filters
- Search bar: Full-width with icon, rounded-full, h-12
- Filter chips: Inline flex wrap with gap-2, rounded-full px-4 py-2
- Active filter: Filled background vs outlined
- Clear all: Text button at end of filter row

### Moderation Controls (Create/Edit List)
- Drag-and-drop entries: Visual grab handle icon on left
- Add entry form: Modal with multi-step wizard (title → description → thumbnail upload)
- Image upload: Dashed border dropzone with preview thumbnail
- Reorder interface: Vertical list with hover:bg treatment

### Modal/Auth Screens
- Centered modal: max-w-md, rounded-2xl, p-8
- Close button: Absolute top-right
- Social login buttons: Full-width with icon+text, stacked with gap-3
- Divider: "OR" text with horizontal lines

## Images

**Hero Section**: Full-width banner image (aspect-[21/9] on desktop, aspect-video on mobile) showcasing community-created lists with subtle gradient overlay. Position primary CTA ("Create Your First List" or "Explore Trending") centered with blurred background button treatment.

**List Thumbnails**: Square (1:1) images throughout, lazy-loaded with skeleton placeholders. Fallback: Centered icon on muted background.

**Category Tiles**: Background images for each category (Music: vinyl record, Movies: film reel, Tech: circuit board, etc.) with text overlay.

**User Avatars**: Circular profile images in comments and user profiles.

## Accessibility & Interaction

- Focus states: ring-2 ring-offset-2 on all interactive elements
- Button states: Subtle scale transform on active (transform active:scale-95)
- Hover states: Opacity or background transitions (duration-200)
- Loading states: Skeleton screens matching content structure
- Empty states: Centered icon + message + CTA button
- Error states: Inline error messages below form fields (text-sm)

## Responsive Breakpoints

- Mobile-first approach
- Tablet (md:): 768px - Two-column grids, increased padding
- Desktop (lg:): 1024px - Three-column grids, max-width containers
- Wide (xl:): 1280px - Four-column category grids

## Page-Specific Layouts

**Home/Discovery**: Tab navigation → Grid of list cards → Infinite scroll loading indicator

**List Detail**: Breadcrumb navigation → List header (title, creator, votes) → Sort dropdown → Entry list → Comments section

**Category View**: Hero banner → Grid of lists filtered by category → Sidebar with trending lists in category

**Create List**: Multi-step form wizard with progress indicator → Entry builder with live preview → Publish confirmation

This design creates a modern, content-rich platform balancing discovery, engagement, and moderation tools with clean, scannable layouts optimized for browsing curated lists.