# Day 5 — Web fixtures and offline tests

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

`pytest` covers the web source with a saved HTML fixture and `httpx.MockTransport`. The tests perform no live requests. A second collect of the same page does not add a second card.

## Work

- Add `tests/fixtures/web_page.html`, a saved slice of a simple article page. Include a canonical link, a visible text block, and a date the parser can read. Keep it a fixture, not a copy of a whole site.
- Add `tests/test_collect.py`.
- Build the client the way the technical specification shows for Career Agent: a handler plus `httpx.MockTransport`, then `httpx.AsyncClient(transport=transport)`. The handler returns the fixture body and must not contact the network.
- Assert the card fields: `source == "web"`, `external_id` equals the canonical URL, `text` contains the article text and not script or style, `url` is that canonical URL, `published_at` is ISO 8601 or `None`, `status == "new"`.
- Run collect twice through the same fake transport and assert the stored row count stays the same.
- Assert the handler saw `GET` and saw it only for the configured page URL.

If pytest-asyncio is required, mark the async test explicitly. Point store and collect at a temporary database.

## Constraints

No live HTTP. No Telethon. No Telegram fixture yet. That is Day 7. Do not add retries that call a real host when the mock is missing. Fail the test instead.

## Done when

`pytest` is green for the web tests. A search of the test run shows no socket to the public internet. You can confirm that by the mock handler serving the fixture and by avoiding any client constructed without `MockTransport` in this test.
