# Adaptative Sharding

Adaptative sharding is an automatic process that creates refined LinkedIn Ad Library jobs when a completed job is too broad.

The objective is to reduce oversized result sets and improve scraping precision by splitting one broad query into multiple keyword-focused queries.

## Overview

Run sharding with:

```bash
export RUBYLIB=~/code1/slave && \
cd ~/code1/slave/extensions/mass.subaccount/p && \
ruby plan.rb run-once=yes
```

## When a Job Is Split

A job is considered for splitting when:

- The job belongs to a LinkedIn Ad Library source.
- The job is performed (`status = 2`).
- `job.total_results > source.event_limit`.

## How Keywords Are Produced

For each candidate job:

1. Load all related events.
2. Build distinct `(id_lead, id_company)` pairs.
3. Extract keywords from event content using normalization:
	- lowercase
	- punctuation removal
	- stopword filtering
	- light stemming
4. Count how many distinct pairs contain each keyword.

## Keyword Selection Rules

- Primary rule: keep keywords with count `>= 5%` of total distinct pairs.
- Low-diversity fallback: if none match primary rule, pick top deterministic keywords by frequency (up to 5), with a minimum support floor.

This fallback avoids empty shard generation when language is sparse but there is still useful signal.

## Job Generation

For each selected keyword:

- Start from original job URL.
- Append keyword into search query param (`keyword` or `q`).
- Create a new pending job (`status = 0`) for the same source/account.

## Safety and Determinism

- Existing URLs are checked to avoid duplicates.
- URLs planned in the same run are deduplicated.
- Keywords are ranked deterministically (frequency, then alphabetical).

## Expected Outcome

Each run can generate zero or more refined jobs.

- `0 jobs` means no split candidates or insufficient signal.
- `N jobs` means sharding was applied successfully and new pending jobs were queued.

