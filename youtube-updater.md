---
name: youtube-updater
description: "Scan the Sales Show YouTube channel, find videos missing SEO optimization (search-friendly titles, descriptions, tags, timestamps), fix them via the YouTube Data API. Run weekly or on demand."
---

# YouTube Updater

Scan and optimize the Sales Show YouTube channel (@salesshow) for search discoverability.

## What this skill does

1. **Authenticate** with YouTube Data API using the stored OAuth refresh token
2. **Scan** all videos on the channel for missing or weak SEO signals
3. **Fix titles** to lead with search terms people actually type (e.g., "How to close more deals" not "The Secret to Closing Any Deal")
4. **Fix descriptions** so the first line contains the primary search phrase
5. **Add tags** matching real YouTube search queries for B2B founders at $1M-$5M
6. **Generate timestamps** by fetching auto-generated transcripts and identifying topic shifts
7. **Push updates** via the YouTube Data API

## How to run it

When the user says `/youtube-updater` or this skill is triggered by a cron job:

### Step 1: Get a fresh access token

Use the OAuth credentials stored in Claude memory (YouTube Data API v3 section) to request a fresh access token from `https://oauth2.googleapis.com/token` using the refresh token grant.

### Step 2: List all videos on the channel

Channel ID: stored in Claude memory.

```
GET https://www.googleapis.com/youtube/v3/search?part=id&channelId={CHANNEL_ID}&type=video&order=date&maxResults=50
```

Then for each video ID:
```
GET https://www.googleapis.com/youtube/v3/videos?part=snippet,statistics&id={VIDEO_ID}
```

### Step 3: Identify videos needing optimization

A video needs optimization if ANY of these are true:
- Title does not start with a search phrase (starts with a name, a clever phrase, or an emoji)
- Description first line does not contain the search phrase from the title
- Tags are empty or fewer than 4
- Description has no timestamps (no lines matching `XX:XX`)

### Step 4: Optimize each video

**Title rules:**
- Lead with what people search: "How to...", "Why...", "B2B sales tips..."
- Guest name goes in parentheses at the end, not the beginning
- Keep under 60 characters
- No emojis in titles

**Description rules:**
- First line = the search phrase expanded into a sentence
- Include 3-5 bullet points of what the viewer will learn
- Add timestamps if missing (see Step 5)
- End with the standard CTA block:

```
Join the Sprint Club (free 7-day trial): https://www.strategysprints.com
Free sales tools: https://www.strategysprints.com/tools
Newsletter: https://thesalesshow.beehiiv.com

CONNECT
X: https://x.com/simonseverino
LinkedIn: https://at.linkedin.com/in/simonseverino
```

**Tags:**
- 5-8 tags per video
- Use actual search terms, not clever phrases
- Include: the main topic, variations, guest name if applicable, "b2b sales"

### Step 5: Generate timestamps (if missing)

1. Use `youtube_transcript_api` Python package to fetch auto-generated captions
2. Segment transcript into 2-minute chunks
3. Analyze each chunk to identify topic shifts and key discussion points
4. Create descriptive chapter titles (not raw transcript snippets)
5. Format as `MM:SS Topic description`
6. Insert into the description after the intro paragraph

### Step 6: Push updates

```
PUT https://www.googleapis.com/youtube/v3/videos?part=snippet
Authorization: Bearer {ACCESS_TOKEN}
Content-Type: application/json

{
  "id": "{VIDEO_ID}",
  "snippet": {
    "title": "...",
    "description": "...",
    "tags": [...],
    "categoryId": "27"
  }
}
```

### Step 7: Report

Post a summary to Slack #general via Make.com or print to terminal:
- How many videos scanned
- How many updated
- What was changed on each

## Target audience for SEO
B2B founders of agencies, consultancies, and SaaS companies at $1M-$5M revenue. They search for:
- "how to close more deals"
- "b2b sales process"
- "how to get more clients"
- "shorten sales cycle"
- "sales strategy for consultants"
- "how to raise prices"
- "founder productivity"
- "scale without hiring"

## Credentials

OAuth credentials (Client ID, Client Secret, Refresh Token, Channel ID) are stored in Claude Code memory under **API Keys > YouTube Data API v3**. Do not hardcode them in this file.
