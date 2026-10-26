# Day 20 — Polish from the manual run

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` and `docs/day-19-run.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

The gaps recorded on Day 19 are fixed. Steps 1–5 pass with no manual workaround.

## Work

Fix only the mismatches listed in `docs/day-19-run.md`. Expected themes, if they were flagged:

- user-visible strings match the specification exactly: `Пока нет упоминаний`, `Укажите канал и адрес страницы`, `Источник сейчас недоступен`, `учебный черновик`, button labels `Собрать`, `Черновик`, `Одобрить`
- stored `published_at` is ISO 8601 or empty
- web `external_id` is the canonical URL
- a repeated collect does not add a row
- approve leaves the draft on the page

After the fixes, walk steps 1–5 again. Update `docs/day-19-run.md` with a short Day 20 section: which steps now match. Keep secrets out of that file.

Add or adjust a test when a fix was a contract bug, for example canonical URL or ISO date parsing. Keep new tests on fixtures and `MockTransport`.

## Constraints

No new sources, no send button, no VK, no Playwright. Do not broaden the page into a multi-page app. If Day 19 said a step was blocked on missing user credentials, do not fake a green manual run. Say it is still blocked.

## Done when

Every step Day 19 could execute now matches the specification without a workaround. `pytest` is still green offline. The Day 20 note records the second walk.
