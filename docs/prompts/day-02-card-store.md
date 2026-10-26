# Day 2 — Card contract and storage

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

A card can be stored in SQLite and its status can move forward on a temporary database. A unit test covers insert and a legal status change.

## Work

In `app/store.py` define `Card` with a dataclass or a pydantic model. Fields:

- `source` — `telegram` or `web`
- `external_id` — post id or canonical URL, stored as text
- `text`
- `url`
- `published_at` — ISO 8601 string or `None`
- `status` — `new`, `draft`, or `approved`

SQLite table `cards`:

- integer primary key `id`
- the fields above
- unique index on `(source, external_id)`

Functions:

- `init_db(path)` creates the table. Tests must pass a temporary file path. Do not require a global production path inside the test.
- `insert_card(card) -> int` inserts one row with status `new` when the caller did not set another legal status. A second insert with the same `(source, external_id)` must not create a second row. Raising on the unique constraint is acceptable today. `INSERT OR IGNORE` belongs to `collect` on Day 3.
- `list_cards() -> list[Card]`
- `get_card(card_id) -> Card | None`
- `set_status(card_id, status)`

Status rules inside `set_status`:

- allowed: `new → draft`, `draft → approved`
- refuse `approved → draft`, `draft → new`, `new → approved`, and any status named `sent` or `отправлено`
- refusal raises a clear exception and leaves the stored status unchanged

Add a unit test that uses a temporary database file: insert one card, read it back, move `new → draft`, and show that a backward move does not stick. Do not add network, Telethon, or HTML parsing.

Keep the database path configurable so Day 17 can keep `*.sqlite` out of git. Default local file, if you need one for the app, must match `.gitignore`.

## Constraints

No `draft_text` column yet. That is Day 10. No page UI. No live HTTP. Secrets stay out of source and out of git.

## Done when

The temporary-database test passes. A duplicate `(source, external_id)` cannot appear as two rows. `sent` is rejected. `uvicorn app.main:app` still starts.
