# Daily Career Radar Context Repo

This repo contains the context and operating instructions for Sia’s automated Daily Career Radar system.

The system runs as a Claude Code Remote Routine. Claude uses the context files in this repo to generate a daily career/news briefing, writes a plain text output file into Google Drive, and Zapier converts that file into an ElevenLabs audio briefing sent to Slack.

## Purpose

The Daily Career Radar should send Sia a useful, personalized briefing every morning with:

* A 5–8 minute podcast-style spoken script.
* A Slack-ready message.
* The top 3 best-fit jobs.
* Up to 15 total job recommendations.
* Direct apply links.
* Job match scores.
* Resume tailoring notes.
* Recruiter outreach suggestions.
* World, stock market, AI, and tech news.
* Word of the day.
* One concrete job-hunt mission.

The user-facing experience should be low-friction:

```text
Claude Routine
→ Google Drive .txt file
→ Zapier
→ ElevenLabs audio
→ Slack message with audio + full briefing
```

Sia should not need to manually open Google Drive, Google Docs, or Google Sheets every morning.

## Repository Structure

```text
career-radar-context/
  README.md
  CLAUDE.md
  docs/
    prd.md
    sia_personalization_context.md
    master_resume_summary.md
```

## File Responsibilities

### `CLAUDE.md`

Main operating instructions for Claude Code.

Claude should read this first. It defines:

* Daily task.
* Output format.
* Required markers for Zapier.
* Job search rules.
* News rules.
* Slack message requirements.
* Quality checks.

### `docs/prd.md`

Product requirements document.

This explains the overall system goal, delivery expectations, and low-friction user experience.

### `docs/sia_personalization_context.md`

Personalization file.

This tells Claude:

* Sia’s job preferences.
* Career positioning.
* News preferences.
* Industries to prioritize or avoid.
* Salary/location filters.
* Tone/personality for the audio briefing.
* Outreach style.
* Job fit scoring rules.

### `docs/master_resume_summary.md`

Condensed factual resume baseline.

This should summarize Sia’s strongest experience:

* Uniwise founding software engineer / backend & AI.
* Chalky founder and product engineer.
* AI Product Recommender.
* UX/product research.
* Voice-to-Text Refiner.
* ML/AI/RL/GAN projects.
* UCSD Cognitive Science, ML specialization, CS minor.
* Key technologies and domain strengths.

Do not store private API keys, Slack tokens, ElevenLabs keys, or passwords in this repo.

## Required Claude Routine Setup

Create a Claude Code Remote Routine.

Recommended schedule:

```text
Every day at 7:30 AM America/Los_Angeles
```

Expected tools/connectors:

* Indeed connector.
* Google Drive connector.
* Web/search/news access if available.

Expected Google Drive output folder:

```text
Daily Career Radar / Zapier Inbox
```

Claude should create one plain text file per run:

```text
career-radar-YYYY-MM-DD.txt
```

Example:

```text
career-radar-2026-06-08.txt
```

## Zapier Handoff Format

Zapier depends on exact text markers.

Claude must output exactly this structure:

```text
SPOKEN_SCRIPT_START
[5–8 minute spoken script. No raw URLs.]
SPOKEN_SCRIPT_END

SLACK_MESSAGE_START
[Complete Slack message with top 3 jobs, full job list, apply links, news, word of the day, and today’s mission.]
SLACK_MESSAGE_END
```

Do not add any text outside these markers.

## Zapier Flow

Recommended Zap:

```text
Google Drive: New File in Folder
→ Files by Zapier: Extract Text From File
→ Formatter: Extract SPOKEN_SCRIPT
→ Formatter: Extract SLACK_MESSAGE
→ ElevenLabs: Convert Text to Speech
→ Slack: Send audio + message
```

Regex for spoken script:

```regex
SPOKEN_SCRIPT_START([\s\S]*?)SPOKEN_SCRIPT_END
```

Regex for Slack message:

```regex
SLACK_MESSAGE_START([\s\S]*?)SLACK_MESSAGE_END
```

## Output Standards

The output should be useful even if the audio fails.

The Slack message should include enough information for Sia to act without opening any backend document.

Each daily briefing should include:

* Audio script.
* Full Slack briefing.
* Top 3 jobs.
* Up to 15 jobs total.
* Apply links.
* Match scores.
* Resume angle.
* Recruiter outreach suggestion.
* News snapshot.
* Word of the day.
* Today’s mission.

## Important Product Principle

Do not make the morning experience feel like a dashboard.

The briefing should be sent to Sia.

Slack should be the main daily interface. Google Drive is only the automation handoff layer.
