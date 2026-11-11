# Home repo with pointers, not a monorepo

The Reading List frontend (`reading-list-web`) and backend (`reading-list-api`) are separate repos, cloned as siblings of this home repo (`../reading-list-web`, `../reading-list-api`). Agents run from this home repo, which holds all skills, steering files and the glossary, and its AGENTS.md points to the other repos. We chose this over a monorepo because the project exists to practise the Home Repo multi-repo workflow. A monorepo would remove the repo boundary we want to learn to work across.

## Consequences

- One feature that spans both apps means two PRs, one per repo, landed backend first.
- Skills and the glossary are not copied into the child repos. Run agents from the home repo.
