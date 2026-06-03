# Onboarding Guide — QA / QC Engineer

**Team:** Quality Assurance & Quality Control  
**Role Overview:** Ensure that every feature shipped to users is correct, secure, accessible, and matches both the product requirements and the visual design. You are the last line of defense before code reaches production.

---

## 1. Welcome

Quality is not a phase at the end of development — it is a mindset woven into every sprint. As a QA/QC Engineer, you work across the entire delivery cycle: reviewing acceptance criteria before development begins, testing during development, and validating before release. Your findings directly protect its users.

You will write automated tests, conduct manual exploratory testing, manage bug reports, and coordinate User Acceptance Testing (UAT). Claude Desktop accelerates test writing and documentation.

---

## 2. Your Responsibilities

**Test Planning**
- Write test plans for every module, covering functional, boundary, and negative cases
- Derive test cases directly from the acceptance criteria in GitHub Issues and PRDs
- Identify and document edge cases that the development team may have overlooked

**Automated Testing**
- Write and maintain unit and feature tests for backend logic and API endpoints
- Write browser tests for critical user flows (login, [workflow name], [workflow name], and key downloads)
- Integrate tests into the CI/CD pipeline — no code ships if tests fail

**Manual Testing**
- Perform exploratory testing on all completed features before sprint closure
- Conduct visual QA — compare every implemented screen against the Figma mockup
- Test cross-browser compatibility (Chrome, Firefox, Safari, Edge)
- Test mobile web responsiveness

**Bug Management**
- File detailed, reproducible bug reports in GitHub Issues
- Triage incoming bugs by severity and priority
- Verify bug fixes before closing issues
- Track bug metrics per sprint in `by-team/qa/bug-log.md`

**UAT Coordination**
- Prepare the UAT checklist (`by-team/qa/uat-checklist.md`) before each production release
- Facilitate UAT sessions with real users or stakeholders
- Document UAT results and sign off on releases

---

## 3. Tools & Access Setup

### 3.1 Local Development Environment
You need the application running locally to conduct API and browser-level testing.

```bash
git clone [REPO_URL]  # from Project Config Sheet
cd [project-folder]
cp .env.example .env.testing    # separate env for tests
php artisan key:generate --env=testing
docker compose up -d
composer install
npm install
php artisan migrate --seed
php artisan serve
```

### 3.2 Test Environment Variables (`.env.testing`)
```env
APP_ENV=testing
DB_DATABASE=[project-code]_test    # separate database for tests
CACHE_DRIVER=array
SESSION_DRIVER=array
QUEUE_CONNECTION=sync              # run jobs synchronously in tests
MAIL_MAILER=array                  # capture emails, don't send
```

Create the test database:
```bash
php artisan migrate --env=testing --seed
```

### 3.3 Postman
- Import the Postman collection from `docs/api/postman/`
- Set up environment variables for `base_url`, `auth_token`
- Use Postman to test API endpoints manually and document edge cases

### 3.4 Claude Desktop
- Install Claude Desktop and sign in with your QTrust premium account
- Connect your local `[PROJECT_CODE]/` folder as the workspace
- Use Claude to generate test cases from acceptance criteria, write tests, and draft bug reports

### 3.5 Figma (View Access)
- Request view access to all project Figma files (links in `by-team/uix/figma-links.md`)
- You will use Figma as the reference for visual QA

### 3.6 GitHub
- Request access with at least **Write** permissions to create and manage Issues
- Familiarize yourself with labels — you will use `type:bug`, `priority:*`, `module:*`, `status:ready-for-qa`

---

## 4. Testing Levels & Strategy

```
Level 1 — Unit Tests (automated)
  ↓ Test individual classes: Services, Models, helpers
  ↓ No database, no HTTP — pure logic
  ↓ Written by: Backend Engineers + QA
  ↓ Located in: tests/Unit/

Level 2 — Feature / Integration Tests (automated)
  ↓ Test full API request-response cycles
  ↓ Uses the test database (RefreshDatabase)
  ↓ Written by: Backend Engineers + QA
  ↓ Located in: tests/Feature/Api/V1/

Level 3 — Browser / E2E Tests (automated, selective)
  ↓ Test critical UI flows end-to-end in a real browser
  ↓ Written by: QA Engineers
  ↓ Located in: tests/Browser/

Level 4 — Manual Exploratory Testing
  ↓ Unscripted exploration to find unexpected failures
  ↓ Conducted by QA on staging environment
  ↓ Findings become bug reports in GitHub Issues

Level 5 — UAT (User Acceptance Testing)
  ↓ Conducted with real users or business stakeholders
  ↓ Validates that the product meets business expectations
  ↓ Checklist-based, results documented in by-team/qa/
```

---

## 5. Writing Tests

> **Note:** The examples below use **Pest PHP** and **Laravel Dusk** (Laravel testing tools). Substitute with your project's test framework (Jest, Pytest, Playwright, Cypress, etc. — see Project Config Sheet).

### Unit Test Example (Service Logic)
```php
// tests/Unit/Services/PayrollServiceTest.php
use App\Services\PayrollService;

it('calculates overtime at 1.5x for weekday overtime hours', function () {
    $service = new PayrollService();
    $basicDailySalary = 500_000; // IDR 500,000 per day
    $overtimeHours = 3;

    $result = $service->calculateOvertimePay($basicDailySalary, $overtimeHours, isWeekend: false);

    // Formula: (basic_daily / 8) * 1.5 * overtime_hours
    expect($result)->toBe(281_250);
});

it('calculates overtime at 2x for weekend overtime hours', function () {
    $service = new PayrollService();
    $result = (new PayrollService())->calculateOvertimePay(500_000, 2, isWeekend: true);
    expect($result)->toBe(250_000);
});
```

### Feature / API Test Example
```php
// tests/Feature/Api/V1/ExampleTest.php
it('records a submission with the required fields', function () {
    $user = User::factory()->create();

    $response = $this->actingAs($user)
        ->postJson('/api/v1/[endpoint]', [
            'field_one'  => 'value',
            'field_two'  => 'value',
            'attachment' => UploadedFile::fake()->image('example.jpg'),
        ]);

    $response->assertCreated()
        ->assertJsonPath('data.status', 'submitted');

    $this->assertDatabaseHas('[table]', [
        'user_id' => $user->id,
        'status'  => 'submitted',
    ]);
});

it('rejects a duplicate submission', function () {
    $user = User::factory()->create();
    // Create existing record to trigger duplicate logic
    SomeModel::factory()->for($user)->create();

    $this->actingAs($user)
        ->postJson('/api/v1/[endpoint]', [...])
        ->assertUnprocessable()
        ->assertJsonValidationErrors(['field']);
});
```

### Browser / E2E Test Example (Laravel Dusk)
```php
// tests/Browser/WorkflowTest.php
it('a user can complete the [workflow name] flow from the web UI', function () {
    $this->browse(function (Browser $browser) {
        $user = User::factory()->create();

        $browser->loginAs($user)
            ->visit('/[workflow-path]/create')
            ->select('type_id', '1')
            ->type('start_date', '2026-07-01')
            ->type('end_date', '2026-07-03')
            ->type('reason', 'Example reason')
            ->press('Submit')
            ->assertSee('[workflow name] submitted successfully')
            ->assertUrlIs('/[workflow-path]');
    });
});
```

---

## 6. Bug Report Standard

Every bug filed in GitHub Issues must be detailed enough for any engineer to reproduce without asking follow-up questions.

```markdown
**Title:** [Module] Short description of the bug — imperative, specific

**Labels:** type:bug · priority:[critical/high/medium/low] · module:[name] · team:[responsible team]

---

## Bug Description
[Clear, one-paragraph description of what is wrong]

## Environment
- Environment: [local / staging / production]
- Browser: [Chrome 125 / Safari 17 / etc.]
- OS: [macOS 14 / Windows 11 / Android 14]
- App version / commit: [if known]

## Steps to Reproduce
1. Log in as [role]
2. Navigate to [page]
3. [Specific action taken]
4. [Next action]

## Expected Behavior
[What should happen according to the PRD / acceptance criteria / Figma]

## Actual Behavior
[What actually happens]

## Visual Evidence
[Screenshot or screen recording — mandatory for UI bugs]

## Figma Reference
[Link to the Figma screen this should match — for visual bugs]

## Additional Notes
[Stack trace, console errors, network requests — paste if available]
```

### Bug Severity Definitions
| Priority | Definition | SLA (Fix Target) |
|---|---|---|
| `priority:critical` | System crash, data loss, security vulnerability, all users blocked | Same sprint |
| `priority:high` | Core feature broken for most users, no workaround | Current sprint |
| `priority:medium` | Feature works but behavior is wrong; workaround exists | Next sprint |
| `priority:low` | Minor visual issue, cosmetic, edge case | Backlog |

---

## 7. Test Coverage Targets

| Layer | Target | Tool |
|---|---|---|
| Service classes (business logic) | ≥ 85% | Unit tests |
| API endpoints (happy path) | 100% | Feature tests |
| API endpoints (error paths) | Key errors covered | Feature tests |
| Critical UI flows | Covered | Browser / E2E tests |
| Manual regression suite | All modules before release | Manual + checklist |

Run coverage report:
```bash
[test command] --coverage  # exact command varies by framework; minimum 80% recommended
```

> **Note:** `php artisan test --coverage --min=80` is the Laravel/Pest equivalent. Substitute with your project's test runner.

---

## 8. Sprint QA Workflow

```
Before sprint starts:
  ↓ Read sprint issues and their acceptance criteria
  ↓ Write test plans in by-team/qa/test-plan-[module].md
  ↓ Identify ambiguous criteria — raise questions in GitHub Issues

During sprint:
  ↓ Write automated tests alongside feature development (don't wait for completion)
  ↓ Test features as soon as they appear on the staging branch

End of sprint:
  ↓ Execute manual regression test for all completed features
  ↓ Run full automated test suite: [test command] runs successfully
  ↓ Close verified issues; re-open with new bug report if broken
  ↓ Write sprint QA report in reports/qa/sprint-[n]-qa-report.md
```

---

## 9. Working with Claude Desktop

**Generate test cases from acceptance criteria:**
> "Convert these acceptance criteria into a comprehensive [test framework] test suite. Cover happy paths, validation errors, authorization failures, and edge cases. Acceptance criteria: [paste from GitHub Issue]"

**Draft a test plan:**
> "Write a QA test plan for the [Module] module of [application type]. Cover: [list the functional areas relevant to your module]. For each area, list test cases with expected inputs and outputs."

**Write a browser/E2E test:**
> "Write a browser/E2E test for the [workflow name] flow. Steps: [describe the flow]."

**Write a bug report:**
> "Write a professional GitHub bug report for this issue: [describe the problem]. Include steps to reproduce, expected vs actual behavior, and severity assessment."

---

## 10. First Week Checklist

- [ ] Repository cloned, test database created and seeded
- [ ] [test command] runs successfully (all existing tests pass)
- [ ] Postman collection imported from `docs/api/postman/`
- [ ] Figma view access confirmed — all design files accessible
- [ ] Claude Desktop installed and workspace connected
- [ ] All PRDs read and acceptance criteria reviewed
- [ ] `by-team/qa/` folder reviewed — understand existing test assets
- [ ] GitHub write access confirmed — can create Issues and apply labels
- [ ] First test plan written for Sprint 0 features
- [ ] Attended sprint planning

---

## 11. Key Resources

| Resource | Location |
|---|---|
| Test Plans | `by-team/qa/test-plan-[module].md` |
| Bug Log | `by-team/qa/bug-log.md` |
| UAT Checklist | `by-team/qa/uat-checklist.md` |
| Sprint QA Reports | `reports/qa/` |
| API Specification | `docs/api/openapi.yaml` |
| Figma Designs | `by-team/uix/figma-links.md` |
| All PRDs | `docs/product/` |
| Pest PHP Docs | [pestphp.com/docs](https://pestphp.com/docs) *(Laravel projects)* |
| Laravel Dusk Docs | [laravel.com/docs/13.x/dusk](https://laravel.com/docs/13.x/dusk) *(Laravel projects)* |
| Postman Docs | [learning.postman.com](https://learning.postman.com) |

---

## Required Tools

See **[`tools/README.md`](../tools/README.md)** for the full tool matrix, setup order, and activation instructions.

**Your required tools:** Claude Desktop · GitHub (write) · Google Drive · Slack · Figma (viewer) · Sentry · Linear (read)

| Tool | Setup Guide | Priority |
|---|---|---|
| Claude Desktop | [`tools/claude-desktop.md`](../tools/claude-desktop.md) | Day 1 |
| Google Drive | [`tools/google-drive.md`](../tools/google-drive.md) | Day 1 |
| GitHub | [`tools/github.md`](../tools/github.md) | Day 1 |
| Slack | [`tools/slack.md`](../tools/slack.md) | Day 1 |
| Figma | [`tools/figma.md`](../tools/figma.md) | Day 1 |
| Sentry | [`tools/sentry.md`](../tools/sentry.md) | Before first staging deploy |

> Connect tools in the order listed. The first four (Claude Desktop, Google Drive, GitHub, Slack) are required on Day 1 for every team member.
