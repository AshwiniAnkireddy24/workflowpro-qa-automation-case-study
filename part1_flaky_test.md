PART 1: Flaky Test Analysis and Stabilization

The given Playwright login test shows intermittent failures,
especially when executed in CI/CD pipelines.

Issues Identified:

1. The test does not wait for page navigation after clicking login.
2. Dashboard elements are loaded dynamically, but no waits are applied.
3. URL assertion is executed immediately after login.
4. Visibility checks are done without ensuring the element is ready.
5. CI environments are slower than local machines.
6. Different browsers and screen sizes increase instability.
7. Login flow may include 2FA for some users.
8. Multi-tenant data loads at different speeds.

Why This Fails in CI/CD:

In local environments, pages usually load faster and timing issues
are hidden. In CI/CD, tests run in headless mode with limited resources,
which exposes race conditions caused by missing waits and synchronization.

Improved Stable Test Example:

```python
def test_user_login_stable(page):
    page.goto("https://app.workflowpro.com/login")

    page.fill("#email", "admin@company1.com")
    page.fill("#password", "password123")
    page.click("#login-btn")

    page.wait_for_url("**/dashboard", timeout=10000)

    welcome_message = page.locator(".welcome-message")
    welcome_message.wait_for(state="visible")

    assert welcome_message.is_visible()
