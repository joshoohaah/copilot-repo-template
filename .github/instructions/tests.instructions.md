---
applyTo: "**/*.test.*,**/*.spec.*,**/test_*.py,**/*_test.py,**/*_test.go,tests/**,test/**"
---

# Test files

- Name tests after the behavior: `returns empty list when no items match`.
- Arrange / Act / Assert, one behavior per test.
- A test must be able to fail: assert on real output, not on mocks you just set up.
- Bug fixes start with a test that reproduces the bug and fails before the fix.
- No sleeps or real network calls; use fakes and fixed clocks.
- Never delete, skip, or loosen a failing test to get `scripts/verify.sh` green.
