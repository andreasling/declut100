# 100-Day Decluttering Challenge — Prototype Requirements Specification

## 1. Purpose

The app supports a 100-day decluttering challenge.

Each day, the user throws away, donates, recycles, or otherwise removes some number of items. The app records the number of items removed each day and tracks progress across the full 100-day challenge.

The challenge has two related goals:

1. **Unique-number goal:** Try to complete days with as many different daily totals from **1 through 100** as possible.
2. **Volume goal:** Remove at least **5,050 items** over the full 100 days.

A mathematically perfect challenge consists of using every number from 1 through 100 exactly once. These values sum to 5,050.

Duplicate daily totals are allowed. They reduce unique-number coverage but do **not** constitute failure.

---

## 2. Core Concepts

### Challenge

A challenge consists of:

- 100 days
- one daily item total per completed day
- a cumulative total of all removed items
- a set of unique daily totals achieved
- progress toward:
  - 100 unique numbers
  - 5,050 total items

### Day

Each challenge day has:

- day number, 1–100
- date
- status:
  - not started
  - in progress
  - completed
- zero or more item-count additions
- current daily total
- completed daily total, once the day is finished

### Addition

The user does not need to enter the complete daily total at once.

A day consists of individual additions, for example:

- morning: +5
- afternoon: +3
- evening: +12

Daily total: 20

Each addition should be retained individually so it can be inspected, corrected, or removed.

---

## 3. Functional Requirements

### 3.1 Start a challenge

The user can start a new 100-day challenge.

For the prototype, the challenge may begin on the current date.

The app should track:

- challenge start date
- current challenge day
- elapsed/completed days

Support for multiple simultaneous challenges is not required for the initial prototype.

---

### 3.2 Record items throughout the day

The primary interaction should be adding items to today's running total.

The user must be able to:

- add 1 item quickly
- add a small preset amount, such as +2 or +5
- enter an arbitrary positive number of items
- make multiple additions throughout the same day

Example:

```text
Today

18 items so far

[ +1 ] [ +2 ] [ +5 ] [ Add… ]

Recent additions
19:20  +6
14:10  +3
08:35  +9
```

Using `Add…` should allow the user to enter a number such as `13`, meaning **add 13 items**, rather than set the day's total to 13.

---

### 3.3 Edit today's additions

The user should be able to correct mistakes.

At minimum:

- remove an addition
- edit an addition

The daily total must be recalculated automatically.

An undo action immediately after adding an entry would also be useful.

---

### 3.4 Show today's uniqueness status

While the day is in progress, the app should compare the current daily total against previously completed days.

Possible states include:

#### Current total is unused

```text
37 items so far

New number
37 has not been used before
```

#### Current total has already been used

```text
39 items so far

Already used
Previously completed on Day 14
```

Passing through an already-used number during the day is not an error.

The uniqueness check matters primarily when the user decides to finish the day.

---

### 3.5 Suggest nearby unused totals

The app should help the user achieve unique daily totals.

If the current total has already been used, show nearby unused numbers.

Example:

```text
39 has already been used.

Next unused number: 40
1 more item
```

Optionally show several nearby choices:

```text
Unused nearby:
40  +1
42  +3
44  +5
```

This is guidance only. The user must always be allowed to complete the day with a duplicate value.

---

### 3.6 Finish a day

The user can explicitly mark the current day as complete.

Before completion, show:

- daily total
- whether that number has previously been used
- effect on unique-number coverage
- contribution to total item count

For an unused value:

```text
Today's total: 37

37 is a new number.

Completing today:
+37 items
+1 unique number

[ Complete day ]
```

For a duplicate:

```text
Today's total: 39

39 was already used on Day 14.

Completing today:
+39 items
+0 unique numbers

[ Complete day ]
```

Duplicate totals must not block completion.

---

## 4. Progress Tracking

### 4.1 Unique-number progress

Track how many distinct daily totals between 1 and 100 have been achieved.

Example:

```text
Unique numbers
22 / 100
```

The app should visually represent the numbers 1–100 in a grid.

Example:

```text
  1   2   3   4   5   6   7   8   9  10
 11  12  13  14  15  16  17  18  19  20
 ...
 91  92  93  94  95  96  97  98  99 100
```

Each number should visually indicate whether it has:

- never been used
- been used once
- optionally, been used multiple times

A duplicated value could display a marker such as `×2`.

---

### 4.2 Total-item progress

Track the cumulative number of items removed.

Display progress toward 5,050:

```text
Items
1,963 / 5,050
```

Values above 5,050 are valid.

Example:

```text
5,384 / 5,050
Goal reached
```

---

### 4.3 Day progress

Display challenge progress separately:

```text
Day 27 of 100
```

The following are therefore three distinct metrics:

- days completed
- unique numbers achieved
- total items removed

They should not be collapsed into a single percentage or score.

---

## 5. History

The user should be able to inspect previous days.

Example:

```text
Day 1   72 items
Day 2    4 items
Day 3   31 items
Day 4   72 items
```

History should show enough information to identify duplicates.

For the prototype, each completed day should contain:

- challenge day number
- calendar date
- final item count
- individual additions made that day

Editing completed days may be supported, but is lower priority than editing the current day.

If completed-day editing is implemented, all derived statistics must update automatically.

---

## 6. Main Screen

The main screen should prioritize today's activity.

Suggested structure:

```text
100 DAY CHALLENGE
Day 27 of 100

TODAY

             37
          items so far

        New number

[ +1 ]  [ +2 ]  [ +5 ]  [ Add… ]

Next unused:
38 (+1)
40 (+3)

[ Finish day ]


PROGRESS

Unique numbers
22 / 100

Items
1,963 / 5,050

[ Number grid ]
```

The exact visual design is not prescribed, but adding items should require very few interactions.

---

## 7. Challenge Completion

After 100 completed days, show a summary.

Example:

```text
CHALLENGE COMPLETE

100 days

5,384 items
Volume goal reached

93 / 100 unique numbers
93% number coverage

7 duplicate days

Missing numbers:
4, 17, 28, 52, 71, 86, 94
```

A perfect result is:

```text
100 / 100 unique numbers
5,050+ total items
```

The app should treat other results as valid completed challenges rather than failures.

---

## 8. Business Rules

1. A challenge contains 100 completed days.
2. Each completed day has one final daily total.
3. A daily total is calculated from the sum of its additions.
4. Duplicate daily totals are allowed.
5. Unique-number coverage considers values **1–100 only**.
6. A number counts toward unique coverage at most once.
7. Daily totals may exceed 100.
8. A daily total above 100 contributes to the 5,050-item goal but not to 1–100 unique-number coverage.
9. The cumulative item count is the sum of all completed daily totals.
10. The 5,050 target remains fixed regardless of duplicates.
11. Intermediate totals during a day have no effect on unique-number statistics.
12. A number becomes achieved only when the day is completed with that final total.

---

## 9. Suggested Data Model

A minimal representation could be:

```text
Challenge
- id
- startDate
- days[]

Day
- dayNumber
- date
- status
- additions[]
- completedAt

Addition
- id
- count
- createdAt
```

Derived values do not necessarily need to be stored:

```text
dailyTotal =
    sum(day.additions.count)

totalItems =
    sum(completedDays.dailyTotal)

usedNumbers =
    distinct completed daily totals where 1 <= total <= 100

uniqueNumberCount =
    count(usedNumbers)

missingNumbers =
    {1..100} - usedNumbers
```

Keeping these as derived state should reduce the risk of statistics becoming inconsistent after edits.

---

## 10. Prototype Scope

### Required for the first prototype

- create/start one challenge
- track day 1–100
- today's running count
- +1 / +2 / +5 shortcuts
- arbitrary item addition
- display individual additions
- edit/remove additions
- current daily total
- uniqueness indication
- nearest-unused-number suggestion
- complete a day even if its total is a duplicate
- 1–100 number grid
- unique-number count
- cumulative item count
- 5,050 progress
- history of completed days
- persistence between app launches
- final challenge summary

### Can be deferred

- accounts/login
- cloud sync
- sharing
- social features
- notifications
- multiple concurrent challenges
- photographs of discarded items
- categories
- detailed statistics
- achievements/gamification
- cross-device synchronization

---

## 11. UX Principles

The prototype should follow these principles:

- **Fast entry:** adding discarded items should take only one or two interactions.
- **Running tally:** the app should support many small additions throughout the day.
- **Encourage rather than enforce:** unique totals are a goal, not a validity constraint.
- **Make useful opportunities visible:** clearly show when one or a few additional items would reach an unused number.
- **Separate the goals:** display number coverage and total volume independently.
- **Avoid unnecessary ceremony:** recording "+3 items" should not require descriptions, categories, confirmation dialogs, or other metadata.
- **Recover from mistakes:** entries should be editable and preferably undoable.
- **Derived statistics:** totals and coverage should follow automatically from recorded daily entries rather than requiring manual management.