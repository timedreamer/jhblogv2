---
title: Building a Humanizer web app with Claude Code and DeepSeek V4
author: Ji Huang
date: '2026-05-04'
categories:
    - misc
tags:
    - AI
    - claude
    - deepseek
    - streamlit
    - python
    - coding
description: A weekend project to build a Streamlit-based Humanizer web app using Claude Code powered by DeepSeek V4. Three hours, $0.33 in API costs, 12.9M tokens processed.
---

I use the [Humanizer skill](https://github.com/obra/superpowers/blob/main/skills/humanizer/SKILL.md) a lot, but sometimes I just have a short piece of text to clean up and opening a terminal feels like overkill.

DeepSeek V4 dropped last week. The API pricing is aggressively cheap, and I'd seen good write-ups about pairing it with Claude Code. I built a web version of the Humanizer over the weekend.

Three hours total. Thirty-three cents in API costs.

## The plan

I used [planning-with-files](https://github.com/othmanadi/planning-with-files) and the [brainstorming skill](https://github.com/obra/superpowers/blob/main/skills/brainstorming/SKILL.md) to think through what I actually wanted before writing any code. The answer was Streamlit: pure Python, free hosting on Streamlit Community Cloud, and password protection out of the box. No React, no build pipelines.

## Building with CC + DeepSeek V4

The whole thing went through several rounds in Claude Code. For the most part it worked, but DeepSeek V4 had a few confidence-over-capability moments. It suggested a one-click deployment button to Streamlit Cloud, that feature doesn't exist. It also kept proposing front-end styling improvements that would require custom JS injection, which is hard to implement in Streamlit, so I skipped them entirely.

These weren't blockers. They just meant more pushback than I'd need with Claude.

## How does DeepSeek V4 hold up?

It's not at Opus level. My rough sense is it sits somewhere around Claude Sonnet 4.5 in capability good enough to build something real, but it fills in gaps more aggressively than I'd like.

That said, $0.33 for a full weekend project is hard to argue with. The Claude Pro $20 plan burns fast. An active coding session can chew through a 5-hour window in under an hour. DeepSeek's pricing, especially with the [current discount](https://x.com/deepseek_ai/status/2049312932014813344), makes exploration a lot cheaper.

The cache hit ratio was stable at 98.6% across both days of work.

| Metric | Value |
|---|---|
| Total cost | $0.33 |
| Total input tokens | 12,912,053 |
| Cache-hit tokens | 12,727,680 |
| Cache-hit ratio | ~98.6% |

## The result

The app is live on Streamlit Community Cloud. The repo is at [timedreamer/humanizer_web](https://github.com/timedreamer/humanizer_web). If you use the Humanizer skill and want a quick paste-and-go option without opening a terminal, it does the job.

For $0.33 and a Saturday afternoon, I'm pretty happy with how CC + DeepSeek V4 held up.

The screenshot of the Streamlit app.

![humanizer web app screenshot](screenshot.png)


