# Snapchat Scraper: export to CSV or JSON with Python, Node.js or curl

Scrape Snapchat public creator profiles: name, subscribers, bio, and per-video Spotlight stats: views, likes, comments, shares, upload date, duration, thumbnail. Optionally the saved Highlights and the current Story with links to every photo and video. Input handles or profile URLs.

This repository has ready scripts for the [Snapchat Scraper: Spotlight, Highlights | $1.49 per 1,000](https://apify.com/v0iddo/snapchat-scraper) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv --max-items 50
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/snapchat-scraper), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "handles": [
    "kyliejenner",
    "daviddobrik"
  ],
  "includeHighlights": true,
  "maxItems": 20
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `handles` | array | Snapchat creator handles (e.g. |
| `includeHighlights` | boolean | Adds one row for each Highlight the creator saved on the profile and one for the Story posted in the last 24 hours, if any: title, number of snaps, newest sn... |
| `maxItems` | integer | The run stops after this many result rows. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "handle": "kyliejenner",
  "displayName": "King Kylie 👑",
  "subscriberCount": 28584800,
  "bio": null,
  "websiteUrl": "kyliecosmetics.com",
  "profilePictureUrl": "https://cf-st.sc-cdn.net/aps/bolt/aHR0cHM6Ly9jZi1zdC5zYy1jZG4ubmV0L2QvaEJwN3Z0ZFl5NFVOeFpnWk04dXBYP2JvPUVna3lBUVJJQWxBWllBRSUzRCZ1Yz0yNQ._RS0,90_FMjpeg",
  "rowType": "spotlight",
  "videoTitle": "new day in the life on my youtube",
  "caption": null,
  "viewCount": 80341,
  "likeCount": 3292,
  "commentCount": 190,
  "shareCount": null,
  "uploadDate": "2026-10-04",
  "durationSecs": 43,
  "keywords": [
    "bedtime",
    "relaxation",
    "sleep",
    "enjoyment",
    "boredom"
  ],
  "thumbnailUrl": "https://cf-st.sc-cdn.net/d/fP2DrQyUjHVZgWhE7sNy8.256.IRZXSOY?mo=GkYaCTIBBEgCUC5gAVCgAVoQRGZMYXJnZVRodW1ibmFpbKIBEAiAAiILEgAqB0lSWlhTT1miARAImgoiCxIAKgdJUlpYU09Z&uc=46",
  "url": "https://www.snapchat.com/@snapchat/spotlight/W7_EDlXWTBiXAEEniNoMPwAAYcWVmcHNqb2p5AaEEYCyMAaEEX5T1AAAAAQ",
  "highlightTitle": null,
  "snapCount": null,
  "mediaUrls": null
}
```

Fields: `handle`, `displayName`, `subscriberCount`, `bio`, `websiteUrl`, `profilePictureUrl`, `rowType`, `videoTitle`, `caption`, `viewCount`, `likeCount`, `commentCount`, `shareCount`, `uploadDate`, `durationSecs`, `keywords`, `thumbnailUrl`, `url`, `highlightTitle`, `snapCount`, `mediaUrls`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/snapchat-scraper').call({"handles": ["kyliejenner", "daviddobrik"], "includeHighlights": true, "maxItems": 20});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~snapchat-scraper/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"handles": ["kyliejenner", "daviddobrik"], "includeHighlights": true, "maxItems": 20}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$1.49 per 1,000 results**, nothing per run. `maxItems` caps the rows, and so the cost, of one run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
