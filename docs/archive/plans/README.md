# Archived Plans

Completed implementation plans.

---

## What Goes Here

Plans are moved here after ALL of the following are true:

1. ✅ All implementation steps marked complete
2. ✅ All tests passing
3. ✅ "Key Discoveries" section filled in
4. ✅ "Future Improvements" section has minimum 2 items
5. ✅ Task summary added to `docs/DONE.md`

---

## File Naming

Keep original filename: `YYYY-MM-DD_task-name.md`

---

## Archive Process

```bash
# Move completed plan from active to archive
mv docs/plans/YYYY-MM-DD_task-name.md docs/archive/plans/

# Also delete /root/.claude/plans/ copy if exists
rm /root/.claude/plans/YYYY-MM-DD_task-name.md
```

---

## Finding Plans

To find a specific archived plan:
- By date: Look for files starting with date
- By feature: Use `grep -r "keyword" docs/archive/plans/`

---

*Archived plans serve as historical reference for decisions and patterns.*
