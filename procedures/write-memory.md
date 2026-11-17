# Write Memory

Use this procedure before you create, change, or delete a file in `memory/`.

The file name is `memory/<topic-in-kebab-case>.md`. Use this format:

```markdown
---
name: <topic-in-kebab-case>
description: <one line that tells when this topic is relevant>
updated: <YYYY-MM-DD>
---

<The facts on this topic. Add the reason if the user gave the reason.>
```

Procedure:

1. Search for a file on the same topic: `grep -ril "<keyword>" memory/`
2. If a file exists, update it. Set the `updated` field to the date of the new fact.
3. If no file exists, create one with the format above.
4. Add or update the index line in `MEMORY.md`: `- [<name>](memory/<name>.md) - <description>`
5. If you delete a memory file, remove its index line.

Rules:

- Keep one topic in each file. A file can contain two or more related facts on that topic.
- Write only durable facts. A durable fact stays true for one month or more.
- Write only facts that the user or a source document stated.
- Do not add facts that you derived. Do not add instructions to yourself.
- Change the file when a fact changes. Delete the file when all its facts are wrong.
- Write absolute dates. Do not write "today" or "last week".
- Keep each file at 60 lines or less.
- `MEMORY.md` contains only index lines. Do not write facts into `MEMORY.md`.
- When `MEMORY.md` is near 150 lines, merge files with related topics or delete obsolete files.
