# Day 4 — Web source

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

A saved HTML document becomes one `Card` with `source="web"` and `external_id` set to the canonical URL. The HTTP client is passed in from outside.

## Work

In `app/services/web_source.py`:

- `fetch_page(client, url) -> str`
  - `client` is an `httpx.AsyncClient` created by the caller
  - perform one `GET` of `url`
  - return the response body as text
  - do not construct a client inside this function
- `parse_page(html, url) -> Card`
  - parse with BeautifulSoup
  - extract the visible article or main text, the canonical URL, and a publication date when the page has one
  - canonical URL preference: `<link rel="canonical">`, otherwise the `url` argument
  - `Card(source="web", external_id=<canonical url>, text=..., url=<canonical url>, published_at=<ISO 8601 or None>, status="new")`
  - trim text with the same whitespace rules as `normalize` in `collect.py`. Call `normalize` if that keeps one code path.

Wire this source so `collect` can call it, still receiving the client from outside. Do not hard-code `WEB_URL` inside `parse_page`.

Add a tiny local check that feeds a short HTML string held in the test or in the function's own development snippet. The fixture file and `MockTransport` suite are Day 5. This check must not perform a live HTTP request.

## Constraints

One page, one request shape. No crawling, no Playwright, no Selenium, no proxy, no login. Do not fetch arbitrary hosts while proving the parser. Ignore script and style text.

## Done when

`parse_page` on a saved HTML string returns a web card whose `external_id` is the canonical URL, whose text has no surrounding junk whitespace, and whose date is ISO 8601 or `None`. `fetch_page` accepts a caller-supplied client and does not build one itself.
