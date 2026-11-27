# Intent: Filter movies by rating and sort them to see the newest releases first.
Author: M.Kamol Deeyeangyang and Naruechit Boonyoung, for the customer proxy (a parent of an 8-year-old). Status: draft.

## Problem
The movie list is too long to browse easily, forcing parents to open and check each movie one by one.
This makes finding suitable movies for their child slow and tedious.

## Proposed outcome
Users can choose a specific rating to filter the catalog themselves instead of having the system decide for them.
The filtered movie list is also ordered so that the newest movies appear first.

## Affected users and systems
Parents first, then every moviegoer. The movie catalog of Rotten Potatoes (Movie, Rating and
MovieCatalog from L2). There is no web page yet: the behaviour is checked through the domain classes.

## Constraints
Ratings are restricted to the 5 fixed values (G, PG, PG-13, R, NC-17), and the system requires no user login or personal data storage.
Plain Java this week, no database.

## Open questions
- Should users be able to select multiple ratings at once or only one rating at a time?
- What happens if no movies match the selected rating?
