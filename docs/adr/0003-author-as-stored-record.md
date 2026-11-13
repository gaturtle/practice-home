# Author is a stored record, not derived from Book names

An Author is its own row in the API's database, and each Book refers to its Author by id. We chose this over working Authors out on the fly by grouping Books on their cleaned-up author name (ignoring case and spacing). Grouping would need no table, no migration and no clean-up, but its key would be the name itself, so renaming an Author would change their identity and their URL. A stored record gives each Author a permanent id that survives renames and merges. It also makes the Book–Author link a real relationship, which is what this feature exists to practise.

## Consequences

- An Author exists only while at least one Book is theirs, so removing a Book must also delete its Author when no other Book points to them.
- Matching names ignoring case and spacing is still how a new Book finds its Author, so the cleaned-up name has to be stored and kept unique.
- Renaming an Author to a name that matches another Author merges them: the Books move to the existing Author and the renamed one is deleted.
