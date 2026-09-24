# How to Scrape BetExplorer Football Odds in Node.js

This example calls our [BetExplorer Scraper](https://apify.com/piotrv1001/betexplorer-scraper) on Apify. It does not implement a scraper from scratch.

## What this example does

- Requests one match from the Premier League page with bookmaker odds enabled
- Waits for the Actor run to finish
- Fetches the run's default dataset and prints the result
- Keeps average odds separate from bookmaker, market, outcome, and update time

## Prerequisites

- Node.js 18 or newer
- An Apify account and API token

## Installation

```bash
npm install
```

## Environment setup

Copy `.env.example` to `.env` and set `APIFY_TOKEN` to your token. Do not commit `.env`.

## Usage

```bash
npm start
```

## Code example

```js
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

// Initialize the ApifyClient with your Apify API token
// Set APIFY_TOKEN in your .env file (copy .env.example to get started)
const client = new ApifyClient({
    token: process.env.APIFY_TOKEN,
});

// Prepare Actor input
const input = {
    startUrls: [{ url: 'https://www.betexplorer.com/football/england/premier-league/' }],
    maxItems: 1,
    scrapeOdds: true,
};

// Run the Actor and wait for it to finish
const run = await client.actor('piotrv1001/betexplorer-scraper').call(input);

// Fetch and print Actor results from the run's dataset (if any)
console.log('Results from dataset');
console.log(`💾 Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
    console.dir(item);
});

// 📚 Want to learn more 📖? Go to → https://docs.apify.com/api/client/js/docs
```

## Example output

[`sample-output.json`](./sample-output.json) is abbreviated from our September 24, 2026 one-match run. It shows average 1X2 odds and one bookmaker's 1X2 outcomes; the actual record has more markets and prices. A league page changes over time, so your first match may differ. Some matches have no bookmaker quotes even when the option is on; inspect `odds` before using the result.

## Use cases

- Compare bookmaker prices for the same football match and market
- Export scores and average 1X2 odds for a league
- Retain quote timestamps for later review
- Build a match-level odds dataset without writing a scraper

## Try the Actor on Apify

**[Open the BetExplorer Scraper on Apify](https://apify.com/piotrv1001/betexplorer-scraper)**

## Related resources

- [How to export football odds from BetExplorer](https://www.falconscrape.com/blog/how-to-scrape-betexplorer-football-odds)

## License

MIT
