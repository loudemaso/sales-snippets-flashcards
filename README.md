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

### Instructions for an authenticated agent

When an agent is connected to Louis DeMaso's Notion workspace and asked to refresh the flashcards:

1. Fetch the Sales Snippets database from the source link above and identify its underlying data source.
2. Query all non-archived rows. Follow pagination until no additional rows remain.
3. Read these properties from every row:
   - `Name` → `name`
   - `Response` → `response`
   - `Type` → `type`
4. Preserve the wording and punctuation from Notion exactly. Convert Notion line-break markup such as `<br>` to newline characters for display, but do not rewrite or summarize the content.
5. Open `index.html` and replace the complete `const cards = [...];` array. Do not alter the surrounding HTML, CSS, or application logic unless separately requested.
6. Do not include Notion page URLs, internal IDs, timestamps, or other properties in the array. Each object should use only this structure:

   ```js
   {
     "name": "The Name value from Notion",
     "type": "The Type value from Notion",
     "response": "The Response value from Notion"
   }
   ```

7. Validate the updated file before committing:
   - Confirm the JavaScript parses successfully.
   - Confirm the number of objects equals the number of retrieved Notion rows.
   - Confirm every object has non-empty `name`, `type`, and `response` values.
   - Confirm the app can show a prompt, flip to its response, and advance to another card.
8. Commit the updated `index.html` with a concise message such as `Refresh flashcards from Notion`.

### Credential safety

**Notion access tokens, integration secrets, personal access tokens, authorization headers, cookies, session data, and other credentials must NEVER be stored anywhere in this repository.** This includes source files, browser JavaScript, configuration files, commit history, pull requests, issues, comments, or logs.

An agent should use only the credentials already provided securely by its authenticated Notion connection. If automated syncing is added later, credentials must be stored as encrypted GitHub Actions secrets and read only at runtime. Never print, return, expose, or commit their values.
