# Day 7 — Telegram fixture and combined dedup

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

Both sources parse fixtures, and dedup is proven for a combined collect. `pytest` stays offline.

## Work

- Add `tests/fixtures/telegram_post.json`, one saved post with id, text, date, and the fields `fetch_channel` needs.
- Add a fake client that returns that fixture and nothing else. It must not be a Telethon client and must not open a session.
- Test the mapped card: `source == "telegram"`, `external_id` equals the message id as text, `text` is the post body after normalize, `url` matches the rule from Day 6, `published_at` is ISO 8601, `status == "new"`.
- Test dedup: two collects of the same fake post leave one row.
- Add one combined test: `collect` with the telegram fake and the Day 5 web `MockTransport` inserts two cards. Collect again. The row count does not grow.
- Keep web and telegram tests in `tests/test_collect.py` unless the file is already split, in which case the combined test still lives on the collect path.

## Constraints

No live Telegram, no live HTTP, no `.session` file, no API hash in the fixture. The fixture is a post the test invented or saved earlier, not a credential.

## Done when

`pytest` is green for web and telegram. The combined test shows two cards, then the same two after a second collect. This is the Day 7 checkpoint.
