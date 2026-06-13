# NoteHub

A note management application built with Next.js (App Router), featuring SSR with TanStack Query hydration, advanced routing (parallel routes, intercepting routes, catch-all routes), and full CRUD functionality.

## Features

- Browse and search notes with debounced input
- Filter notes by tag via sidebar navigation
- Create notes via a modal form with validation
- Delete notes
- Preview note details in a modal without leaving the current page
- View individual note details on a dedicated page
- Server-side rendering with TanStack Query prefetch and cache hydration
- Global loading and error boundaries
- Custom 404 page

## Pages

| Route | Description |
|-------|-------------|
| `/` | Home page with app info |
| `/notes/filter/[tag]` | Notes list filtered by tag (catch-all route) |
| `/notes/filter/all` | All notes without filtering |
| `/notes/[id]` | Full note detail page (SSR) |

## Routing highlights

- **Parallel routes** — `@sidebar` slot renders the tag filter menu alongside the notes list; `@modal` slot renders note previews as an overlay
- **Catch-all route** — `/notes/filter/[...slug]` handles all tag filters including `/all`
- **Intercepting route** — navigating to `/notes/[id]` from the filter page opens a modal preview instead of a full page navigation; direct URL access still loads the full page

## Tech Stack

- [Next.js](https://nextjs.org) — App Router, SSR, parallel routes, intercepting routes
- [TanStack Query](https://tanstack.com/query) — server state, prefetch, hydration
- [Axios](https://axios-http.com) — HTTP requests
- [Formik](https://formik.org) + [Yup](https://github.com/jquense/yup) — form and validation
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
  layout.tsx                      # Root layout — Header, Footer, TanStackProvider, @modal slot
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
      page.tsx                    # Full note detail page (SSR + prefetch)
      NoteDetails.client.tsx      # Client component for note details
      error.tsx                   # Error boundary for /notes/[id]
    filter/
      layout.tsx                  # Filter layout — @sidebar slot + children
      @sidebar/
        page.tsx                  # SidebarNotes — tag filter links
        default.tsx               # Default sidebar render
      [...slug]/
        page.tsx                  # Notes page by tag (SSR + prefetch)
        Notes.client.tsx          # Search, pagination, note list (client component)
        error.tsx                 # Error boundary
components/
  Header/                         # Navigation header
  Footer/                         # Footer
  TanStackProvider/               # QueryClientProvider wrapper
  NoteList/                       # Notes list with delete and view details
  NoteForm/                       # Create note form (Formik + Yup)
  Modal/                          # Portal-based modal
  SearchBox/                      # Debounced search input
  Pagination/                     # Pagination control
  Loader/                         # Loading spinner
  ErrorMessage/                   # Inline error display
lib/
  api.ts                          # fetchNotes, fetchNoteById, createNote, deleteNote
types/
  note.ts                         # Note, NoteTag, NoteFormValues
```
