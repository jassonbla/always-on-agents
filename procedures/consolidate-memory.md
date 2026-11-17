# Consolidate Memory

Use this procedure at session start when older journal files have no `## Consolidated` line.
Do it one time for each daily restart.

1. Find the journal files to consolidate: `grep -L "^## Consolidated" journal/*.md`
2. Remove the journal file of today from the list. Read each file that stays in the list.
3. Apply this test to each entry: "Is this true and useful one month from now?"
4. If the answer is no, do nothing. The entry stays in the journal.
5. If the answer is yes, search `memory/` for a file on the same topic.
6. Update that file, or create a new file. Obey `procedures/write-memory.md`.
7. Set the `updated` field to the date of the journal entry.
8. Add or update the index line in `MEMORY.md`.
9. Delete memory files that the new facts make wrong. Remove their index lines.
10. Add a `## Consolidated` heading at the end of each journal file that you read.
11. Below the heading, write the current date and the names of the memory files that you changed.
12. Do the verification below. Correct each problem.
13. Tell the user the number of facts that you added, changed, and deleted.

Verification:

- `wc -l MEMORY.md memory/*.md` shows no file above its limit in `AGENTS.md`.
- Each file in `memory/` has one index line in `MEMORY.md`.
- Each index line in `MEMORY.md` points to a file that exists.
- `MEMORY.md` contains only the heading, the description line, and index lines.

Rules:

- Do not change the old entries of a journal file. The `## Consolidated` section is the only addition.
- Do not copy a full journal entry. Write only the durable fact.
- Preferences, decisions, people, and system facts are usually durable.
- Task progress, one-time requests, and conversation details are usually not durable.
