# Day 9 — Real draft mode with a safe fallback

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

`mode="real"` sends one request to a cloud model through an `httpx` client supplied by the caller. Network and key failures fall back to the Day 8 mock draft. The process does not crash. Missing key stays on mock.

## Work

- Extend `draft(card, mode, client=None)`. Keep the Day 8 mock path.
- Real mode uses the injected `httpx.AsyncClient` or a sync client if the rest of drafts is sync. Match the style you already chose, but the test must pass `MockTransport` and the function must not construct its own client for the test path.
- One request only. The prompt tells the model to retell facts that are present in `card.text` and to add nothing else. Put that instruction in source as the request body, not as a hidden second call.
- On timeout, connection error, non-success HTTP status, or a missing key, return the mock draft from Day 8. Do not raise to the caller for those cases.
- If the key is absent, do not issue the HTTP request at all. Return mock.

Tests, all offline:

- `MockTransport` records one request and returns a short factual reply. Assert one request and that the reply is what the handler returned.
- A second test with no key asserts the mock mark `учебный черновик` and asserts the handler was not called.
- A third test where the handler returns HTTP 500, or raises a transport error, asserts the mock draft comes back.

## Constraints

Do not call a live model while writing or running tests. Do not log the key. Do not add a send-to-Telegram step. If the provider choice is ambiguous, use one documented chat-completions URL read from the environment, defaulting so tests never need a real host. Mock mode remains the default and the delivery mode if real mode is awkward. That is an accepted state of the specification.

## Done when

`pytest` shows one mocked request on the success path, zero requests without a key, and a mock fallback when the fake server fails. `LLM_MODE` still defaults to `mock`.
