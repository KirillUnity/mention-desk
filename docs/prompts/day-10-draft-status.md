# Day 10 — Draft button writes status and text

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

Acting on a card as «Черновик» runs `draft`, stores the draft text, and sets the status to `draft`.

## Work

- Add a `draft_text` column to `cards`. For this dev database, a simple `ALTER TABLE` on startup is enough. If the local file is a disposable dev database, recreating it is also fine. Do not destroy a caller's data without using the dev path. Tests use a temporary database and should create the column from `init_db` so new databases already have `draft_text`.
- `set_status(id, "draft")` remains the only legal step from `new`. Persist `draft_text` in the same action, either inside `set_status` or in a store function `save_draft(id, text)` that also moves `new → draft`. A second save while already `draft` may refresh `draft_text` and stay on `draft`. It must not move backward.
- In `app/main.py`, add a handler the future button will call: load the card, call `draft` with the resolved mode (`mock` by default), save the text, set status `draft`, and show or return the stored draft. A minimal HTML form or a `POST /cards/{id}/draft` that returns the updated card is enough if the full page is not built yet. Day 11 builds the page layout. This handler must already be real.
- Use mock mode in the default server path so a click does not require a key.

Add a test on a temporary database: insert a card, invoke the draft action, assert `status == "draft"` and `draft_text` contains `учебный черновик` when mode is mock.

## Constraints

No «Отправить» button, no Telegram write, no email. Approving is Day 13. Do not skip `new → draft` straight to `approved`.

## Done when

The test shows the status change and the saved draft text. Restarting the app against the dev database does not crash on the new column.
