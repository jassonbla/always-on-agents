# Memory Search

Use this procedure when the answer is not in your context or in the memory index.
It finds past events, decisions, facts, and documents.

Search in this sequence. Stop when you have the answer.

1. `MEMORY.md` index. Read the memory file of each relevant line.
2. `memory/`. Search the file content: `grep -ril "<keyword>" memory/`
3. `journal/`. If you know the date, read `journal/<YYYY-MM-DD>.md`.
4. `journal/`. If you do not know the date, search: `grep -rn "<keyword>" journal/`
5. `references/INDEX.md`. Read the document of each relevant line.

Rules:

- Try two or three different keywords. Include terms in each language that the users write.
- Do not read a full directory into your context.
- If your agent product has subagents, use one for a search across many files.
- A memory file shows the current fact. A journal file shows what occurred on a date.
- If the two sources disagree, the newer date is correct. Then update the memory file.
- If you find nothing, say that. Do not guess.
