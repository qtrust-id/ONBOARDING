# Onboarding Guide — Backend Engineer

**Team:** Backend Engineering  
**Role Overview:** Design, build, and maintain the server-side application — the data models, business logic, API endpoints, background jobs, and integrations that power the entire application.

---

## 1. Welcome

You are responsible for the core of the application: everything that persists data, enforces business rules, and serves information to the frontend and mobile clients. The quality of your data models, the correctness of your business logic, and the security of your API determine the overall reliability of the product.

Your tech stack is defined in the Project Configuration Sheet. The examples in this guide use **Laravel 13 / PHP 8.3 / PostgreSQL / Redis / GCP Cloud Run** as a reference implementation — substitute with your project's actual stack. Claude Desktop is your pair programmer — use it aggressively to accelerate boilerplate-heavy tasks like model generation, resource classes, and test writing.

---

## 2. Your Responsibilities

- Design database schema (migrations, indexes, constraints) in collaboration with Product Development
- Implement Eloquent models with relationships, casts, and scopes
- Build REST API endpoints: Form Requests, Controllers, API Resources, route definitions
- Implement authorization using Laravel Policies and Gates
- Write service classes for complex business logic (rules specific to your project domain)
- Create background jobs for asynchronous operations (bulk processing, exports, email notifications)
- Integrate third-party services (payment gateways, external APIs, cloud storage)
- Write unit and feature tests using Pest PHP
- Review Frontend and Mobile API integration issues and resolve them promptly
- Document all API changes in `docs/api/openapi.yaml`

---

## 3. Tech Stack

| Technology | Version | Purpose |
|---|---|---|
| Laravel | 13.x | PHP application framework |
| PHP | 8.3 | Runtime — use named arguments, readonly properties, enums |
| PostgreSQL | 16 | Primary relational database |
| Redis | 7 | Cache, session, queue driver |
| Laravel Sanctum | 4.x | API token authentication |
| Laravel Horizon | 5.x | Queue monitoring dashboard |
| Laravel Telescope | 5.x | Local debug/inspection tool |
| Pest PHP | 3.x | Test framework (preferred over PHPUnit directly) |
| Spatie Packages | various | Permissions (spatie/laravel-permission), Media, Activity Log |

> **Note:** The table above reflects the reference Laravel stack. Check your Project Configuration Sheet for the actual technologies used in your project.

---

## 4. Local Development Environment Setup

### 4.1 Prerequisites

```bash
# macOS — example for a PHP/Laravel project; adapt for your project's runtime
brew install php@8.3 composer
brew install --cask docker

# Verify
php --version       # must be 8.3.x
composer --version
docker --version
```

> **Note:** The commands above are for a PHP/Laravel stack. If your project uses a different runtime (Node.js, Python, Go, etc.), substitute the appropriate package manager and version check commands.

### 4.2 Clone & Configure
```bash
git clone [REPO_URL]  # URL from Project Config Sheet
cd [project-directory]

cp .env.example .env
# Laravel: php artisan key:generate
# Other frameworks: use the equivalent key/secret generation command for your stack
```

Edit `.env` for local backend development:
```env
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=[project-code]_dev
DB_USERNAME=[project-code]_user
DB_PASSWORD=secret

REDIS_HOST=127.0.0.1
REDIS_PORT=6379

CACHE_DRIVER=redis
SESSION_DRIVER=redis
QUEUE_CONNECTION=redis

SANCTUM_STATEFUL_DOMAINS=localhost,localhost:3000

# Leave blank locally — use GCP Secret Manager in production
FILESYSTEM_DISK=local
```

### 4.3 Start Services
```bash
# Start PostgreSQL and Redis via Docker
docker compose up -d

# Verify
docker compose ps
```

### 4.4 Install & Initialize
```bash
# Laravel example — substitute with your project's package manager and CLI commands
composer install
php artisan migrate --seed
php artisan horizon        # in a separate terminal (queue worker)
php artisan serve
```

> **Note:** `composer install`, `php artisan migrate`, and `php artisan serve` are Laravel-specific commands. Substitute the equivalent commands for your project's framework (e.g., `npm install` / `python manage.py migrate` / `go run .`).

### 4.5 Local Debugging Tool
```bash
# Laravel example: Telescope accessible at http://localhost:8000/telescope
# Refer to your project stack for the equivalent debugging/inspection tool
# Never deploy local debugging tools to production
```

---

## 5. Application Architecture

### Directory Structure
```
app/
├── Console/
│   └── Commands/           ← CLI commands (batch jobs, imports)
├── Exceptions/
│   └── Handler.php
├── Http/
│   ├── Controllers/
│   │   ├── Api/
│   │   │   └── V1/         ← All REST API controllers
│   │   └── Web/            ← Web-only controllers (if any)
│   ├── Middleware/
│   ├── Requests/           ← Form Request validation classes
│   └── Resources/          ← API Resource (JSON response transformers)
├── Models/                 ← ORM models
├── Policies/               ← Authorization policies (one per model)
├── Services/               ← Business logic layer
│   └── ...
├── Jobs/                   ← Queued jobs
├── Notifications/          ← Email/push notifications
├── Events/                 ← Application events
└── Listeners/              ← Event listeners
database/
├── migrations/             ← One file per table change, chronological
├── seeders/                ← DatabaseSeeder + module seeders
└── factories/              ← Model factories for testing
routes/
├── api.php                 ← All API routes (prefix: /api/v1)
└── web.php                 ← Web routes (dashboard pages)
tests/
├── Unit/                   ← Pure unit tests (no database)
│   ├── Services/
│   └── Models/
└── Feature/                ← Integration/feature tests (uses database)
    └── Api/
        └── V1/
```

### Service Layer Pattern
Never put complex business logic in controllers. Controllers orchestrate; Services calculate and decide.

```php
// Controller — thin, delegates to service
class LeaveController extends Controller
{
    public function store(StoreLeaveRequest $request, LeaveService $service): JsonResponse
    {
        $leave = $service->apply(auth()->user(), $request->validated());
        return new LeaveResource($leave);
    }
}

// Service — fat, contains all the rules
class LeaveService
{
    public function apply(User $employee, array $data): Leave
    {
        // Check balance, validate overlap, calculate working days,
        // create record, trigger notification, deduct balance
    }
}
```

---

## 6. Database & Migration Standards

### Migration Naming
```
YYYY_MM_DD_HHMMSS_create_[table]_table.php
YYYY_MM_DD_HHMMSS_add_[column]_to_[table]_table.php
YYYY_MM_DD_HHMMSS_create_[table]_[related_table]_table.php  ← pivot
```

### Column Conventions
- Primary key: `id` (ULID recommended for distributed safety — use `$table->ulid('id')`)
- Foreign keys: `[model]_id` (e.g., `employee_id`, `department_id`)
- Timestamps: always include `$table->timestamps()` and `$table->softDeletes()` on all main entities
- Booleans: prefix with `is_` or `has_` (e.g., `is_active`, `has_overtime`)
- Enums: use PHP-backed enums + cast in model

### Index Strategy
Add indexes on:
- All foreign key columns
- Any column used in `WHERE` clauses in API filters
- Composite indexes for frequently combined filters

---

## 7. API Design Standards

All API routes are versioned under `/api/v1/`. Use resource routing.

### Route Conventions
```php
// routes/api.php
Route::prefix('v1')->middleware('auth:sanctum')->group(function () {
    Route::apiResource('employees', EmployeeController::class);
    Route::apiResource('leaves', LeaveController::class);
    Route::post('leaves/{leave}/approve', [LeaveController::class, 'approve']);
    Route::post('leaves/{leave}/reject', [LeaveController::class, 'reject']);
});
```

### Response Format — always consistent
```json
// Success (200/201)
{
  "data": { ... },
  "message": "Request submitted successfully."
}

// Paginated list
{
  "data": [ ... ],
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 124
  }
}

// Validation error (422)
{
  "message": "The given data was invalid.",
  "errors": {
    "start_date": ["The start date must be a future date."]
  }
}

// Not found (404)
{
  "message": "Resource not found."
}
```

### Form Request Validation
Every API endpoint that accepts input must have a dedicated Form Request class. Never validate in the controller method.

---

## 8. Testing Standards

Write tests with Pest PHP. Target coverage: **≥ 80% for service classes**, **100% for all API endpoints** (happy path + common error paths).

```php
// tests/Feature/Api/V1/LeaveTest.php
it('allows an employee to submit a leave request', function () {
    $employee = User::factory()->employee()->create();
    $leaveType = LeaveType::factory()->create();

    $response = $this->actingAs($employee)
        ->postJson('/api/v1/leaves', [
            'leave_type_id' => $leaveType->id,
            'start_date' => now()->addDays(3)->toDateString(),
            'end_date' => now()->addDays(5)->toDateString(),
            'reason' => 'Family event',
        ]);

    $response->assertCreated()
        ->assertJsonPath('data.status', 'pending');
});

it('rejects a leave request when the employee has insufficient balance', function () {
    $employee = User::factory()->employee()->withLeaveBalance(0)->create();

    $this->actingAs($employee)
        ->postJson('/api/v1/leaves', [...])
        ->assertUnprocessable()
        ->assertJsonValidationErrors(['balance']);
});
```

### Testing Rules
- Use `RefreshDatabase` trait — every test runs in a transaction that is rolled back
- Use Factories for all test data — never hardcode IDs
- Mock external HTTP calls with `Http::fake()` — never call real third-party APIs in tests
- Queue jobs with `Queue::fake()` — assert they were dispatched, not that they ran

---

## 9. Security Requirements

- All routes under `/api/v1/` must be protected with appropriate authentication middleware
- Every controller action must enforce authorization (using Policies, Guards, or equivalent)
- Never trust user input — always validate at the boundary
- Never expose sensitive fields (passwords, tokens) in API responses
- Log all destructive actions (delete, bulk update)
- Rate-limit authentication endpoints

---

## 10. Working with Claude Desktop

**Generate a full model + migration + factory + seeder:**
> "Generate a [FRAMEWORK] model for a [Entity] table. Fields: [list all fields with types and constraints]. Include: migration, factory, and database seeder."

**Generate a REST API endpoint:**
> "Create a [FRAMEWORK] controller, request validator, and response serializer for [Action]. Endpoint: [METHOD] [/path]. Validation rules: [list rules]. Return [ResponseObject] on success."

**Generate tests:**
> "Write [test framework] tests for the [endpoint]. Cover: [list scenarios — e.g., successful case (201), unauthenticated (401), validation failures (422), not found (404)]."

**Design a service class:**
> "Design a [ServiceName] class for [use case]. Business rules: [list rules relevant to your domain]."

---

## 11. Git Workflow

```bash
git checkout develop
git pull origin develop
git checkout -b feature/[module]-[description]

# Examples:
# feature/employee-model-and-api
# feature/leave-approval-service
# fix/[module]-[description]
```

All PRs target `develop`. Require 1 reviewer. CI (tests) must pass.

---

## 12. First Week Checklist

- [ ] Repository cloned and `.env` configured
- [ ] Docker services running, database migrated and seeded
- [ ] Development server running, app accessible at `http://localhost:8000`
- [ ] Debugging/inspection tool accessible (Telescope, Django Debug Toolbar, etc. — per project stack) and showing requests
- [ ] Claude Desktop installed, workspace folder connected
- [ ] All existing PRDs read — understand data requirements
- [ ] `docs/api/README.md` and `docs/api/openapi.yaml` reviewed
- [ ] `by-team/backend/README.md` read
- [ ] First GitHub Issue assigned — start with a model + migration task
- [ ] Attended sprint planning for Sprint 0

---

## 13. Key Resources

| Resource | Location |
|---|---|
| Project Configuration Sheet | Provided by your team lead |
| API Specification | `docs/api/openapi.yaml` |
| DB Schema Drafts | `by-team/backend/db-schema-drafts/` |
| All PRDs | `docs/product/` |
| Mobile API Contracts | `by-team/mobile/api-contracts.md` |
| Git Conventions | `src-link/GITHUB.md` |
| Laravel 13 Docs | [laravel.com/docs/13.x](https://laravel.com/docs/13.x) *(applies to a Laravel-based stack)* |
| Pest PHP Docs | [pestphp.com/docs](https://pestphp.com/docs) *(applies to a Laravel-based stack)* |
| Spatie Packages | [spatie.be/open-source](https://spatie.be/open-source) *(applies to a Laravel-based stack)* |
| PHP 8.3 New Features | [php.net/releases/8.3](https://www.php.net/releases/8.3/) *(applies to a Laravel-based stack)* |

---

## Required Tools

See **[`tools/README.md`](../tools/README.md)** for the full tool matrix, setup order, and activation instructions.

**Your required tools:** Claude Desktop · GitHub (write) · Google Drive · Slack · Sentry · Datadog

| Tool | Setup Guide | Priority |
|---|---|---|
| Claude Desktop | [`tools/claude-desktop.md`](../tools/claude-desktop.md) | Day 1 |
| Google Drive | [`tools/google-drive.md`](../tools/google-drive.md) | Day 1 |
| GitHub | [`tools/github.md`](../tools/github.md) | Day 1 |
| Slack | [`tools/slack.md`](../tools/slack.md) | Day 1 |
| Sentry | [`tools/sentry.md`](../tools/sentry.md) | Before first staging deploy |
| Datadog | [`tools/datadog.md`](../tools/datadog.md) | Before infra provisioning |

> Connect tools in the order listed. The first four (Claude Desktop, Google Drive, GitHub, Slack) are required on Day 1 for every team member.
