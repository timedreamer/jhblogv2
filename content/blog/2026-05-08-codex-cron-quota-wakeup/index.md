---
title: "Start Codex CLI quota periods automatically with cron"
date: 2026-05-08
author: "Ji Huang"
tags: ["codex", "cron", "ubuntu"]
---

I use Codex CLI on an Ubuntu server. The quota window is 5 hours, so I trigger a tiny Codex request at **07:30, 12:30, and 17:30** to get three full windows per day. You can use a similar cron job for Claude Code too.

The solution is a small shell script plus a user cron job.

## Create the wakeup script

Create the job folder and copy and paste this whole block into the terminal:

```bash
mkdir -p ~/project/misc/codex_timely_job
cat > ~/project/misc/codex_timely_job/codex-wakeup.sh <<'EOF'
#!/usr/bin/env bash

JOB_DIR="$HOME/project/misc/codex_timely_job"
LOG="$JOB_DIR/log.txt"

{
  echo "===== START $(date '+%Y-%m-%d %H:%M:%S %Z') ====="

  cd "$JOB_DIR"

  "$HOME/.npm-packages/bin/codex" exec \
    --model gpt-5.4-mini \
    -c model_reasoning_effort=\"low\" \
    --skip-git-repo-check \
    --ephemeral \
    "hi"

  echo "===== END $(date '+%Y-%m-%d %H:%M:%S %Z') ====="
  echo
} >> "$LOG" 2>&1
EOF

chmod +x ~/project/misc/codex_timely_job/codex-wakeup.sh
```

## Test it manually

```bash
~/project/misc/codex_timely_job/codex-wakeup.sh
tail -n 50 ~/project/misc/codex_timely_job/log.txt
```

The timestamp is important. Without it, it is hard to tell whether cron actually ran the job.

Example log:

```text
===== START 2026-05-08 17:30:01 EDT =====
...
===== END 2026-05-08 17:30:10 EDT =====
```

## Add the cron job

Open crontab:

```bash
crontab -e
```

Add this line:

```cron
30 7,12,17 * * * /home/YOUR_USER/project/misc/codex_timely_job/codex-wakeup.sh
```

This runs the script at 07:30, 12:30, and 17:30 every day.

## Verify

List the installed cron jobs:

```bash
crontab -l
```

Check the server time:

```bash
date
```

Cron uses the server's timezone, not the laptop timezone.

After the next scheduled run, check the log:

```bash
tail -n 50 ~/project/misc/codex_timely_job/log.txt
```

## Notes

I first tried `model_reasoning_effort="minimal"`, but it failed because my Codex session had tools attached that were incompatible with minimal effort. Using `low` worked.

The `--ephemeral` flag keeps the run lightweight. The prompt is only `hi`, so the request is intentionally tiny.

No cron restart is needed after editing the script. Cron will use the updated script the next time it runs.
