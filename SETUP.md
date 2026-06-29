# Whiskey Raffle Setup

This repo contains the static landing page for the conference booth raffle.

## Files

- `index.html` — raffle entry form
- `logo.png` — convention logo image shown at the top of the page

## Before publishing

1. Upload `logo.png` into the repo root next to `index.html`.
2. Open `index.html` and replace:

```js
const ENDPOINT = "PASTE_YOUR_APPS_SCRIPT_URL_HERE";
```

with your deployed Google Apps Script Web App URL.

## Apps Script

Use this script in the Google Sheet's Apps Script editor:

```js
function doPost(e) {
  try {
    var data = JSON.parse(e.postData.contents);
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();

    if (sheet.getLastRow() === 0) {
      sheet.appendRow(["Timestamp", "First Name", "Last Name", "Work Email"]);
    }

    sheet.appendRow([
      new Date(),
      data.first || "",
      data.last || "",
      data.email || ""
    ]);

    return ContentService
      .createTextOutput(JSON.stringify({ result: "success" }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService
      .createTextOutput(JSON.stringify({ result: "error", message: err.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

## GitHub Pages

Once `index.html` and `logo.png` are in the default branch, GitHub Pages can publish from the root of `main`.

## Important note about private repos

GitHub Pages availability for private repositories depends on the organization plan and settings. Even when the repo is private, the published Pages site is typically public unless your GitHub plan supports private Pages and the org allows it.
