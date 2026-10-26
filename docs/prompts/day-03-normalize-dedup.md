# Day 3 — Normalize and deduplicate in collect

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

`collect` walks sources, normalizes records into cards, and skips a row whose `(source, external_id)` is already stored. One failing source does not wipe cards already saved and does not stop the other sources.

## Work

In `app/services/collect.py`:

- `normalize(raw) -> Card`
  - trim surrounding whitespace on text fields
  - collapse runs of blank lines inside `text` to a single blank line
  - `published_at` is an ISO 8601 string, or `None` when the raw value is missing or unusable
  - new cards start at status `new`
- `collect(sources) -> list[Card]`
  - `sources` is an iterable of callables or small adapter objects already used by the project. Each source returns zero or more raw records or cards. Pick one shape and document it in the module docstring. Day 4 and Day 6 must be able to plug in without rewriting the dedup rule.
  - persist with `INSERT OR IGNORE` on `(source, external_id)` via `app/store.py`. Add a store function if Day 2 only had a raising insert. Do not delete existing rows.
  - return the cards this call actually inserted, or return the full stored list if that is simpler, but a second call with the same `external_id` must not increase the row count. The test must assert the count.
  - isolate errors: if one source raises, record that the source is unavailable, continue with the remaining sources, and keep previously stored cards. Return the unavailable flag to the caller in a structured result. A bare boolean is enough if the UI does not exist yet. The banner text is Day 13.

Add a test that calls `collect` twice with the same `external_id` and asserts a single row. Use an in-memory or temporary SQLite file. Sources in this test can be plain fakes that return dicts. Do not open the network and do not call Telethon.

## Constraints

Do not implement HTML or Telegram parsing here. Do not add the Mention Desk page. A source failure must not call `DELETE` and must not drop the database file.

## Done when

The dedup test passes. A test or an assertion in the same file shows that a raising source leaves an already inserted card in place and still lets a healthy source insert. `pytest` for these new tests is green.
