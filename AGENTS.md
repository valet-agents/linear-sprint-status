This folder contains the source for a Skilled Agent originally built for the Valet runtime. Changes should follow the Skilled Agent open standard.

## Setup

### Connectors

- **linear-mcp**: The Linear MCP server, OAuth-authenticated. The agent uses it to read the active cycle, list and filter issues, follow blocking relations, and (when explicitly asked in Slack) create or update issues. Add it from the catalog at the org level so other Linear-powered agents can share it.

### Channels

- **slack** (slack): The agent's per-agent Slack bot. Listens for @mentions and replies in-thread, and posts the daily standup to whichever channels the bot has been invited to. Slack writes use the auto-injected outbound Slack connector.
- **cron** (cron): Fires the daily sprint standup at 10am Pacific, Monday through Friday (`0 10 * * 1-5`, `America/Los_Angeles`). Declared inline in `valet.yaml`, so it's created automatically by the dashboard setup flow.

### Secrets

This agent uses the OAuth variant of the Linear MCP, so no API token is needed at the org or agent level. The OAuth grant happens in the dashboard setup flow when you connect Linear.

### External Setup

1. After deploy, invite the agent's Slack bot to whichever channel(s) you want the daily standup in. The agent posts the standup to every channel it's a member of — invite it to one focused channel, or several. If the bot has not been invited anywhere, the standup is sent as a DM to the workspace install user with a one-line nudge to invite it somewhere.
2. Invite the bot to any additional channels where teammates should be able to @mention it for ad-hoc Linear questions (e.g. an engineering channel for "what's at risk this sprint?" follow-ups).
3. The first cron fire is the next 10am Pacific weekday after deploy. To smoke-test sooner, @mention the bot in Slack with a question like *"what's at risk this sprint?"* — that exercises the Slack + Linear path without waiting for the cron.

## Customizing

- **Change the schedule**: edit the `cron` and `timezone` on the `cron` channel in `valet.yaml`, then redeploy.
- **Control where the standup posts**: invite or remove the bot from channels in Slack — that's the only signal the agent uses. There is no channel name in the configuration.
- **Watch a specific Linear team**: if you have multiple teams with active cycles, set a `TEAM_KEY` env var (e.g. `ENG`) on the agent. The SOUL workflow honors it and falls back to "the first team with an active cycle" when it's unset.
