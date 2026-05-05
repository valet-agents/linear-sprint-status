# Daily Sprint Standup

The cron channel fires once on its schedule. There is no payload
to parse — your job is to run the standup workflow and post the
result to Slack.

## Steps

1. Follow the **Daily Standup Workflow** in SOUL.md (Phases 1–3):
   pull the active cycle from Linear, group issues by state, and
   compose the standup message.
2. Resolve target channels per the SOUL **Where to post** section:
   list every channel the bot is a member of and post once to each.
   If the bot is in zero channels, DM the workspace install user
   instead with the standup and a one-line invite hint.
3. Post exactly once per resolved destination. Do not retry on
   failure — log the error in your session and continue with the
   remaining destinations. The next cron fire is the recovery.
4. Do not send any follow-ups, reactions, or thread replies after
   the initial post. Your turn ends after the posts complete.

## Skip conditions

Skip posting (and stop silently) if any of these are true:

- The team has no active cycle (between cycles, or no cycles
  configured for the team).
- The active cycle has zero issues. (Post a single line `No issues
  in the active cycle.` to the resolved destinations is fine, but
  if the cycle itself doesn't exist, stay silent.)
