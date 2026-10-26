# Day 8 — Mock drafts

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

`draft(card, mode)` in mock mode returns a fixed training reply, with no network, and the reply is marked as a training draft.

## Work

In `app/services/drafts.py`:

- `draft(card, mode) -> str`
- When `mode` is `mock`, or when `mode` is omitted, return a deterministic string. The same card always yields the same string in a test.
- The string includes the exact mark `учебный черновик` so the page can show it later.
- The string may quote or compress `card.text`, but it must not pretend to be a live model and must not add facts that are not in `card.text`. A fixed template that includes the card text is the right shape.
- `LLM_MODE` from the environment defaults to `mock`. If `LLM_MODE` is unset, empty, or `real` while no model key is configured, force `mock`. Read the key only to decide the force. Do not send it anywhere. Name the key in one place, for example `LLM_API_KEY`, and list it in `.env.example` with an empty value. Do not require a real key.
- `mode="real"` may raise `NotImplementedError` or fall through to mock today. The real request is Day 9. Prefer fall through to mock if that keeps the app safe.

Add a unit test that calls `draft` with `mode="mock"` and asserts the exact training text and the mark `учебный черновик`. The test must not open a socket.

## Constraints

No HTTP client calls on this day. No Telegram sends. No status change yet. That is Day 10. Do not put a model key in source.

## Done when

The unit test passes and shows a deterministic training draft. With no key in the environment, the resolved mode is `mock`.
