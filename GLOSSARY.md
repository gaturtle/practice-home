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

**Author**:
Whoever the Reader names as having written a Book. Every Book has exactly one Author, and Books whose author names match, ignoring case and spacing, share the same Author. An Author exists only while at least one Book on the Reading List is theirs.
_Avoid_: Writer, creator, artist

**Reading Status**:
Where a Book sits in its lifecycle: Want to Read, Reading, or Finished.
_Avoid_: State, progress, stage

**Rating**:
An optional whole-number score from 1 to 5 that the Reader gives a Finished Book. Only a Finished Book can have a Rating; a Book loses its Rating when it leaves Finished. A Finished Book with no Rating is unrated.
_Avoid_: Score, stars, review

**Average Rating**:
The mean of the Ratings on an Author's rated Finished Books. It is not itself a Rating. An Author with no rated Finished Books has no Average Rating.
_Avoid_: Author rating, author score
