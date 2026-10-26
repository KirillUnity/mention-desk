# Day 21 — Handoff

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before the final check. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Leave `docs/prompts/` unchanged except to fix a prompt only if the user asks.

## Outcome

The v1 vertical slice is ready to show. Offline `pytest` is green, the README matches the program, the file layout matches the specification, and a short demo trail shows the five visible states.

## Work

Check the repository against this layout. Extra tests and `docs/` notes may exist. These paths must exist and be real:

- `README.md`
- `.env.example`
- `.gitignore`
- `requirements.txt`
- `app/main.py`
- `app/store.py`
- `app/services/collect.py`
- `app/services/telegram_source.py`
- `app/services/web_source.py`
- `app/services/drafts.py`
- `tests/fixtures/`
- `tests/test_collect.py`

Run `pytest` and confirm it does not open live Telegram, live HTTP, or a live model. Fix failures that are real regressions. Do not delete a test to go green.

Confirm the contract in code and README:

- `collect` dedups on `source + external_id` and keeps stored cards when one source fails
- `draft` is mock by default, with `учебный черновик`, and real mode falls back to mock without a key
- `set_status` allows only `new → draft → approved`
- card fields are `source`, `external_id`, `text`, `url`, `published_at`, `status`
- the page has no send action and the server does not post to Telegram or send mail

Demo trail, in the browser if it is available, otherwise by the HTML each route returns. Capture one screenshot or one saved HTML snippet per state under `docs/demo/` and list them in the README appendix:

1. empty table, «Пока нет упоминаний»
2. «Собрать» without settings, `Укажите канал и адрес страницы`
3. cards in `новое`
4. a card after «Черновик» with the training mark
5. the same card as `одобрено` with the draft still visible

Use fixtures for states 3–5 if a live channel is not configured. Label the demo as fixture-backed in that case.

Update the README only where the finished behavior disagrees with it.

## Constraints

Do not add VK, Playwright, email, or a send button on the last day. Do not commit secrets. Do not call live Telethon from tests. Mock remains a valid default.

## Done when

`pytest` is green offline, the README can start the project, the five demo states are saved, and the contract list above holds in code. This is the Day 21 checkpoint: the portfolio slice is ready to show.
