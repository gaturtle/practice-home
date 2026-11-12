# Home repo

Run agents from here. This repo holds the shared skills (`.claude/skills`, `.agents/skills`, installed from mattpocock/skills via `skills-lock.json`), the glossary and the ADRs for the Reading List project. The app code lives in the sibling repos below.

## Other Repos

- reading-list-api (the HTTP API: Hono, Zod/OpenAPI, SQLite): `../reading-list-api`. Clone from https://github.com/gaturtle/reading-list-api if missing. Read its AGENTS.md first.
- reading-list-web (the React + Vite UI): `../reading-list-web`. Clone from https://github.com/gaturtle/reading-list-web if missing. Read its AGENTS.md first.

## Language

Use the terms in `GLOSSARY.md` (Reader, Reading List, Book, Reading Status, Rating) in code, UI copy, commits and PRs. Architectural decisions are in `docs/adr/`.

## Cross-repo changes

For a feature that touches both apps, work in this order:

1. **API first.** Change `../reading-list-api`, test it, then run `pnpm gen:openapi` to regenerate `openapi.json`.
2. **Then web.** In `../reading-list-web`, run `pnpm gen:api` to regenerate the typed client from `../reading-list-api/openapi.json`, then fix type errors and build the UI.
3. **One branch and one PR per repo**, using the same branch name in both. Merge the API PR first.

## Running locally

From this repo:

- `pnpm dev`: starts the API (http://localhost:3000) and web (http://localhost:5173) together. The web app proxies `/api` to the API.
- `pnpm test`: runs the tests in both repos.
- `pnpm install:all`: installs dependencies in both repos.
