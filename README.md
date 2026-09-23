# Skiddle Ticket Availability Watcher

A small personal-use Python script that polls the [Skiddle API](https://www.skiddle.com/api/) for
specific events and sends a push notification (via [ntfy.sh](https://ntfy.sh)) the moment tickets
become available for purchase.

## What it does

- Checks one or more Skiddle event IDs on a schedule (e.g. every 5 minutes via cron)
- Tracks previous ticket-availability state between runs, so you're only notified on a *change*
  (unavailable → available), not on every single check
- Sends a free push notification to your phone or desktop via ntfy.sh when tickets appear

## Why

I wanted to get tickets to specific events as soon as they go on sale, without manually refreshing
the page. This is a personal tool for tracking events I want to attend — not a scraper or resale
tool.

## Setup

1. Apply for a free Skiddle API key: https://www.skiddle.com/api/join.php
2. Install dependencies:
   ```
   pip install requests
   ```
3. Edit the `CONFIG` section at the top of `skiddle_ticket_watcher.py` with:
   - your Skiddle API key
   - the Skiddle event ID(s) you want to watch
   - an ntfy.sh topic name (any unguessable string you make up)
4. Subscribe to your ntfy topic in the [ntfy app](https://ntfy.sh/app) or at
   `https://ntfy.sh/<your-topic>` in a browser
5. Run once manually to confirm it works and to sanity-check the raw API response:
   ```
   python skiddle_ticket_watcher.py
   ```
6. Schedule it to run every few minutes with cron (macOS/Linux) or Task Scheduler (Windows) —
   see the comments at the bottom of the script for exact commands.

## Notes

- Uses only Skiddle's public, documented REST endpoints — no scraping.
- Polls on a schedule (not a tight loop) to stay well within reasonable API usage.
