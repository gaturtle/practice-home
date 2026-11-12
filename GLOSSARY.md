# Reading List

A personal tracker for books a reader wants to read, is reading, or has finished. Exists as a practice target for the Home Repo multi-repo agent workflow.

## Language

**Reader**:
The single person who owns the Reading List. There is exactly one.
_Avoid_: User, account, member

**Reading List**:
The collection of all Books one reader is tracking.
_Avoid_: Library, bookshelf, catalog

**Book**:
A single title the reader has added to their Reading List.
_Avoid_: Item, entry, title

**Reading Status**:
Where a Book sits in its lifecycle: Want to Read, Reading, or Finished.
_Avoid_: State, progress, stage

**Rating**:
An optional whole-number score from 1 to 5 that the Reader gives a Finished Book. Only a Finished Book can have a Rating; a Book loses its Rating when it leaves Finished. A Finished Book with no Rating is unrated.
_Avoid_: Score, stars, review
