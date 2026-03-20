## API Testing Resources

Notes and patterns for API test automation. I've worked with API testing across multiple roles — from Postman collections at Sorenson Communications to Playwright's API client in my current stack.

---

## Documentation and Links

- [Postman Docs](https://learning.postman.com/docs/getting-started/introduction/)
- [REST Assured](https://rest-assured.io/)
- [Playwright API Testing](https://playwright.dev/docs/api-testing)
- [HTTPie](https://httpie.io/) — great CLI tool for quick manual API checks
- [JSON Schema](https://json-schema.org/) — for response validation

---

## Tools Overview

| Tool | Language | Best For |
|------|----------|----------|
| Postman | N/A (GUI) | Exploration, collections, manual testing |
| REST Assured | Java | API tests alongside Selenium Java suites |
| Playwright APIRequestContext | Python/TS | API tests alongside UI Playwright suites |
| requests + pytest | Python | Lightweight standalone API test suites |

---

## Postman

Postman is where most API testing starts — great for exploring endpoints before you automate.

Key things to know:

- **Collections** — Group requests into collections and run them as a suite with the Collection Runner.
- **Environments** — Use environment variables (`{{base_url}}`, `{{token}}`) so collections are portable across dev/staging/prod.
- **Pre-request Scripts** — Run JavaScript before a request, e.g., to generate auth tokens.
- **Tests tab** — Write assertions in JavaScript that run after each request.

```javascript
// Postman test example
pm.test("Status is 200", () => {
    pm.response.to.have.status(200);
});

pm.test("Response has user ID", () => {
    const body = pm.response.json();
    pm.expect(body.id).to.be.a("number");
});
```

- **Newman** — Run Postman collections from the command line or CI/CD pipelines.

```bash
newman run collection.json -e environment.json
```

---

## Playwright API Testing (Python)

Playwright's `APIRequestContext` lets you make HTTP calls directly, useful when:
- You need to set up test data via API before a UI test
- You want pure API tests in the same framework as your UI tests

```python
import pytest
from playwright.sync_api import Playwright

@pytest.fixture
def api(playwright: Playwright):
    context = playwright.request.new_context(
        base_url="https://api.example.com",
        extra_http_headers={"Authorization": "Bearer my-token"}
    )
    yield context
    context.dispose()

def test_get_user(api):
    response = api.get("/users/1")
    assert response.status == 200
    body = response.json()
    assert body["name"] == "Dylan"

def test_create_user(api):
    response = api.post("/users", data={"name": "New User", "role": "tester"})
    assert response.status == 201
```

---

## requests + pytest (Lightweight Python)

For standalone API test suites without UI testing, `requests` + `pytest` is minimal and effective.

```python
import requests
import pytest

BASE_URL = "https://api.example.com"

@pytest.fixture(scope="session")
def auth_token():
    response = requests.post(f"{BASE_URL}/auth/login", json={
        "username": "testuser",
        "password": "password"
    })
    return response.json()["token"]

def test_get_users(auth_token):
    headers = {"Authorization": f"Bearer {auth_token}"}
    response = requests.get(f"{BASE_URL}/users", headers=headers)
    assert response.status_code == 200
    assert isinstance(response.json(), list)
```

---

## REST Assured (Java)

Used this alongside Selenium/Java suites. REST Assured uses a fluent given/when/then style that reads cleanly.

```java
import io.restassured.RestAssured;
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

@Test
public void testGetUser() {
    given()
        .header("Authorization", "Bearer " + token)
    .when()
        .get("/users/1")
    .then()
        .statusCode(200)
        .body("id", equalTo(1))
        .body("name", notNullValue());
}
```

---

## Common Patterns

### Authentication

Most APIs use one of these:
- **Bearer token** — set in `Authorization` header
- **API Key** — query param or header (depends on the API)
- **Basic Auth** — base64 encoded `username:password`

Always pull credentials from environment variables, not hardcoded in tests.

```python
import os
TOKEN = os.environ["API_TOKEN"]
```

### Response Validation

Don't just check status codes. Validate:
- Response body structure (required fields present)
- Data types match expectations
- Business logic (e.g., returned user ID matches what you requested)

### Test Data Isolation

API tests can leave dirty state. Either:
- Create and delete test data within each test
- Use a dedicated test environment that gets reset on each run
- Use database seeding scripts in CI before the test run

---

## CI/CD Integration

At Sorenson, I ran Postman collections via Newman in Jenkins pipelines. At Optilogic, API tests run as part of the Pytest suite in GitHub Actions alongside UI tests — same pipeline, same reports.
