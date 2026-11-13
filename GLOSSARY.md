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

**Shelf**:
A named group of Books that the Reader makes. A Book can be on any number of Shelves, including none, and at most once on each. A Shelf exists until the Reader deletes it, even when it holds no Books; deleting a Shelf takes its Books off it but keeps them on the Reading List, and a Book removed from the Reading List leaves every Shelf it was on. No two Shelves have names that match ignoring case and spacing. A Shelf is not a Reading Status.
_Avoid_: Tag, label, category, collection

**Reading Status**:
Where a Book sits in its lifecycle: Want to Read, Reading, or Finished.
_Avoid_: State, progress, stage

**Rating**:
An optional whole-number score from 1 to 5 that the Reader gives a Finished Book. Only a Finished Book can have a Rating; a Book loses its Rating when it leaves Finished. A Finished Book with no Rating is unrated.
_Avoid_: Score, stars, review

**Average Rating**:
The mean of the Ratings on an Author's rated Finished Books. It is not itself a Rating. An Author with no rated Finished Books has no Average Rating.
_Avoid_: Author rating, author score

**Finished On**:
The day the Reader finished a Book. It is set to the current day when the Book becomes Finished, and the Reader may change it to any day that is not in the future. Only a Finished Book has a Finished On; a Book loses it when it leaves Finished. A Finished Book whose Finished On is unknown counts toward no Reading Goal until the Reader sets one.
_Avoid_: Completed date, finish date, read date

**Reading Goal**:
The number of Books the Reader aims to Finish in one calendar year. Each year has at most one Reading Goal, which the Reader may set or change; a year without one has no Reading Goal. A Book counts toward the Reading Goal of the year of its Finished On.
_Avoid_: Challenge, target, quota

**Year in Review**:
A summary of the Books the Reader Finished in one calendar year: how many, when, how they were Rated and whose they were. It is worked out from the Reading List and is never edited directly. A Book belongs to the Year in Review of the year of its Finished On; a Finished Book whose Finished On is unknown belongs to none.
_Avoid_: Wrapped, stats, report, recap
