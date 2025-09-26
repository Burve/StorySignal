# StorySignal

Automated, low-volume posting bot for my author account on X (and optionally Bluesky).  
It announces new RoyalRoad chapters and occasional promos, ~3–4 posts per day.

**Links**
- RoyalRoad: https://burve.app/r1/rrosotr  
- Contact: aleksandrs.ivancenko@gmail.com  
- Status: personal hobby project; open for transparency

---

## What it does
- Posts short updates with a link to my RoyalRoad novel.
- Can attach one image (cover/illustration) when available.
- Runs via n8n (cron/manual/RSS trigger), using X’s official API with **user-context OAuth**.

## What it does **not** do
- No automated replies, DMs, follows, likes, or scraping.
- No resale, sharing, or aggregation of X data.
- No posting on behalf of other users/accounts.

## Permissions & scopes
- App permissions: **Read and write** (no DM access requested).
- OAuth scopes requested: `tweet.write`, `tweet.read`, `users.read`, `offline.access` (for refresh tokens).

## Privacy & data use
- Stores only minimal operational data: my own post IDs, timestamps, and outbound link metadata (for logs/retries).
- No personal data about other users is collected, shared, or sold.
- Retention: logs are kept for short troubleshooting windows and then deleted.
- If you have questions or concerns, email **your@email**.

## Setup (summary)
> This section is here so reviewers understand how the app operates.

1. **Create an X Developer Project & App.**  
   - App type: *Web App, Automated App or Bot*  
   - Permissions: *Read and write*  
   - Callback URL: your n8n OAuth redirect URL (exact match)  
   - Website URL: this repository URL

2. **n8n workflow**  
   - Trigger: Cron/RSS/Webhook  
   - Build post text (hook + link + UTM)  
   - (Optional) upload image, then include media ID  
   - POST to `https://api.x.com/2/tweets` in user context  
   - Log tweet ID, time, and link

3. **Rate limits & volume**  
   - Targets ~90–120 posts/month (well under X’s free 500 writes/month cap).  
   - Backoff on errors; respect rate limits.

## Configuration (env/example)
```
X_CLIENT_ID=...
X_CLIENT_SECRET=...        # if using confidential client
X_REDIRECT_URI=https://<your-n8n-domain>/rest/oauth2-credential/callback
X_SCOPES=tweet.read tweet.write users.read offline.access

ROYALROAD_URL=https://burve.app/r1/rrosotr
DEFAULT_IMAGE_URL=https://...
```

## Transparency
This bot exists to make routine “new chapter” announcements easier while keeping **human** replies and engagement. If you see any issues, please open an issue or email **your@email**.

## License
MIT (or “All rights reserved” — your choice)

