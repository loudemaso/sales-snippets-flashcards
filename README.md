# Sales Snippets Flashcards

A small, dependency-free flashcard app built from the **Sales Snippets** Notion database.

- Shows the Notion `Name` property on the front.
- Shows the Notion `Response` property on the back.
- Tap the card (or use the button) to flip it.
- Uses a shuffled deck, so every card appears once before any repeat.
- Supports the right-arrow key for the next card.

## Run locally

Open `index.html` directly, or serve the folder with any static web server.

## Publish with GitHub Pages

In the repository, open **Settings → Pages**, choose **Deploy from a branch**, select `main` and `/ (root)`, then save. The site will be available at:

`https://loudemaso.github.io/sales-snippets-flashcards/`

## Notion data

The app currently contains a snapshot of all 18 rows retrieved from the Sales Snippets database on September 13, 2026.

A public Notion page is not an unauthenticated Notion API endpoint. Live or scheduled updates require a Notion connection/PAT stored as a GitHub Actions secret, never in this public repository or in browser JavaScript. Until that sync is added, update the embedded `cards` array in `index.html` when the Notion database changes.
