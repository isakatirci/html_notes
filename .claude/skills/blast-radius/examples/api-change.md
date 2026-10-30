# Example: Renaming `getUserById()`

You: "I want to rename the `getUserById()` function to `fetchUser()`"

Blast Radius:
- Direct callers in UserService: 23 references (file:line provided)
- REST API endpoint `/users/:id` depends on this
- Mobile app v1.0-1.2 makes calls (3 months to update)
- Background job `UserNotifier` calls this at startup
- Tests: 8 tests verify behavior, all will break with rename

Recommendation: Add new `fetchUser()`, deprecate `getUserById()`,
run both for 2 releases, update tests incrementally.
