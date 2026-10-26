# Day 19 — Manual run against the specification

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before the run. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement product fixes yet unless the server crashes before any step can be seen. Leave `docs/prompts/` unchanged. Day 20 applies the fix list.

## Outcome

The six-step check has been attempted on a real test channel and a simple page. The gap between the specification and the program is written down.

## Work

Use the local `.env` that the user already created. If it is missing, stop after preparing a checklist and say which values are needed: `TELEGRAM_API_ID`, `TELEGRAM_API_HASH`, `TELEGRAM_CHANNEL`, `WEB_URL`. Do not invent them. Create the Telethon session locally if the README says how and the user has credentials. Never commit the session.

Run and record:

1. Open the page with an empty store, or a fresh database file. Expect «Пока нет упоминаний».
2. Clear channel and page URL temporarily, press «Собрать». Expect `Укажите канал и адрес страницы` and no new rows. Restore the settings afterward.
3. Press «Собрать» with the test channel and the simple page. Expect cards in `новое`.
4. Press «Собрать» again. Expect the same count.
5. Draft one card and approve it. Expect `одобрено`, the draft still on the page, and no outgoing Telegram post. Confirm no send button exists.
6. Run `pytest` and confirm it stays offline while the manual run may use the network.

Write `docs/day-19-run.md` in Russian, for a reader, not as a log of class names. For each step: совпадает, нет, or не доделано, plus what you saw. List concrete fixes for Day 20: message text, ISO dates, canonical URL, labels, banner. If a step matches, say so in one line.

## Constraints

Read-only on Telegram. Do not post, do not email, do not use a foreign account. Do not put secrets into `docs/day-19-run.md`. If login needs an interactive code, stop and ask the user. Do not bypass that prompt.

## Done when

`docs/day-19-run.md` exists and a reader can see the delta for all six steps. Day 20 can fix from that file alone.
