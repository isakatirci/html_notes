# Example: Rename user table columns (`old_name` → `new_name`)

Change: Rename user table columns (old_name → new_name)

Phase 1 - Expand (1 release):
- Add new_name column, backfill from old_name
- Write to both old_name and new_name
- Read from new_name (fallback to old_name)
- Schema: Both columns present

Phase 2 - Migrate (2-3 releases):
- Update all readers to use new_name
- Stop writing to old_name
- Monitor old_name queries (should drop)
- Schema: Both columns, old_name unused

Phase 3 - Contract (1 release):
- Verify no reads of old_name (monitoring)
- Drop old_name column
- Update schema
- Schema: Only new_name

Safety: Each phase deployable independently. Rollback: Drop
new column, revert writes. Mobile lag: Supported via fallback reads.
