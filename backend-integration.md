# Short Backend Integration Test Fix Prompt

```
Fix backend integration tests that fail when connecting to Oracle database. Current issue: Oracle connection errors during test startup.

REQUIREMENTS:
1. Mock Oracle connections for integration tests instead of real database
2. Create test database fixtures with sample data
3. Use in-memory SQLite or test containers for integration testing
4. Add proper test database setup/teardown
5. Mock Arrow/DuckDB operations with test data
6. Ensure tests run without requiring real Oracle instance
7. Add connection retry logic and better error handling

SPECIFIC FIXES NEEDED:
- Replace Oracle connections with mocks in test environment
- Create pytest fixtures for database testing
- Add test data generators for comparison scenarios
- Mock external Oracle dependencies
- Add integration test configuration separate from production

OUTPUT: Working integration tests that don't require real Oracle connections.
```