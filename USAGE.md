# USAGE — StorySignal

This guide walks you from “nothing” to a working n8n workflow that posts to X using your app’s user‑context OAuth. It matches the setup you’ve already started.

---

## Prerequisites
- An X account (the one that will post).
- X Developer **Project + App** created.
- A public page for **Website URL** (your GitHub repo is perfect).
- **n8n** running at an HTTPS URL (e.g., `https://n8n.example.com`).

> **Tip:** Keep **App permissions** at **Read and write** (no DM). Requesting DM requires extra justification.

---

## 1) Configure your X App (Developer Portal)
1. **Project/App purpose:** *Making a bot* (or similar).
2. **App permissions:** *Read and write*.
3. **Type of app:** *Web App, Automated App or Bot*.
4. **Website URL:** link to this GitHub repository.
5. **Callback URL:** your n8n OAuth redirect (exact match, no extra slashes):
   ```
   https://<your-n8n-domain>/rest/oauth2-credential/callback
   ```
6. **OAuth 2.0 scopes** (minimum):
   - `tweet.write`
   - `tweet.read`
   - `users.read`
   - `offline.access` (for refresh tokens)
7. **Save** and note your **Client ID** (and **Client Secret** if you created a confidential client).

> If the portal asks for endpoints later, the standard ones are:
> - Authorization URL: `https://x.com/i/oauth2/authorize` (or `https://twitter.com/i/oauth2/authorize`)
> - Token URL: `https://api.x.com/2/oauth2/token` (or `https://api.twitter.com/2/oauth2/token`)

---

## 2) Create an OAuth2 credential in n8n
1. In **n8n → Credentials → New → OAuth2 API**.
2. Fill in:
   - **Authorization URL:** `https://x.com/i/oauth2/authorize`
   - **Token URL:** `https://api.x.com/2/oauth2/token`
   - **Client ID/Secret:** from your X App (Secret only if confidential client).
   - **Scope:** `tweet.read tweet.write users.read offline.access`
   - **Auth URL Parameters (optional):** `response_type=code&code_challenge_method=S256`
   - **Authentication:** Authorization Code (PKCE if offered).
   - **Redirect URL:** n8n will display this — copy it into your X App **Callback URL** (must match exactly).
3. Click **Connect OAuth2** and complete the consent screen while logged in to the **posting** account. n8n should store access + refresh tokens.

---

## 3) Minimal “Post a Tweet” workflow in n8n
1. **Manual Trigger** node.
2. **Set** (or **Function**) node — build your text:
   - Example text: `Chapter live — dark tunnels & darker deals. Read: https://burve.app/r1/rrosotr`
3. **HTTP Request** node:
   - **Method:** `POST`
   - **URL:** `https://api.x.com/2/tweets`
   - **Authentication:** OAuth2
   - **OAuth2 Credential:** select the one you created
   - **Send:** JSON
   - **Body:**
     ```json
     {
       "text": "Chapter is live — read now: https://burve.app/r1/rrosotr"
     }
     ```
   - **Headers:** `Content-Type: application/json`
4. **Execute** (from the Manual Trigger). If successful, you’ll get a JSON response with the created tweet ID.

### Optional: attach an image
1. Add an **HTTP Request** node *before* `POST /2/tweets` to upload media (v2 media upload).
2. Use the returned `media_id` in your tweet body:
   ```json
   {
     "text": "New chapter is live!",
     "media": { "media_ids": ["<MEDIA_ID>"] }
   }
   ```

> Keep posts ≤ 280 characters. Links count using t.co rules; ultra‑long text will 422.

---

## 4) From manual to automatic
Pick one trigger and wire it into your HTTP Request node:
- **Cron** — e.g., 10:00, 14:00, 19:00 daily.
- **RSS Read** — watch your release feed and post on new item.
- **Webhook** — submit a small form from your browser when you want to post.

Optional nodes:
- **Google Sheets/Airtable** — log tweet IDs, timestamps, and URLs.
- **If** — avoid duplicate posts; check against last posted URL.
- **Wait** — stagger secondary reminders a few hours after the first post.

---

## 5) Good practices
- Keep text varied; don’t post the exact same phrasing every time.
- 1–2 relevant hashtags max.
- Use UTM tags on your links to track CTR in analytics.
- Handle errors with backoff; respect write limits (Free tier: 500 writes/month per app & per user).

---

## 6) Troubleshooting
- **401 / invalid_scope** — scopes in n8n don’t match the App; re‑authorize after fixing.
- **Callback URL mismatch** — the URL in your App must exactly match n8n’s redirect.
- **403 / write limit exceeded** — you hit monthly write cap; reduce frequency or upgrade.
- **422 / unprocessable** — text too long or bad body; simplify payload to `{ "text": "..." }`.
- **429 / rate limit** — slow down; add retry with exponential backoff.

---

## 7) (Later) Add Bluesky (optional)
- Create an **app password** in Bluesky.
- In n8n, use **HTTP Request** to call `com.atproto.repo.createRecord` with your text and link.
- If adding an image, upload the blob first and reference it in the record’s `embed`.

---

## 8) Repository structure (suggested)
```
/README.md        # overview
/PRIVACY.md       # privacy policy
/USAGE.md         # this file
/workflow/
  StorySignal.json  # n8n export (optional)
ENV.example       # sample env vars (optional)
```

---

## License
MIT (selected). Add a `LICENSE` file in the repo if you haven’t already and paste the MIT text from GitHub’s template.

