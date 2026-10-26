# Day 11 — Page list and empty state

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

One HTML page opens in the browser. It shows the title Mention Desk, a «Собрать» button, and a card table. An empty table shows «Пока нет упоминаний».

## Work

Render the page from `app/main.py` at `GET /`. Keep collection rules out of the router: the router calls store and, later, services.

The page shows:

- heading text `Mention Desk`
- a button labeled `Собрать`
- a table of cards with text, date, link, source, and status

When `list_cards()` is empty, the page shows the exact sentence `Пока нет упоминаний`. That empty table is a normal state.

Status labels on the page:

- `new` → `новое`
- `draft` → `черновик`
- `approved` → `одобрено`

Dates that are ISO 8601 may be shown as stored, or in a readable local form, as long as the underlying value remains ISO 8601. Readable dates are polished on Day 14.

The «Собрать» button may submit to a placeholder that does not collect yet. Day 12 makes it collect. If a card is already `draft`, show `draft_text` near the source text. Include a «Черновик» control that posts to the Day 10 handler when a card exists. With an empty database the control is absent and the empty sentence is visible.

Use ordinary HTML. No frontend build, no Docker.

## Constraints

Do not require `TELEGRAM_CHANNEL` or `WEB_URL` to render the empty page. Do not call Telegram or the web source while rendering an empty list. No «Отправить» control anywhere in the HTML.

## Done when

Opening `/` with an empty database shows Mention Desk, «Собрать», and «Пока нет упоминаний». A temporary card inserted in a test or a manual sqlite row appears in the table with a readable status label. Verify the empty page in the browser if a browser tool is available; otherwise fetch `/` with the local server and quote the empty sentence from the body.
