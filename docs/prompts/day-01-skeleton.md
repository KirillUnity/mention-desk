# Day 1 — Project skeleton

Implement only this day of Mention Desk. Read `docs/mention-desk-tz.md` before changing code. The repository root is the Mention Desk project. Do not create a nested `mention-desk/` directory. Do not implement later days. Leave `docs/prompts/` and the existing starter `README.md` unchanged.

## Outcome

The process starts with `uvicorn app.main:app` and returns a stub. There is no database, no collection, and no draft yet.

## Work

Create this layout at the repository root:

- `requirements.txt` with FastAPI, uvicorn, httpx, beautifulsoup4, telethon, pytest, and pytest-asyncio.
- `.env.example` listing `TELEGRAM_API_ID`, `TELEGRAM_API_HASH`, `TELEGRAM_CHANNEL`, `WEB_URL`, and `LLM_MODE=mock`. Leave secret values empty. Do not create a real `.env`.
- Confirm `.gitignore` ignores `.env`, `*.session`, `*.sqlite`, `__pycache__`, and virtualenvs. Extend it if a pattern is missing. Do not stop ignoring secrets.
- `app/__init__.py`
- `app/main.py` — a FastAPI application object named `app` that responds on `/` with a short stub. A one-line plain-text or minimal HTML response is enough. The real Mention Desk page is Day 11.
- `app/store.py` — importable module with a module docstring only.
- `app/services/__init__.py`
- `app/services/collect.py`, `app/services/telegram_source.py`, `app/services/web_source.py`, `app/services/drafts.py` — importable modules with module docstrings only. No collection or draft behavior.

Install the requirements into a local virtual environment if one is needed to prove the server imports. Do not commit `.venv` or any session file.

## Constraints

Python 3.12. No Docker, PostgreSQL, VK, Playwright, Selenium, proxies, email, or Telegram writes. Do not call Telethon. Do not put API keys or hashes in source. Do not replace the starter README with install docs; that is Day 18.

## Done when

`uvicorn app.main:app` starts and `GET /` returns the stub. Empty service modules import. `.env.example` exists and `.env` does not. The working tree has no `*.session` and no sqlite database.
