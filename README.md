# The SEO Skill That Fixed 159 Videos While You Slept

Scans your YouTube channel. Finds videos with weak titles, missing timestamps, empty tags. Fixes them via the YouTube Data API. Runs weekly on autopilot.

---

## The problem

You published 159 videos. Most have clever titles that nobody searches for. Descriptions that say nothing useful in the first line. No timestamps. No tags. Every video is invisible to YouTube search.

Fixing them manually.. 159 videos, 15 minutes each.. that's 40 hours of tedious work.

## What this skill does instead

One command. Every video on your channel scanned and optimized.

- **Titles** rewritten to lead with what people actually type into YouTube search
- **Descriptions** restructured so the first line contains the primary search phrase
- **Tags** added matching real search queries for B2B founders at $1M-$5M
- **Timestamps** auto-generated from transcripts
- **Updates pushed** via the YouTube Data API

Runs weekly via launchd. Or on demand with `/youtube-updater`.

---

## The rules it follows

**Titles:**
- Lead with search terms people type.. "How to...", "Why...", "B2B sales tips..."
- Guest name in parentheses at the end, not the beginning
- Under 60 characters. No emojis.

**Descriptions:**
- First line = the search phrase expanded into a sentence
- 3-5 bullet points of what the viewer will learn
- Timestamps if missing
- Standard CTA block at the end

**Tags:**
- 5-8 per video
- Actual search terms, not clever phrases
- Always includes: main topic, variations, guest name, "b2b sales"

---

## Usage

```
/youtube-updater
```

Scans both channels (The Sales Show + The Investing Show), identifies videos needing optimization, fixes them, and reports what changed.

### Install

```bash
git clone https://github.com/SimonTheSalesBooster/youtube-updater.git
cp youtube-updater/youtube-updater.md ~/.claude/commands/
```

### Requirements

- Claude Code with YouTube Data API v3 OAuth credentials
- Google OAuth refresh token with `youtube` and `youtube.force-ssl` scopes

---

## About

Built by Simon Severino.. author of *Strategy Sprints* and *Time Freedom* with Jay Abraham. Added over $2 Billion in sales to B2B clients in finance, software, and consulting.

[strategysprints.com](https://strategysprints.com) | [The Sales Show](https://youtube.com/@salesshow)

---

*keep rolling, Simon & The Sprinters*
