# Day 13 — Source failure banner and approve

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

A failed source keeps saved cards and shows «Источник сейчас недоступен». «Одобрить» sets `approved` and leaves the draft on the page. Nothing is sent outward.

## Work

- When `collect` reports a source unavailable, re-render the page with the exact banner `Источник сейчас недоступен` above the table. Cards that were already stored stay listed.
- Add «Одобрить» on a card that is `draft`. It calls `set_status(id, "approved")`. `draft_text` stays stored and stays visible. Status label becomes `одобрено`.
- Refuse approve when the card is still `new`. The user drafts first.
- Search the HTML and the route list for a send action. There must be no button `Отправить`, no `client.send_message`, no SMTP, and no outbound POST except the model call already isolated in `draft` for `mode=real`. The default page path uses mock and does not perform that call.

Tests on a temporary database and fake sources:

- one source raises, one card was stored earlier, the card remains, and the page body contains `Источник сейчас недоступен`
- draft then approve, `draft_text` unchanged, status `approved`
- the rendered HTML contains no `Отправить`

## Constraints

Do not catch the source error by deleting the database. Do not add a status `sent`. Approving does not call Telegram.

## Done when

The tests above pass. Manual check, if the server is up: a drafted card can be approved and the draft text remains. The specification steps for approve and for an unavailable source are covered.
