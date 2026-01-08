PART 3: API and UI Integration Testing

Purpose

The goal of this test is to verify that a project created through the
backend API is correctly visible across different platforms while
maintaining tenant-level security.

This type of testing helps ensure that the backend, frontend, and mobile
views are working together as expected.

Test Scenario

The following flow would be validated:

1. Create a project using the API
2. Verify the project appears in the web application
3. Check that the project is accessible on mobile
4. Ensure the project is not visible to users from other tenants

Example Test Flow

```python
def test_project_creation_flow():
    # Step 1: Create project via API
    project_id = create_project_api(
        tenant="company1",
        name="Test Project"
    )

    # Step 2: Verify project in web UI
    open_dashboard(tenant="company1")
    assert project_is_visible("Test Project")

    # Step 3: Verify project on mobile
    open_mobile_dashboard(tenant="company1")
    assert project_is_visible("Test Project")

    # Step 4: Verify tenant isolation
    open_dashboard(tenant="company2")
    assert not project_is_visible("Test Project")
