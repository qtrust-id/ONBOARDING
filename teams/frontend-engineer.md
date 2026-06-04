# Onboarding Guide — Frontend Engineer

**Team:** Frontend Engineering  
**Role Overview:** Build the web user interface that users interact with every day, faithfully translating Figma designs into polished, performant frontend templates.

---

## 1. Welcome

You are responsible for everything the user sees and touches in the web application. You work at the boundary between design and logic — taking Figma mockups from the UIX Designer and wiring them to the REST API from the Backend Engineer. Your output must be pixel-accurate, accessible, and fast.

Your frontend stack is defined in the Project Configuration Sheet. The examples in this guide use **Laravel Blade + Vite + Tailwind CSS + Alpine.js** as a reference implementation.

Claude Desktop is your pair programmer and code generator.

---

## 2. Your Responsibilities

- Implement all frontend views and layouts from approved Figma designs
- Build reusable components used across the application
- Set up and maintain the asset pipeline (CSS, JS, fonts, images)
- Integrate frontend views with backend API endpoints
- Ensure UI is responsive, accessible (WCAG 2.1 AA), and performant
- Write and maintain frontend-related tests (browser-level E2E tests when relevant — see Section 9 for tooling notes)
- Participate in design reviews to flag implementation constraints early
- Conduct visual QA — compare your implementation against the Figma source
- Integrate the Sentry SDK into the web app and triage the JavaScript errors, network failures, and component crashes it reports; confirm fixes reduce the error rate (see [`tools/sentry.md`](../tools/sentry.md)) — connect the Sentry MCP in Claude Desktop

---

## 3. Tech Stack

> **Note:** The tech stack below reflects one possible configuration. Your project's actual stack is defined in the Project Configuration Sheet.

| Technology | Version | Purpose |
|---|---|---|
| Laravel Blade | 13.x | Server-side templating engine |
| PHP | 8.3 | Server runtime |
| Vite | 6.x | Asset bundler (replaces webpack/mix) |
| Tailwind CSS | 4.x | Utility-first CSS framework |
| Alpine.js | 3.x | Lightweight reactive JS for UI interactions |
| Axios | 1.x | HTTP client for AJAX/API calls |
| Chart.js | 4.x | Data visualization (dashboards, reports) |
| Laravel Dusk | 8.x | Browser-based end-to-end tests (Laravel-specific — substitute with the appropriate E2E tool for your project's framework) |

---

## 4. Local Development Environment Setup

### 4.1 Prerequisites
Ensure the following are installed on your machine before cloning the repository.

> **Note:** The prerequisites below are for a Laravel-based stack. Your project's actual requirements depend on the tech stack — check the Project Configuration Sheet before proceeding.

```bash
# macOS (using Homebrew) — example for a Laravel/PHP/Node stack
brew install php@8.3 composer node@20

# Verify
php --version      # should be 8.3.x
composer --version
node --version     # should be 20.x
npm --version
```

You also need:
- **Docker Desktop** — for running PostgreSQL and Redis locally without installing them natively
- **Git** — configured with your GitHub account
- **VS Code** — recommended IDE (see extensions below)

### 4.2 Clone the Repository
```bash
git clone [REPO_URL]  # URL from Project Config Sheet
cd [project-folder]
```

### 4.3 Environment Configuration
```bash
cp .env.example .env
php artisan key:generate
```

Edit `.env` with your local database credentials:
```env
APP_NAME="[PROJECT_NAME]"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=[project-code]_dev
DB_USERNAME=postgres
DB_PASSWORD=secret

REDIS_HOST=127.0.0.1
REDIS_PORT=6379
```

### 4.4 Start Local Services (Docker)
```bash
# Start PostgreSQL and Redis
docker compose up -d

# Verify both are running
docker compose ps
```

### 4.5 Install Dependencies & Run Migrations
```bash
composer install
npm install
php artisan migrate --seed
```

### 4.6 Start Development Servers
Open two terminal windows:

```bash
# Terminal 1 — start the development server (command varies by framework)
# For Laravel: php artisan serve
# For other frameworks: refer to the Project Config Sheet

# Terminal 2 — Vite asset watcher (hot reload, if applicable)
npm run dev
```

Visit the local URL (see Project Config Sheet) — you should see the application.

### 4.7 Recommended VS Code Extensions

> **Note:** The extensions below are for a Laravel/Blade project. Add or substitute based on your project's stack.

```
Laravel Blade Snippets     — amirmitchell.blade-formatter
Laravel Blade formatter    — shufo.vscode-blade-formatter
Tailwind CSS IntelliSense  — bradlc.vscode-tailwindcss
Alpine.js IntelliSense     — alpine-js.alpine-js
PHP Intelephense           — bmewburn.vscode-intelephense
GitLens                    — eamodio.gitlens
Prettier                   — esbenp.prettier-vscode
```

---

## 5. Project Structure (Frontend-Relevant)

The example below reflects a Laravel Blade project structure. Your project's actual structure is defined by its framework — refer to the Project Configuration Sheet.

```
resources/
├── views/
│   ├── layouts/
│   │   ├── app.blade.php          ← Main authenticated layout
│   │   ├── auth.blade.php         ← Unauthenticated layout (login, etc.)
│   │   └── print.blade.php        ← Print layout (reports, exports)
│   ├── components/
│   │   ├── button.blade.php
│   │   ├── input.blade.php
│   │   ├── select.blade.php
│   │   ├── table.blade.php
│   │   ├── modal.blade.php
│   │   ├── badge.blade.php
│   │   ├── card.blade.php
│   │   ├── pagination.blade.php
│   │   ├── breadcrumb.blade.php
│   │   └── alert.blade.php
│   ├── auth/
│   │   ├── login.blade.php
│   │   └── forgot-password.blade.php
│   ├── dashboard/
│   │   └── index.blade.php
│   ├── [module-a]/
│   │   ├── index.blade.php
│   │   ├── create.blade.php
│   │   ├── edit.blade.php
│   │   └── show.blade.php
│   ├── [module-b]/
│   ├── [module-c]/
│   └── ...
├── css/
│   └── app.css                    ← Framework CSS + custom styles
└── js/
    ├── app.js                     ← Main JS entry point
    └── bootstrap.js               ← Axios, Echo setup
public/
└── build/                         ← Compiled output (do not edit)
```

---

## 6. Coding Standards & Conventions

### Blade Templates
- Use named slots for complex components: `<x-modal>`, `<x-button>`
- Prefer components over partials for reusable UI elements
- Never write inline JavaScript inside Blade templates — use Alpine.js `x-data` blocks or external JS files
- Always use the `@csrf` directive in forms
- Escape all user-generated content with `{{ }}`, never `{!! !!}` unless you are certain the source is safe

### Tailwind CSS
- Follow the design system tokens — do not hardcode color hex values in templates
- Use responsive prefixes consistently: `sm:`, `md:`, `lg:`, `xl:`
- Group utilities logically: positioning → sizing → spacing → typography → color → state
- Extract repeated utility combinations into components — not `@apply`
- Never use `!important` unless absolutely unavoidable

### Alpine.js
- Keep `x-data` objects small and focused — one concern per component
- Use `$store` for shared state across components on the same page
- Prefer Alpine.js `x-show` over `x-if` for toggling elements that are frequently shown/hidden (avoids DOM removal cost)

### JavaScript
- Use ES2020+ syntax (the project targets modern browsers)
- No jQuery — use your project's reactive JS library and Axios for all interactivity and HTTP
- All Axios calls go through a central `api.js` module — do not call endpoints inline in templates

### File Naming
- View templates: `kebab-case` (e.g., `kebab-case.blade.php`)
- Components: `kebab-case`
- JS modules: `camelCase.js`
- CSS custom classes: `kebab-case` with a `[project-code]-` prefix for custom utilities

---

## 7. Git Workflow

### Branching
Always branch from `develop`, never from `main`.

```bash
git checkout develop
git pull origin develop
git checkout -b feature/[module]-[short-description]

# Examples:
# feature/[module]-[description]
# fix/[module]-[description]
```

### Committing
Use conventional commits:
```
feat([module]): add [feature]
fix([module]): correct [issue]
style([module]): align [element] to design spec
refactor(components): extract reusable [component] component
test([module]): add E2E test for [flow]
```

### Pull Requests
- Target: `develop` branch (never `main`)
- Title must reference the GitHub Issue: `feat([module]): [description] [#issue-number]`
- Include screenshots comparing your implementation to the Figma mockup
- Request review from at least one other Frontend Engineer or the Tech Lead
- CI must pass before merging

> **Environments:** merging to `develop` auto-deploys to **staging** — verify your views there (and confirm against Figma) before marking the issue *Ready for QA*. Production is a gated deploy promoted from `main` with approval, never directly. Full flow: [`../environments-and-promotion.md`](../environments-and-promotion.md).

---

## 8. Reading Figma Designs

Before implementing any screen:
1. Open the corresponding Figma file (links in `by-team/uix/figma-links.md`)
2. Switch to **Dev Mode** in Figma to see exact dimensions, spacing values, and CSS suggestions
3. Read the handoff notes in `by-team/uix/handoff-notes/[module].md`
4. Check all screen states: default, loading, empty, error, hover, disabled

You can also ask Claude to read the Figma design directly:
> "Read the Figma frame at [URL] and generate a component that matches it exactly."

---

## 9. Working with Claude Desktop

> **Note:** The prompts below assume a project using **Laravel Blade + Tailwind CSS + Alpine.js**. If your project uses a different stack, adjust the framework references accordingly.

**Generate a component from a Figma design:**
> "Read the Figma design at [URL] and generate a reusable Blade component for the [Entity] Table. It should support pagination, a search input, and a status badge column. Use Tailwind CSS and Alpine.js for the search filter."

**Generate a full page view:**
> "Create a Blade view for the [Module] index page. It should have: a summary bar showing [key metrics], a table of [data] with status badges, and a '[Action]' button that opens a modal. Use the app.blade.php layout and Tailwind CSS."

**Implement an API integration:**
> "Write an Alpine.js component that fetches [data type] from `/api/v1/[endpoint]` using Axios, displays a loading skeleton while fetching, and renders the data in a table. Handle API errors with a dismissible alert."

**Write an E2E test (Laravel Dusk — Laravel-specific; substitute with the appropriate E2E tool for your project's framework):**
> "Write a Laravel Dusk test that: logs in as [User Role], navigates to the [Feature] page, searches for '[query]', and asserts that the results are filtered correctly."

> **⚡ Usage tip:** Design-to-code from Figma is one of the heaviest token workloads. Point Claude at a **specific Figma frame/node** rather than the whole file, **skip screenshots** when Dev Mode values suffice, and **iterate on the generated code** instead of re-reading the design each time. Heavy users should request a **Premium** seat. Full guidance: [Managing Token Usage & Quota](../tools/README.md#managing-token-usage--quota).

---

## 10. Collaboration With Other Teams

| Team | How You Work Together |
|---|---|
| **UIX Designer** | Primary handoff relationship. Receive Figma links + handoff notes. Flag any design constraints early (before starting implementation). |
| **Backend Engineer** | Consume the REST API. Coordinate on endpoint response shapes. If an API is missing or has wrong data, file a GitHub Issue with `team:backend`. |
| **QA / QC** | QA will test your implemented views. Ensure your work matches acceptance criteria before marking your issue as ready for QA. |
| **Product Development** | Clarify edge cases in user stories that affect the UI. |

---

## 11. First Week Checklist

- [ ] Repository cloned and development environment running locally
- [ ] `.env` configured, database migrated and seeded
- [ ] Application visible at local development URL (see Project Config Sheet)
- [ ] VS Code extensions installed
- [ ] Claude Desktop installed, workspace folder connected
- [ ] Figma view access granted — all design files reviewed
- [ ] `by-team/frontend/component-specs.md` read
- [ ] `designs/design-system/README.md` read — understand all tokens
- [ ] Sentry SDK integrated (DSN from Secret Manager) and Sentry MCP connected in Claude Desktop
- [ ] First GitHub Issue assigned and branch created
- [ ] Attended sprint planning for Sprint 0

---

## 12. Key Resources

> **Note:** The framework-specific links below apply to a Laravel-based stack. Substitute with the relevant documentation for your project's framework and tools.

| Resource | Location |
|---|---|
| Design System | `designs/design-system/README.md` |
| Figma Files | `by-team/uix/figma-links.md` |
| Component Specs | `by-team/frontend/component-specs.md` |
| API Specification | `docs/api/README.md` |
| Backend API Docs | `docs/api/openapi.yaml` |
| Git Conventions | `src-link/GITHUB.md` |
| Laravel 13 Docs | [laravel.com/docs/13.x](https://laravel.com/docs/13.x) |
| Tailwind CSS Docs | [tailwindcss.com/docs](https://tailwindcss.com/docs) |
| Alpine.js Docs | [alpinejs.dev](https://alpinejs.dev) |
| Vite Docs | [vitejs.dev](https://vitejs.dev) |

---

## Required Tools

See **[`tools/README.md`](../tools/README.md)** for the full tool matrix, setup order, and activation instructions.

**Your required tools:** Claude Desktop · GitHub (write) · Google Drive · Slack · Figma (viewer) · Sentry

| Tool | Setup Guide | Priority |
|---|---|---|
| Claude Desktop | [`tools/claude-desktop.md`](../tools/claude-desktop.md) | Day 1 |
| Google Drive | [`tools/google-drive.md`](../tools/google-drive.md) | Day 1 |
| GitHub | [`tools/github.md`](../tools/github.md) | Day 1 |
| Slack | [`tools/slack.md`](../tools/slack.md) | Day 1 |
| Figma | [`tools/figma.md`](../tools/figma.md) | Day 1 |
| Sentry | [`tools/sentry.md`](../tools/sentry.md) | Before first staging deploy |

> Connect tools in the order listed. The first four (Claude Desktop, Google Drive, GitHub, Slack) are required on Day 1 for every team member.
