# Sales Snippets Flashcards

A small, dependency-free flashcard app built from the **Sales Snippets** Notion database.

- Shows the Notion `Name` property on the front.
- Shows the Notion `Response` property on the back.
- Tap the card (or use the button) to flip it.
- Uses a shuffled deck, so every card appears once before any repeat.
- Supports the right-arrow key for the next card.

## Live site

[Open Sales Snippets Flashcards](https://loudemaso.github.io/sales-snippets-flashcards/)

## Run locally

Open `index.html` directly, or serve the folder with any static web server.

## Notion data

The flashcards are stored as a snapshot in the `cards` array inside `index.html`. The source is the [Sales Snippets Notion database](https://app.notion.com/p/louisdemaso/eea9f5e7820f4cfe917b8b236bf596d2?v=9f3102a6cdff4494846e9947f3c7f6c9).

**Last successful Notion sync:** `2026-09-13T20:36:37.510Z` (UTC)

<!-- notion-sync-timestamp: 2026-09-13T20:36:37.510Z -->

### Instructions for an authenticated agent

When an agent is connected to Louis DeMaso's Notion workspace and asked to refresh the flashcards:

1. Read the ISO-8601 timestamp from the `notion-sync-timestamp` comment above. Treat it as the exclusive lower bound for changed records.
2. Record a new UTC `syncStartedAt` timestamp **before** querying Notion. Do not write this new timestamp to the README yet.
3. Fetch the Sales Snippets database from the source link above and verify that its data source still contains `Name`, `Response`, `Type`, and `Last Modified`. `Last Modified` must be a Notion `last_edited_time` property.
4. Compare each non-archived row's `Last Modified` value with the saved timestamp. Only a row with `Last Modified > saved timestamp` needs its card content reprocessed.
   - A lightweight metadata query can first retrieve `url`, `createdTime`, `Name`, and `Last Modified`.
   - Fetch faithful rich-text values for `Response` only for changed rows.
   - Follow pagination until all matching rows have been considered.
5. Reconcile the lightweight current-row list against the existing card names and count. If a row was deleted or archived, or an existing row's `Name` changed so it cannot be matched safely, fall back to a full rebuild of the array. This prevents stale or duplicate cards.
6. For each changed row, map the properties as follows:
   - `Name` → `name`
   - `Response` → `response`
   - `Type` → `type`
7. Preserve the wording and punctuation from Notion exactly. Convert Notion line-break markup such as `<br>` to newline characters for display, but do not rewrite or summarize the content.
8. Open `index.html` and update the affected objects inside `const cards = [...];`. For a full rebuild, replace the complete array. Do not alter the surrounding HTML, CSS, or application logic unless separately requested.
9. Do not include Notion page URLs, internal IDs, timestamps, or other properties in the card array. Each object must use only this structure:

   ```js
   {
     "name": "The Name value from Notion",
     "type": "The Type value from Notion",
     "response": "The Response value from Notion"
   }
   ```

10. Validate before committing:
    - Confirm the JavaScript parses successfully.
    - Confirm the card count matches the current number of non-archived Notion rows.
    - Confirm every object has non-empty `name`, `type`, and `response` values.
    - Confirm the app can show a prompt, flip to its response, and advance to another card.
11. After—and only after—the Notion query, any required card update, and validation all succeed, replace both README timestamp occurrences with the recorded `syncStartedAt` value. Using the query-start time instead of the finish time ensures that an edit made during the sync will be reconsidered next time.
12. Commit the changed files with a concise message such as `Refresh flashcards from Notion`. If no cards changed, commit only the advanced README timestamp. If any step fails, leave the previous timestamp unchanged.

### Credential safety

**Notion access tokens, integration secrets, personal access tokens, authorization headers, cookies, session data, and other credentials must NEVER be stored anywhere in this repository.** This includes source files, browser JavaScript, configuration files, commit history, pull requests, issues, comments, or logs.

An agent should use only the credentials already provided securely by its authenticated Notion connection. If automated syncing is added later, credentials must be stored as encrypted GitHub Actions secrets and read only at runtime. Never print, return, expose, or commit their values.
