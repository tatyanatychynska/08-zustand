# NoteHub

A note management application built with Next.js (App Router), featuring SSR with TanStack Query hydration, advanced routing (parallel routes, intercepting routes, catch-all routes), full CRUD functionality, SEO metadata, and draft auto-save via Zustand.

## Live Demo

[https://08-zustand-seven-ivory.vercel.app](https://08-zustand-seven-ivory.vercel.app)

## Features

- Browse and search notes with debounced input
- Filter notes by tag via sidebar navigation
- Create notes on a dedicated page with native HTML form validation
- Auto-save form draft to Zustand store and localStorage — draft persists across page reloads
- Delete notes
- Preview note details in a modal without leaving the current page
- View individual note details on a dedicated page
- Server-side rendering with TanStack Query prefetch and cache hydration
- SEO metadata and Open Graph tags for all pages
- Global loading and error boundaries
- Custom 404 page
- Roboto font loaded globally via next/font/google

## Pages

| Route | Description |
|-------|-------------|
| `/` | Home page with app info |
| `/notes/filter/[tag]` | Notes list filtered by tag (catch-all route) |
| `/notes/filter/all` | All notes without filtering |
| `/notes/[id]` | Full note detail page (SSR) |
| `/notes/action/create` | Create new note page with draft auto-save |

## Routing highlights

- **Parallel routes** — `@sidebar` slot renders the tag filter menu alongside the notes list; `@modal` slot renders note previews as an overlay
- **Catch-all route** — `/notes/filter/[...slug]` handles all tag filters including `/all`
- **Intercepting route** — navigating to `/notes/[id]` from the filter page opens a modal preview instead of a full page navigation; direct URL access still loads the full page

## SEO

All pages export `metadata` or `generateMetadata` (typed as `Metadata` from `next`) with `title`, `description`, and `openGraph` fields (title, description, url, images). Dynamic pages (`/notes/[id]`, `/notes/filter/[...slug]`) generate metadata at request time.

## Tech Stack

- [Next.js](https://nextjs.org) — App Router, SSR, parallel routes, intercepting routes
- [TanStack Query](https://tanstack.com/query) — server state, prefetch, hydration
- [Zustand](https://zustand-demo.pmnd.rs) — draft note state with `persist` middleware (localStorage)
- [Axios](https://axios-http.com) — HTTP requests
- TypeScript, CSS Modules

## Getting Started

1. Clone the repository
2. Install dependencies:

```bash
npm install
```

3. Create a `.env` file in the root:

```
NEXT_PUBLIC_NOTEHUB_TOKEN=your_token_here
```

4. Run the development server:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Project Structure

```
app/
  page.tsx                        # Home page
  layout.tsx                      # Root layout — Header, Footer, TanStackProvider, @modal slot, Roboto font, global metadata
  loading.tsx                     # Global loading fallback
  not-found.tsx                   # Custom 404 page
  @modal/
    default.tsx                   # Renders null when no modal is active
    (.)notes/
      [id]/
        page.tsx                  # Intercepting route — prefetches note, renders NotePreviewClient
        NotePreview.client.tsx    # Modal with note details (client component)
  notes/
    [id]/
      page.tsx                    # Full note detail page (SSR + prefetch + generateMetadata)
      NoteDetails.client.tsx      # Client component for note details
      error.tsx                   # Error boundary for /notes/[id]
    action/
      create/
        page.tsx                  # Create note page — metadata export + renders CreateNote
        CreateNote.tsx            # Page layout with NoteForm
    filter/
      layout.tsx                  # Filter layout — @sidebar slot + children
      @sidebar/
        page.tsx                  # SidebarNotes — tag filter links
        default.tsx               # Default sidebar render
      [...slug]/
        page.tsx                  # Notes page by tag (SSR + prefetch + generateMetadata)
        Notes.client.tsx          # Search, pagination, note list, Create note link (client component)
        error.tsx                 # Error boundary
components/
  Header/                         # Navigation header
  Footer/                         # Footer
  TanStackProvider/               # QueryClientProvider wrapper
  NoteList/                       # Notes list with delete and view details
  NoteForm/                       # Create note form (native HTML form, Zustand draft, onChange auto-save)
  Modal/                          # Portal-based modal
  SearchBox/                      # Debounced search input
  Pagination/                     # Pagination control
  Loader/                         # Loading spinner
  ErrorMessage/                   # Inline error display
lib/
  api.ts                          # fetchNotes, fetchNoteById, createNote, deleteNote
  store/
    noteStore.ts                  # Zustand store — draft state with persist middleware
types/
  note.ts                         # Note, NoteTag, NoteFormValues
```