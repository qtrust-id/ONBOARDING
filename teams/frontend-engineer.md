# Onboarding Guide — Frontend Engineer

**Team:** Frontend Engineering  
**Role in HRIS:** Build the web user interface that HR administrators, managers, and employees interact with every day, faithfully translating Figma designs into polished, performant Blade templates.

---

## 1. Welcome

You are responsible for everything the user sees and touches in the HRIS web application. You work at the boundary between design and logic — taking Figma mockups from the UIX Designer and wiring them to the REST API from the Backend Engineer. Your output must be pixel-accurate, accessible, and fast.

On this project, you work primarily with **Laravel Blade**, **Vite**, **Tailwind CSS**, and **Alpine.js**. Claude Desktop is your pair programmer and code generator.

---

## 2. Your Responsibilities

- Implement all Blade views and layouts from approved Figma designs
- Build reusable Blade components used across the application
- Set up and maintain the Vite asset pipeline (CSS, JS, fonts, images)
- Integrate frontend views with backend API endpoints
- Ensure UI is responsive, accessible (WCAG 2.1 AA), and performant
- Write and maintain frontend-related tests (browser-level via Laravel Dusk when relevant)
- Participate in design reviews to flag implementation constraints early
- Conduct visual QA — compare your implementation against the Figma source

---

## 3. Tech Stack

| Technology | Version | Purpose |
|---|---|---|
| Laravel Blade | 13.x | Server-side templating engine |
| PHP | 8.3 | Server runtime |
| Vite | 6.x | Asset bundler (replaces webpack/mix) |
| Tailwind CSS | 4.x | Utility-first CSS framework |
| Alpine.js | 3.x | Lightweight reactive JS for UI interactions |
| Axios | 1.x | HTTP client for AJAX/API calls |
| Chart.js | 4.x | Data visualization (dashboards, reports) |
| Laravel Dusk | 8.x | Browser-based end-to-end tests |

---

## 4. Local Development Environment Setup

### 4.1 Prerequisites
Ensure the following are installed on your machine before cloning the repository:

```bash
# macOS (using Homebrew)
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
git clone https://github.com/qtrust/hris-app.git
cd hris-app
```

### 4.3 Environment Configuration
```bash
cp .env.example .env
php artisan key:generate
```

Edit `.env` with your local database credentials:
```env
APP_NAME="HRIS QTrust"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=hris_dev
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
# Terminal 1 — Laravel development server
php artisan serve

# Terminal 2 — Vite asset watcher (hot reload)
npm run dev
```

Visit `http://localhost:8000` — you should see the HRIS login page.

### 4.7 Recommended VS Code Extensions
Install these for the best developer experience:

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

```
resources/
├── views/
│   ├── layouts/
│   │   ├── app.blade.php          ← Main authenticated layout
│   │   ├── auth.blade.php         ← Unauthenticated layout (login, etc.)
│   │   └── print.blade.php        ← Print layout (payslips, reports)
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
│   ├── employees/
│   │   ├── index.blade.php
│   │   ├── create.blade.php
│   │   ├── edit.blade.php
│   │   └── show.blade.php
│   ├── attendance/
│   ├── leaves/
│   ├── payroll/
│   ├── performance/
│   └── recruitment/
├── css/
│   └── app.css                    ← Tailwind + custom styles
└── js/
    ├── app.js                     ← Main JS entry point
    └── bootstrap.js               ← Axios, Echo setup
public/
└── build/                         ← Vite compiled output (do not edit)
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
- Extract repeated utility combinations into Blade components — not `@apply`
- Never use `!important` unless absolutely unavoidable

### Alpine.js
- Keep `x-data` objects small and focused — one concern per component
- Use `$store` for shared state across components on the same page
- Prefer Alpine.js `x-show` over `x-if` for toggling elements that are frequently shown/hidden (avoids DOM removal cost)

### JavaScript
- Use ES2020+ syntax (the project targets modern browsers)
- No jQuery — Alpine.js and Axios handle all interactivity and HTTP
- All Axios calls go through a central `api.js` module — do not call endpoints inline in templates

### File Naming
- Blade views: `kebab-case.blade.php`
- Blade components: `kebab-case.blade.php` (accessed as `<x-kebab-case>`)
- JS modules: `camelCase.js`
- CSS custom classes: `kebab-case` with a `hris-` prefix for custom utilities

---

## 7. Git Workflow

### Branching
Always branch from `develop`, never from `main`.

```bash
git checkout develop
git pull origin develop
git checkout -b feature/[module]-[short-description]

# Examples:
# feature/attendance-check-in-view
# feature/employee-table-component
# fix/leave-form-date-validation
```

### Committing
Use conventional commits:
```
feat(attendance): add check-in form with camera preview
fix(employee): correct pagination on search results
style(dashboard): align summary cards to design spec
refactor(components): extract reusable badge component
test(leaves): add Dusk test for leave submission flow
```

### Pull Requests
- Target: `develop` branch (never `main`)
- Title must reference the GitHub Issue: `feat(payroll): payslip PDF export [#42]`
- Include screenshots comparing your implementation to the Figma mockup
- Request review from at least one other Frontend Engineer or the Tech Lead
- CI must pass before merging

---

## 8. Reading Figma Designs

Before implementing any screen:
1. Open the corresponding Figma file (links in `by-team/uix/figma-links.md`)
2. Switch to **Dev Mode** in Figma to see exact dimensions, spacing values, and CSS suggestions
3. Read the handoff notes in `by-team/uix/handoff-notes/[module].md`
4. Check all screen states: default, loading, empty, error, hover, disabled

You can also ask Claude to read the Figma design directly:
> "Read the Figma frame at [URL] and generate a Blade component with Tailwind CSS that matches it exactly."

---

## 9. Working with Claude Desktop

**Generate a Blade component from a Figma design:**
> "Read the Figma design at [URL] and generate a reusable Blade component for the Employee Table. It should support pagination, a search input, and a status badge column. Use Tailwind CSS and Alpine.js for the search filter."

**Generate a full page view:**
> "Create a Blade view for the Leave Management index page. It should have: a summary bar showing remaining leave days, a table of leave history with status badges, and a 'New Leave Request' button that opens a modal. Use the app.blade.php layout and Tailwind CSS."

**Implement an API integration:**
> "Write an Alpine.js component that fetches attendance data from `/api/v1/attendance/history` using Axios, displays a loading skeleton while fetching, and renders the data in a table. Handle API errors with a dismissible alert."

**Write a Dusk test:**
> "Write a Laravel Dusk test that: logs in as an HR Admin, navigates to the Employee list, searches for 'John', and asserts that the results are filtered correctly."

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
- [ ] Login page visible at `http://localhost:8000`
- [ ] VS Code extensions installed
- [ ] Claude Desktop installed, workspace folder connected
- [ ] Figma view access granted — all design files reviewed
- [ ] `by-team/frontend/component-specs.md` read
- [ ] `designs/design-system/README.md` read — understand all tokens
- [ ] First GitHub Issue assigned and branch created
- [ ] Attended sprint planning for Sprint 0

---

## 12. Key Resources

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
