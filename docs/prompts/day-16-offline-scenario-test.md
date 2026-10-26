# Day 16 — Full offline scenario test

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

One integration test replays specification check 6 without the network: fixtures in, one telegram card and one web card, mock draft, approve, no duplicate.

## Work

Add one test that:

1. Loads `tests/fixtures/telegram_post.json` through the fake Telegram client.
2. Loads `tests/fixtures/web_page.html` through `httpx.MockTransport`.
3. Calls `collect` once and asserts both cards, including `source`, `external_id`, `text`, `url`, `published_at`, and `status="new"`.
4. Calls `collect` again and asserts the row count is unchanged.
5. Calls `draft(card, mode="mock")` and asserts `учебный черновик`.
6. Saves the draft, sets status `draft`, then `approved`.
7. Asserts the final card is `approved`, `draft_text` is kept, and no status `sent` exists.

The Telethon client in this test is the fake from Day 7. The HTTP client is only the mock transport. Assert the web handler was used and that no code path constructed `TelegramClient`.

If an import of the real factory would touch the network, keep that import out of this test.

## Constraints

No live requests. No `.session` file created by the test. No model call. Mock mode only in this test.

## Done when

`pytest` passes this integration test and the earlier tests. The test is the automated form of specification scenario step 6.
