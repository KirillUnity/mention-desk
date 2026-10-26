# Day 18 — README

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` and the current `README.md` before editing. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` unchanged.

## Outcome

Another person can set up, run, and test the project from `README.md`, and can see what the program deliberately does not send.

## Work

Rewrite `README.md` in two parts. Part 1 is for a reader in front of the page and the terminal, in Russian, with no file paths, class names, or parameters mixed into those paragraphs. End part 1 with a short check scenario of the six user-visible steps. Then a horizontal rule and the heading `Техническое приложение`.

Part 1 covers:

- what the user sees on the page
- empty table text `Пока нет упоминаний`
- «Собрать» without settings shows `Укажите канал и адрес страницы`
- collect, no duplicate, «Черновик», «Одобрить», statuses «новое», «черновик», «одобрено»
- training draft mark `учебный черновик` when the model is not connected
- banner `Источник сейчас недоступен`
- how to install dependencies, copy `.env.example` to `.env`, fill the local Telegram api id, api hash, own channel, and one page URL, and create the Telethon session on this computer
- how to start the server and how to run the tests
- a section on what this version does not send: no posts to Telegram, no email, no evasion of platform limits; VK and browser automation are not in this version

Part 2, the technical appendix, covers the file map, `collect` / `draft` / `set_status`, environment names, the session filename `mention_desk.session`, and a short mermaid flowchart of page → collect → cards → draft → approved. Keep the Day 1–21 prompt index only if it still helps a developer. The install instructions belong where a newcomer will find them. If you mention commands, put the command lines in the appendix or in a clearly marked terminal section that part 1 introduces in plain language.

Look at the real modules while writing the appendix. Do not document functions that do not exist.

## Constraints

Do not paste secrets from a local `.env` into the README. Do not tell the reader to commit a session. Python 3.12, no Docker and no PostgreSQL in the setup path.

## Done when

A newcomer can follow the README from an empty virtualenv to a running page and a green `pytest` without asking where the entry point is. The no-send section is explicit. Part 1 stays free of module names.
