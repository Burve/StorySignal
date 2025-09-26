# Privacy Policy — StorySignal

_Last updated: 2025-09-27_

StorySignal is a small, personal automation that posts updates about my RoyalRoad novel to X (and optionally Bluesky). This document explains what data is used and how.

## What StorySignal does
- Creates original posts from my author account on a schedule or when a new chapter releases.
- Optionally attaches a single image (cover/illustration).
- Runs via n8n using official platform APIs with **user-context** authentication.

## Data StorySignal collects/handles
- **Operational logs (minimal):**
  - IDs of my own posts (tweet IDs), timestamps, and the outbound link that was posted (including UTM parameters).
  - Basic job status (success/error messages) for troubleshooting.
- **No third‑party personal data:**
  - StorySignal does **not** collect, store, or process other users’ personal data.
  - It does **not** scrape timelines, followers, DMs, or analytics from X/Bluesky.

## Data sources
- Content I provide (post text, link, optional image URL).
- Platform responses from X/Bluesky limited to the result of my own post action (e.g., created post ID).

## Storage & retention
- Logs are stored inside my automation environment (e.g., n8n + its database or a connected sheet) and used only for reliability and accounting.
- Retention is short: routine logs may be deleted automatically after troubleshooting windows.
- Backups, if any, are for disaster recovery only.

## Sharing & selling
- StorySignal does **not** sell, rent, or share data with third parties.
- No advertising partners or data brokers are involved.

## Permissions & scopes
- X OAuth scopes requested: `tweet.write`, `tweet.read`, `users.read`, `offline.access` (refresh tokens).
- No DM access is requested. If that changes, this policy will be updated first.

## Security
- Credentials (API keys/tokens) are stored in environment variables or n8n credentials with restricted access.
- I rotate/revoke tokens if suspicious activity is detected.

## Children’s data
- StorySignal is not directed to children and does not knowingly collect information from anyone under 16.

## Contact
If you have privacy questions or concerns, contact: **aleksandrs.ivancenko@gmail.com**.

## Changes to this policy
If StorySignal’s functionality or data practices change, this document will be updated and the repository history will reflect the changes.

