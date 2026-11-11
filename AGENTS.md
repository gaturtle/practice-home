# Home repo

This is the home repo. Run agents from here. It holds the skills (`.claude/skills`), the glossary and the ADRs for the Reading List project. The app code lives in sibling repos.

## Repos

- For the API, clone `gaturtle/reading-list-api` to `../reading-list-api` if it is missing.
- For the web app, clone `gaturtle/reading-list-web` to `../reading-list-web` if it is missing.

Each repo has its own `AGENTS.md` with its commands and conventions. Read it before changing that repo.

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
