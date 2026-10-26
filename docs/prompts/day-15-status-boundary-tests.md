# Day 15 — Status and boundary tests

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

Automated tests lock the invariants from the specification: forward statuses only, no `sent`, and a failed source does not erase earlier cards.

## Work

Add tests, reusing the temporary database:

- `approved → draft` is rejected and the stored status stays `approved`
- `draft → new` is rejected and the stored status stays `draft`
- `set_status(..., "sent")` is rejected. The same for any «отправлено» value if the API accepts a string
- `new → approved` stays rejected
- `collect` when the second source raises: the card from the first source is still in the database, and the result marks the failed source unavailable

Keep these tests offline. Fakes only. No new product behavior unless a test shows the invariant is missing. If it is missing, fix the store or `collect` to match the specification, then re-run the tests.

## Constraints

Do not weaken a test to match a bug. Do not add a `sent` status to make a test pass. Do not open Telegram or HTTP.

## Done when

`pytest` is green and the cases listed above are present in the test file. This covers the method invariants in the technical specification.
