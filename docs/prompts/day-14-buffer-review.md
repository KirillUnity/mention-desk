# Day 14 — Buffer and review

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

A person can walk steps 1–5 of the specification by hand. Small UX gaps from that walk are fixed. The slice is ready to demo as a user.

## Work

Walk this scenario against the local server. Use mock drafts. Use empty settings first, then a configured channel and page only if those settings already exist in the local environment. Do not invent secrets. If live Telegram or the live page is not available, walk steps 1, 2, 4, and 5 with fixtures or cards inserted through the store, and write down that step 3 waited on credentials.

1. Open the page with no cards. See «Пока нет упоминаний».
2. Press «Собрать» with channel and page unset. See `Укажите канал и адрес страницы` and no new cards.
3. With sources configured, press «Собрать». Cards show `новое`.
4. Press «Собрать» again. The count stays the same.
5. Open a card, press «Черновик», compare the reply to the source text, press «Одобрить». Status is `одобрено`. The message stayed on the page.

Fix only the UX nicks this walk exposes:

- the mark `учебный черновик` is visible above a mock reply
- dates on the page are readable, while stored values stay ISO 8601
- labels stay Russian as in the specification
- the empty state, the settings sentence, and the source banner do not cover each other

Do not add features from Week 3. Do not add «Отправить».

## Constraints

If a live collect runs, it only reads. It must not post to the channel. Keep `LLM_MODE=mock` for the demo walk.

## Done when

Steps 1–5 have been executed as far as the local environment allows, and the UX fixes above are present. Write a short note at `docs/day-14-walk.md` listing which steps you actually saw in the browser. That note is a walk log, not a new specification.
