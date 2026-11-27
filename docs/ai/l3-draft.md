Tool: Claude Opus 5.5 (Claude Code, desktop app)
Date: 2026-11-27
Prompt: docs/ai/l3-prompt.txt

# spec.md: Filter movies by rating, newest first

Source: intent.md (draft). Scope: the Rotten Potatoes movie catalog (Movie, Rating, MovieCatalog). Plain Java, in memory, no database, no login, no personal data.

## 1. Functional requirements

- REQ-1: The system shall offer exactly five ratings to choose from: G, PG, PG-13, R and NC-17.
- REQ-2: When the user chooses one rating, the system shall show every movie in the catalog that has that rating.
- REQ-3: The movies shown after choosing a rating shall not include any movie with a different rating.
- REQ-4: The movies shown after choosing a rating shall be ordered by release date, newest first.
- REQ-5: The full, unfiltered movie list shall also be ordered by release date, newest first.
- REQ-6: When two movies have the same release date, they shall be ordered by title, A to Z, ignoring case.
- REQ-7: The user shall be able to choose only one rating at a time.
- REQ-8: When no movie has the chosen rating, the system shall show an empty list and the message "No movies with this rating".
- REQ-9: A rating label that is not one of the five in REQ-1 (for example "X" or "pg-13") shall be rejected with an error that lists the five allowed ratings.
- REQ-10: Choosing a rating shall not change, add or remove any movie in the catalog.
- REQ-11: The filter shall respond within 1 second for a catalog of up to 10,000 movies.
- REQ-12: The system shall not ask for a login and shall not store any personal data about the user.

## 2. User stories

### Story 1: Filter movies by rating
As a parent of a young child
So that I can quickly find movies my child is allowed to watch
I want to see only the movies with the rating I choose

Acceptance criteria:
- AC-1.1: Choosing PG-13 shows every PG-13 movie in the catalog and no movie of any other rating.
- AC-1.2: The movies shown are ordered newest first.
- AC-1.3: Choosing a rating that no movie has shows an empty list and the message "No movies with this rating".
- AC-1.4: Only one rating can be chosen at a time.

### Story 2: See the newest movies first
As a moviegoer
So that I notice new releases without scrolling through old ones
I want the movie list to show the newest movies first

Acceptance criteria:
- AC-2.1: In the full list, a movie released in 2011 appears before one released in 1992.
- AC-2.2: Movies released on the same date appear in title order, A to Z.

### Story 3: Save a preferred rating
As a returning parent
So that I do not have to choose the same rating every visit
I want the site to remember the last rating I chose

Acceptance criteria:
- AC-3.1: After choosing G and coming back later, the list opens already filtered to G.
- AC-3.2: The user can clear the saved rating and see all movies again.

## 3. Gherkin scenarios for Story 1

```gherkin
Feature: Filter movies by rating
  As a parent of a young child
  So that I can quickly find movies my child is allowed to watch
  I want to see only the movies with the rating I choose

  Background:
    Given the catalog contains these movies:
      | title          | rating | released   |
      | Aladdin        | G      | 1992-11-25 |
      | Men In Black   | PG-13  | 1997-07-02 |
      | The Help       | PG-13  | 2011-08-10 |
      | Alien          | R      | 1979-05-25 |

  Scenario: Happy path - only the chosen rating is shown, newest first
    When I filter movies by rating "PG-13"
    Then I should see "The Help" before "Men In Black"
    And I should not see "Aladdin"
    And I should not see "Alien"

  Scenario: Sad path - no movie has the chosen rating
    When I filter movies by rating "NC-17"
    Then I should see no movies
    And I should see the message "No movies with this rating"

  Scenario: Sad path - unknown rating label
    When I filter movies by rating "X"
    Then I should see an error that lists "G, PG, PG-13, R, NC-17"
```

## 4. Assumptions and areas of concern

Assumptions (not stated in the intent):
1. REQ-6 and AC-2.2: the tie-break by title for equal release dates is my own choice.
2. REQ-7 and AC-1.4: one rating at a time is assumed; the intent lists this as an open question.
3. REQ-8 and AC-1.3: the empty list and the exact message text are assumed; the intent lists this as an open question.
4. REQ-9: users might type a label, so invalid labels are handled; the intent does not say how a rating is chosen.
5. REQ-11: the 1-second limit and the 10,000-movie size are guesses; the intent gives no performance target.
6. Story 3 assumes the site can remember a preference between visits.
7. "Newest" is taken to mean the release date, not the date the movie was added to the catalog.

Areas of concern:
1. Story 3 needs some way to remember the user, which conflicts with the constraint of no login and no personal data. It should be confirmed or dropped.
2. A rating is not the same as suitability: a parent may still need more than the rating to judge a movie for an 8-year-old.
3. The Movie record from L2 requires a rating, so movies without a rating cannot exist in the catalog today; if unrated movies are added later, the filter must decide where they go.
4. There is no web page yet, so "show" and "message" can only be checked through the domain classes this week.
