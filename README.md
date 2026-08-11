# Promptspad

Promptspad is a Vite + React AI prompt marketplace and admin console for browsing, discovering, saving, and managing prompt content.

## Product Architecture

The application is structured as a front-end single-page web app powered by React Router. The main app shell is defined in [src/App.jsx](src/App.jsx) and establishes the public route tree, protected admin route tree, global providers, and top-level layout.

### Frontend Architecture

The frontend stack is organized around a React component hierarchy and route-driven pages:

- App shell and route wiring: [src/App.jsx](src/App.jsx)
- Global UI layout: [src/components/NavBar.jsx](src/components/NavBar.jsx), [src/components/Footer.jsx](src/components/Footer.jsx)
- Context providers for account, plan, saved prompts, admin mode, author mode, and modal state: [src/context/AuthContext.jsx](src/context/AuthContext.jsx), [src/context/SavedContext.jsx](src/context/SavedContext.jsx), [src/context/AdminContext.jsx](src/context/AdminContext.jsx), [src/context/AuthorContext.jsx](src/context/AuthorContext.jsx), [src/context/UpgradeModalContext.jsx](src/context/UpgradeModalContext.jsx)
- Shared design system and UI styling assets live under [src/components](src/components) and [src/index.css](src/index.css)

### Public-Facing Architecture

User-facing flows are implemented as public pages:

1. Listing / marketplace page
   - The main discovery experience is implemented through [src/pages/ExplorePage.jsx](src/pages/ExplorePage.jsx).
   - It supports category filtering, model filtering, rating filtering, plan filtering, search, and a prompt grid.
   - The page receives current auth state and save actions from the app shell.

2. Prompt details page
   - Individual prompt detail views are implemented in [src/pages/PromptDetailPage.jsx](src/pages/PromptDetailPage.jsx).
   - This page loads a prompt from the prompt dataset, controls premium access, allows save, copy, share, and AI assistant-style chat or generation flows.

3. Category browsing
   - The taxonomy/navigation page is implemented in [src/pages/CategoriesPage.jsx](src/pages/CategoriesPage.jsx).
   - It renders category cards and routes users into the explore listings experience.

4. News / blog surfaces
   - The public news experience is represented by [src/pages/NewsPage.jsx](src/pages/NewsPage.jsx) and the supporting UI in [src/pages/NewsSection.jsx](src/pages/NewsSection.jsx).
   - The admin news management surface uses [src/pages/admin/AdminNewsPage.jsx](src/pages/admin/AdminNewsPage.jsx) to create, edit, delete, and publish blog/news content.

### Admin Architecture

Admin functionality is exposed through protected routes in [src/App.jsx](src/App.jsx) and rendered inside the admin layout shell.

Admin modules include:

1. Users with roles
   - The user management page is [src/pages/admin/UsersPage.jsx](src/pages/admin/UsersPage.jsx).
   - It displays admin/user/pro roles and account status information.
   - Auth role handling is maintained in [src/context/AuthContext.jsx](src/context/AuthContext.jsx), where login, registration, plan changes, and role updates are persisted in local storage.

2. Create blog / news
   - The admin news page provides add/edit/delete forms for news/blog entries in [src/pages/admin/AdminNewsPage.jsx](src/pages/admin/AdminNewsPage.jsx).
   - Blog/news records contain title, body, tag, status, and date fields.

3. Publish blog / news
   - The same admin page exposes a status field with Published and Draft states, enabling draft-to-publish lifecycle management.
   - News items are stored in the UI as sample local data and can be surfaced in the public news page.

### Data and Service Layer

This repository is currently a frontend-focused implementation with browser-side persistence rather than a production-grade API server.

Main data and service files:

- Prompt/category/news seed data: [src/data/prompts.js](src/data/prompts.js)
- Centralized local storage-oriented service utilities: [src/services/dataService.js](src/services/dataService.js)
- AI runtime integration through OpenAI SDK calls: [src/services/openai.js](src/services/openai.js)
- Public prompt listing data and model/category metadata: [src/data/prompts.js](src/data/prompts.js)

The package manifest shows the presence of support libraries such as Express and dotenv, but the current codebase does not expose a dedicated Express API or server implementation. API-style integration is performed through client-side service functions and UI contexts.

## Stack

### Frontend

- React 18 for UI composition and app state-driven rendering
- Vite 4 for local development, bundling, and production build output
- React Router DOM 6 for client-side navigation across public and admin routes
- React Hot Toast for toast notifications
- Framer Motion for UI animation support
- Lucide React for iconography
- Recharts for admin dashboard/chart style analytics surfaces

### Styling and UI

- Tailwind CSS 3 for utility-driven layout and design system styling
- Custom CSS variables and global theme styling in [src/index.css](src/index.css)
- Shared UI components across the marketplace and admin console in [src/components](src/components)

### App State and Persistence

- React Context API for authentication, saved prompts, user plan, upgrade modal state, admin state, and author state
- Browser localStorage persistence for user records, saved state, admin/configuration-like data, and prompt/news records
- Static dataset-driven prompt/category/news content stored primarily in [src/data/prompts.js](src/data/prompts.js) and [src/services/dataService.js](src/services/dataService.js)

### AI and External Services

- OpenAI SDK integration through [src/services/openai.js](src/services/openai.js) for chat completion, prompt generation, and prompt improvement flows
- The runtime currently calls external AI capabilities from the browser layer rather than through a dedicated backend API

### Backend / Server Signal

- Express, CORS, and dotenv dependencies are present in [package.json](package.json), but no application server implementation is currently wired up in the source tree
- The implementation is therefore best described as a frontend-first prompt marketplace with backend-ready dependency scaffolding rather than a full server-rendered or API-backed system

## Routes Overview

The app exposes these primary public URLs:

- `/` — home/listing experience
- `/explore` — searchable prompt marketplace listing
- `/categories` — static category taxonomy navigation
- `/prompt/:id` — prompt detail display
- `/news` — public news/blog reading page
- `/checkout` — checkout/paywall flow
- `/dashboard` and `/profile` — account-related pages
- `/chat` — AI chat page

Protected admin URLs:

- `/admin` — dashboard
- `/admin/users` — user roles and user management
- `/admin/authors` — author review and author management
- `/admin/prompts` — prompt management
- `/admin/categories` — categories management
- `/admin/paywall` — paywall configuration
- `/admin/reports` — reports and analytics view
- `/admin/settings` — settings
- `/admin/news` — blog/news creation and publishing

## Getting Started

Install dependencies:

```bash
npm install
```

Run locally:

```bash
npm run dev
```

Build the production bundle:

```bash
npm run build
```

## Notes

Promptspad is designed as a frontend marketplace UI for prompt discovery, prompt detail consumption, AI generation, saved prompt workflows, and admin management. The current implementation relies heavily on static local datasets and browser localStorage for persistence, with OpenAI-powered AI actions executed from the browser service layer.
