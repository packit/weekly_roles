# How Packit team does Kanban?

Packit team follows a Kanban methodology but not use just a board with `todo`/`wip`/`done` columns. We have couple of extra columns and rules. Let's take a look:

## Packit Kanban Board

The current board is publicly available at https://github.com/orgs/packit/projects/7.

### Card states (board columns)

#### `new`

This is a column where all the new cards are added (automatically for public repositories, via GitHub action for private ones). At the beginning of each [Standup meeting](./meetings#Standup), a [triaging](#Triage) of new unassigned cards happens resulting in either moving the card away into one of the other states or assigning a team member to further investigate and finish the triaging. Also, a card can be returned to the `new` column if we realise the card is not clear or needs attention (e.g. to check for relevancy). This can happen when going through the old cards during [Standup](./meetings#Standup).

#### `backlog`

This is a pile of cards that have been basically categorized but are not a current priority (present in the `priority-backlog`), or it is not within our capacity to finish them within next 3 months. The order of this column is not maintained.

#### `priority-backlog`

This is an ordered list of categorized cards that the team is considering for the next work. The priority is based on impact, user value, urgency and the current team plans and capacity. The team is revisiting the epic-level priorities on a quarterly level and the cards in the `priority-backlog` should be done in 3 months. This means the number of cards needs to be maintained below ~50.

#### `refined`

This column consists of cards prepared to be worked on next when someone has a free capacity. The tasks should be actionable right away and doable by anyone. To get a card to this state a [refining process](#Refine) is used.

#### `in-progress`

When a card is taken by someone from a `refined` column, it is assigned to that person and moved to this column. This is the list of cards that are being actively worked on.

#### `in-review`

These tasks are nearly finished, being reviewed and polished.

#### `done`

This is the column where all the done cards result.

### Card labels

Here is a list of labels we use to categorize cards to help ourselves navigate through the backlog and plan our work.
Note that there is no priority label since this consists of multiple factors like _impact_ and _gain_. Combined with the urgency, our current plans (based on demand) and capacity, this is visible from the place on the board.

#### Area

These labels help to group related cards across all the projects. The area can determine a target git-forge (e.g. `area/github`/`area/gitlab`), service being integrated (e.g. `area/testing-farm` or `area/copr`) or a shared code-level logic (e.g. `area/config` or `area/database`).

#### Complexity

This is a rough estimation of how much time is expected for this card to be done. Mainly to separate epics from single cards. -- work consisting of multiple tasks, usually taking more time and being discussed in quarterly meetings.

#### Gain

This determines the value it gives to affected users.

- `gain/high`: significant change for the user, e.g.:
  - New users can start using Packit.
  - Significant time is saved for the user when the task is done.
  - A user might stop using Packit when not available.
- `gain/low`: change not so important to start/stop using Packit, e.g.:
  - If `workaround-exists` (separate label).
  - User can fully benefit from Packit without the card being done.

#### Impact

These labels determine the size of a user group affected by this card combined with a frequency. To make this comparable, it is based on generic user roles like `upstream-developer` or `fedora-maintainer`. This means we won't mark a card as high-impactful when all (but a few in reality) users of a rear functionality are affected.

This needs to be combined with frequency -- this means how often they can benefit from a new feature or how often an issue is hit.

- `impact/high`: Many users are affected by this card and the occurrence is significant.
- `impact/low`: The card affects only a few users and/or rarely-used features. The issues are not hit often.

#### `blocked`

This label is used to show that we can't move this card/work forward because of an external reason. For a reason, this is not a board state since the card can be blocked on various states of the development.

#### `resource-reduction`

This label marks tasks that can lead to better resource utilisation. (Without significant functional loss, fewer resources can be spent.)

#### `discuss`

This label helps us gather cards to be discussed in weekly architecture meetings.

#### Action items / for discussion (Section to be removed before merging.)

##### To remove:

- `needs-info` (`new` state)
- `needs-design` (not used)
- `pinned`
- `triaged`
- `invalid`
- `RHOSC`, `GSOC`
- Is `has-release-notes` still relevant?
- Do `wontfix` and `invalid` labels provide any value when the issue is closed and marked as not planned with a comment?

##### Edits

- Rename `area/refactor` to `area/technical-debt`.
- Rename `testing` to `area/testing`

##### Questions

- Do we need complexity?
- Do we need `kind/documentation`? (We have a separate project for doc-only cards and don't mention this explicitly on other cards.)

### Grooming process cheat sheet:

### Triage

:::note

Process of handling new cards and categorizing them.

:::

1. Triaging
   1. **_Is the card not valid or out of our scope?_** => Politely provide reasoning and close the issue as not planned.
   2. **_Do we have a related card for this?_** => Link the relevant cards, and add to an epic if applicable. Link and close in favour of a duplicate issue if applicable.
   3. **_Do we need to get more information from the requester? Is it necessary for a team-member to take a look?_** => Assign a person to continue with the discussion and leave it in the `new` column.
   4. **_Is the task actionable and we are in general sure what the card is about and which way the solution be chosen?_** => Enhance the title and description, if needed, and move outside of the `new` column.
   5. **_Does the issue come from an external person and there is a chance of contributing this?_** => Politely ask if the requester would be able to contribute this with our help.
2. Labelling
   1. **_Can we get a new user or allow a new user to start using a feature? Can this determine for the user if Packit will be used in the future?_** => Add `gain/high` label.
      **_Is there a workaround or the feature is not significant to the user?_** => use `gain/low`.
   2. **_Is this affecting many users from a role group (e.g. Fedora maintainers, Upstream developers,...)? Can this bring a significant number of new users?_** => Add `impact/high`, add `impact/low` otherwise.
   3. Add a `demo` label for tasks that are worth presenting to the team or users.
   4. Add `area/*`, `kind/*` and other suitable labels if needed.
   5. Add a deadline if applicable.
3. Prioritising
   1. **_Is there a strict deadline? Did we break anything crucial? Are we significantly blocking users?_** => [Refine the card](#Refine) right away and move to the `refined` column.
   2. **_Is there a `gain/high` and `impact/high` label? Do we need/want to finish this within ~3 months? Is this part of our current high-level plans for the quarter?_** => Move to the `priority-backlog` column.
   3. **_Is there a workaround? Doesn't the task make a user start/stop using Packit? Or, otherwise. _** => move to the `backlog` column.

### Refine

(=prepare card for work)

1. Clarification, make sure that
   1. It is clear what needs to be done and there is a definition of done.
   2. Everyone in the team understands the task to an extent of being able to work on the card themselves.
      (Suggest splitting (i.e. _breakdown_) or brainstorming, if needed.)
   3. No one has any objections to the card itself or the chosen way.
2. Vote about the time estimation (return to the previous step in case new concerns are raised).
   1. Voting is done via hands and by using a Fibonacci sequence number.
   2. If the vote is not united, discuss the reasons and the card in more detail so there is an agreement.
   3. Split the card if the team is thinking about picking `8` (=more than a week) as a time estimate.
   4. Fill the result in card details.
   5. Move the card to the `refined` column and reorder if needed.