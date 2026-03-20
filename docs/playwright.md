## Playwright Resources

Resources, tips, and patterns for Playwright test automation. My current stack at Optilogic is **Python + Pytest + Playwright**, though I also have experience with **TypeScript** from side projects at TestOut.

---

## Documentation and Links

- [Playwright Docs](https://playwright.dev/docs/intro)
- [Playwright Python Docs](https://playwright.dev/python/docs/intro)
- [Pytest-Playwright Plugin](https://playwright.dev/python/docs/test-runners)
- [Playwright GitHub](https://github.com/microsoft/playwright)
- [Trace Viewer](https://playwright.dev/docs/trace-viewer) — invaluable for debugging failures in CI

---

## Why Playwright Over Selenium?

Playwright solves most of Selenium's reliability problems out of the box:

- **Auto-waiting** — Playwright waits for elements to be actionable before interacting. No more `WebDriverWait` boilerplate everywhere.
- **Network interception** — Built-in request mocking and API testing support.
- **Multi-browser** — Chromium, Firefox, and WebKit from one API.
- **Trace & video** — Full test traces and video recording for post-mortem debugging.
- **Fixtures** — Pytest fixtures make setup/teardown clean and composable.

---

## Setup (Python + Pytest)

```bash
pip install pytest-playwright
playwright install
```

Basic project structure:

```
tests/
├── conftest.py       # shared fixtures
├── pages/            # page objects
│   └── login_page.py
└── test_login.py
```

---

## Page Object Pattern

Playwright works great with the Page Object Model. Pages wrap `page` (the Playwright `Page` object) and expose actions.

```python
# pages/login_page.py
class LoginPage:
    def __init__(self, page):
        self.page = page
        self.username = page.get_by_label("Username")
        self.password = page.get_by_label("Password")
        self.submit = page.get_by_role("button", name="Login")

    def login(self, user, pw):
        self.username.fill(user)
        self.password.fill(pw)
        self.submit.click()
```

```python
# test_login.py
def test_login_success(page):
    login = LoginPage(page)
    page.goto("https://example.com/login")
    login.login("testuser", "password123")
    expect(page).to_have_url("https://example.com/dashboard")
```

---

## Fixtures with conftest.py

Pytest fixtures are how you share state (browser, page, auth) across tests cleanly.

```python
# conftest.py
import pytest
from playwright.sync_api import sync_playwright

@pytest.fixture(scope="session")
def browser_context():
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=True)
        context = browser.new_context()
        yield context
        browser.close()

@pytest.fixture
def page(browser_context):
    page = browser_context.new_page()
    yield page
    page.close()
```

The `pytest-playwright` plugin provides these out of the box — you usually only need custom fixtures for things like pre-authenticated contexts.

---

## Common Operations

**Click:**
```python
page.get_by_role("button", name="Submit").click()
```

**Fill a field:**
```python
page.get_by_label("Email").fill("user@example.com")
```

**Select from dropdown:**
```python
page.get_by_label("Country").select_option("US")
```

**Assert text:**
```python
from playwright.sync_api import expect
expect(page.get_by_role("heading")).to_have_text("Dashboard")
```

**Wait for navigation:**
```python
with page.expect_navigation():
    page.get_by_role("button", name="Login").click()
```

---

## API Testing with Playwright

Playwright has a built-in `APIRequestContext` for making HTTP requests — useful for test setup/teardown or pure API tests alongside UI tests.

```python
def test_api_response(playwright):
    api = playwright.request.new_context(base_url="https://api.example.com")
    response = api.get("/users/1")
    assert response.status == 200
    body = response.json()
    assert body["id"] == 1
```

See the [API Testing](api-testing.md) page for more.

---

## GitHub Actions CI/CD

At Optilogic I built the test pipeline with GitHub Actions. Basic workflow:

```yaml
name: Playwright Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Install browsers
        run: playwright install --with-deps chromium
      - name: Run tests
        run: pytest tests/ --tb=short
```

---

## Debugging Tips

- Use `page.pause()` to drop into Playwright Inspector mid-test during local debugging.
- Run with `--headed` to watch the browser: `pytest --headed`
- Generate a trace on failure and open with `playwright show-trace trace.zip`
- `PWDEBUG=1 pytest` enables inspector mode automatically
