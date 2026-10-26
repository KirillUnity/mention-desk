# Day 6 — Telegram source interface

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

`fetch_channel(channel, client)` can map a channel history into cards. The Telethon client is created outside the function so tests can pass a fake. This day does not open a real Telegram session.

## Work

In `app/services/telegram_source.py`:

- `fetch_channel(channel, client)` reads the history of the caller's own channel through the injected client and maps each message to `Card(source="telegram", external_id=str(message.id), ...)`.
- `text` is the message text, passed through `normalize`.
- `url` is a link to that post when the channel username and message id are enough to build `https://t.me/<channel>/<id>`. If the channel value is already a URL or an id, keep one obvious rule and document it in the module docstring.
- `published_at` is the message date as ISO 8601, or `None`.
- `status` is `new`.
- Empty or service messages with no text are skipped.

Client construction belongs in `app/main.py` or a small factory next to it, and only on the real runtime path. `fetch_channel` must not call `TelegramClient(...)`. The factory reads `TELEGRAM_API_ID`, `TELEGRAM_API_HASH`, and the session filename `mention_desk.session` from the environment when the real client is eventually built. Do not instantiate that client while importing the app and do not instantiate it in tests.

Plug the function into `collect` using the source shape from Day 3. The runtime wiring can stay behind a function that receives the client later. Day 12 is what connects the button.

## Constraints

Do not log in, do not send messages, do not download media, do not write a `.session` file, and do not call Telegram's network. Reading history in production later is still read-only. No proxies and no foreign accounts.

## Done when

A fake client object passed into `fetch_channel` returns telegram cards whose `external_id` is the message id. Importing `app.main` and `telegram_source` does not create a session file and does not open a network connection.
