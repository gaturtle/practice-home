# Issue tracker: GitHub, one tracker per repo

Issues and specs live as GitHub issues, in the repo the work belongs to. Use the `gh` CLI for all operations.

Agents run from the home repo, so `gh` would default to `gaturtle/practice-home`. **Always pass `-R <repo>`** (or `--repo`) to pick the tracker.

## Which repo an issue goes in

| The issue is about…                           | Repo                        |
| --------------------------------------------- | --------------------------- |
| The HTTP API, the database, `openapi.json`    | `gaturtle/reading-list-api` |
| The React UI, the generated client            | `gaturtle/reading-list-web` |
| The glossary, ADRs, skills, home repo scripts | `gaturtle/practice-home`    |

## Features that cross repos

One issue per repo, with the same title in each. The web issue is **blocked by** the API issue (the API lands first, see AGENTS.md › Cross-repo changes): add the dependency with `gh issue edit <web#> -R gaturtle/reading-list-web --add-blocked-by gaturtle/reading-list-api#<api#>`, or, if that fails, put a `Blocked by: gaturtle/reading-list-api#<n>` line at the top of the body. Each body links to the other issue as `gaturtle/<repo>#<n>`. Never use a bare `#<n>`, which only resolves within the same repo.

## Conventions

- **Create an issue**: `gh issue create -R <repo> --title "..." --body "..."`. Use a heredoc for multi-line bodies.
- **Read an issue**: `gh issue view <number> -R <repo> --json number,title,body,labels,comments`.
- **List issues**: `gh issue list -R <repo> --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'` with appropriate `--label` and `--state` filters. To see everything, run it once per repo.
- **Make an issue a sub-issue of a parent**: `gh issue create -R <repo> --parent <parent> ...`, or `gh issue edit <parent> -R <repo> --add-sub-issue <child>` afterwards (`gh` 2.94+). Older `gh`: `gh api --method POST repos/<owner>/<repo>/issues/<parent>/sub_issues -F sub_issue_id=<child-db-id>` (database id, as in **Blocking** below). Without sub-issues, put `Part of gaturtle/<repo>#<parent>` at the top of the child body.
- **Blocking**: GitHub's **native issue dependencies**, the canonical, UI-visible representation. Add an edge with `gh issue create -R <repo> --blocked-by <n>,<n> ...` or `gh issue edit <child> -R <repo> --add-blocked-by <blocker>` (`gh` 2.94+). Older `gh`: `gh api --method POST repos/<owner>/<repo>/issues/<child>/dependencies/blocked_by -F issue_id=<blocker-db-id>`, where `<blocker-db-id>` is the blocker's numeric **database id** (`gh api repos/<owner>/<repo>/issues/<n> --jq .id`, _not_ the `#number` or `node_id`). Where dependencies aren't available, fall back to a `Blocked by: gaturtle/<repo>#<n>` line at the top of the child body. A ticket is unblocked when every blocker is closed.
- **Count open blockers**: `gh api repos/<owner>/<repo>/issues/<n> --jq .issue_dependencies_summary.blocked_by` (open blockers only; 0 means startable). `gh issue view <n> -R <repo> --json blockedBy` also lists closed blockers, so filter it: `--jq '[.blockedBy.nodes[] | select(.state == "OPEN")] | length'`.
- **Comment on an issue**: `gh issue comment <number> -R <repo> --body "..."`
- **Apply / remove labels**: `gh issue edit <number> -R <repo> --add-label "..."` / `--remove-label "..."`
- **Close**: post the explanation with `gh issue comment <number> -R <repo> --body "..."`, then `gh issue close <number> -R <repo>`. On an already-closed issue, `gh issue close --comment` drops the comment and still exits 0.

File issues only into the user's own repos (the three above). Never file into `mattpocock/skills` (the repo these skills come from) unless the user owns it.

## Pull requests as a triage surface

**PRs as a request surface: no.** _(Set to `yes` if these repos treat external PRs as feature requests; `/triage` reads this flag.)_

When set to `yes`, PRs run through the same labels and states as issues, using the `gh pr` equivalents (with `-R <repo>`):

- **Read a PR**: `gh pr view <number> -R <repo>`, then `gh pr view <number> -R <repo> --comments` (off a TTY, `--comments` prints only the comments, not the body), and `gh pr diff <number> -R <repo>` for the diff.
- **List external PRs for triage**: `gh api --paginate 'repos/<owner>/<repo>/pulls?state=open' --jq '.[] | select(.author_association | IN("OWNER","MEMBER","COLLABORATOR") | not) | {number, title, author: .user.login, author_association, labels: [.labels[].name]}'`.
- **Comment / label / close**: `gh pr comment`, `gh pr edit --add-label`/`--remove-label`, `gh pr close`, each with `-R <repo>`.

GitHub shares one number space across issues and PRs within a repo, so a bare `#42` may be either: resolve with `gh pr view 42 -R <repo>` and fall back to `gh issue view 42 -R <repo>`.

## When a skill says "publish to the issue tracker"

Create a GitHub issue in the repo the work belongs to (see **Which repo an issue goes in**). For a cross-repo feature, create one per repo as in **Features that cross repos**.

## When a skill says "fetch the relevant ticket"

Read it as in **Read an issue** above. If the reference has no repo, ask which repo, or check each of the three.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a single issue with **child** issues as tickets.

- **Map**: a single issue labelled `wayfinder:map`, holding the Notes / Decisions-so-far / Fog body. A map for a cross-repo feature lives in `gaturtle/practice-home`; a single-repo map lives in that repo. `gh issue create -R <repo> --label wayfinder:map`.
- **Child ticket**: an issue linked to the map as a GitHub sub-issue (see **Make an issue a sub-issue of a parent**), filed in the repo the work belongs to. Where sub-issues aren't available (including across repos), add the child to a task list in the map body as `gaturtle/<repo>#<n>` and put `Part of gaturtle/<map-repo>#<map>` at the top of the child body. Labels: `wayfinder:<type>` (`research`/`prototype`/`grilling`/`task`). Once claimed, the ticket is assigned to the driving dev.
- **Blocking**: see **Blocking** under Conventions.
- **Frontier query**: list the map's open children (`gh issue list -R <repo> --state open`, scoped to the map's sub-issues / task list), drop any with an open blocker (see **Count open blockers**, or an open issue in the `Blocked by` line) or an assignee; first in map order wins.
- **Claim**: `gh issue edit <n> -R <repo> --add-assignee @me`, the session's first write.
- **Resolve**: `gh issue comment <n> -R <repo> --body "<answer>"`, then `gh issue close <n> -R <repo>`, then append a context pointer (gist + link) to the map's Decisions-so-far.
