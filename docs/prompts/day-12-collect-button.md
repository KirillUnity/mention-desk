# Day 12 — Collect button and settings check

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

«Собрать» either asks for settings or runs `collect` and refreshes the page. Manual steps 1 and 2 of the specification pass.

## Work

Add `POST /collect` from the «Собрать» button.

- If `TELEGRAM_CHANNEL` or `WEB_URL` is missing or blank, insert nothing and show the exact sentence `Укажите канал и адрес страницы`. Leave existing cards untouched. The empty-state sentence remains when the table is still empty.
- If both are set, call `collect` with the telegram and web sources. Build the Telethon client only in this runtime path, through the Day 6 factory. Pass an `httpx.AsyncClient` into the web source from outside `fetch_page`.
- After a successful collect, redirect or re-render `/` so the table shows the new cards with status `новое`.
- A second collect of the same items does not add rows. That behavior already lives in `collect`. Do not reimplement dedup in the router.

Settings come from the environment. Document in a code comment near the factory that a local `.env` is loaded only if you already have a loader; otherwise read `os.environ`. Do not commit `.env`.

For tests, call `POST /collect` with the environment cleared and assert the settings sentence and a zero insert count. A second test may pass fake sources into the route if the route can accept them. Do not require a live channel for `pytest`.

## Constraints

Read-only Telegram. No send, no email, no VK, no Playwright. If the real Telethon client cannot be constructed because credentials are absent, treat that the same as missing channel settings and show `Укажите канал и адрес страницы` rather than crashing. Do not invent credentials.

## Done when

With empty settings, the page still shows «Пока нет упоминаний» after «Собрать», plus `Укажите канал и адрес страницы`. With both settings present, the route calls `collect`. `pytest` stays offline. Manually confirm steps 1 and 2 in the browser when the server can start.
