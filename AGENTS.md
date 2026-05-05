This folder contains the source for a Skilled Agent originally built for the Valet runtime. Changes should follow the Skilled Agent open standard.

## Setup

### Connectors

- **stripe-mcp**: The Stripe MCP server. The agent uses it to read invoices and customers, surface past-due invoices, detect duplicates and outsized amounts, and answer ad-hoc invoice questions in Slack. Voiding, cancelling, or marking an invoice paid only happens when explicitly confirmed in Slack. Add it from the catalog at the org level so other Stripe-powered agents (e.g. `stripe-revenue-health`) can share it.

### Channels

- **slack** (slack): The agent's per-agent Slack bot. Listens for @mentions and replies in-thread, and posts the daily AP digest to whichever channels the bot has been invited to. Slack writes use the auto-injected outbound Slack connector.
- **cron** (cron): Fires the daily AP digest at 8am Pacific, Monday through Friday (`0 8 * * 1-5`, `America/Los_Angeles`). Declared inline in `valet.yaml`, so it's created automatically by the dashboard setup flow.

### Secrets

- **STRIPE_SECRET_KEY**: A Stripe API key — either a live secret key (`sk_live_...`) or, strongly recommended, a Restricted Key (`rk_live_...`) with read-only scopes on Invoices and Customers. Generate at <https://dashboard.stripe.com/apikeys>. The restricted key is the safest default — this agent never needs write access by default, and write actions (void, cancel, mark paid) only fire when a human confirms in Slack, so granting more than read is unnecessary unless you want to enable those.

### External Setup

1. After deploy, invite the agent's Slack bot to whichever channel(s) you want the daily AP digest in (typically a finance, ops, or AP channel). The agent posts to every channel it's a member of — invite it to one focused channel, or several. If the bot has not been invited anywhere, the digest is sent as a DM to the workspace install user with a one-line nudge to invite it somewhere.
2. Invite the bot to any additional channels where teammates should be able to @mention it for ad-hoc invoice questions (e.g. *"what's the total open AP for Acme?"*).
3. The first cron fire is the next 8am Pacific weekday after deploy. To smoke-test sooner, @mention the bot in Slack with a question like *"who's behind on payment this week?"* — that exercises the Slack + Stripe path without waiting for the cron.

## Customizing

- **Change the schedule**: edit the `cron` and `timezone` on the `cron` channel in `valet.yaml`, then redeploy. The default `0 8 * * 1-5` America/Los_Angeles is tuned to land before most teams' workday starts.
- **Anomaly thresholds**: SOUL.md uses a 2x trailing-90-day mean as the "outsized amount" trigger and a 7-day window for duplicate detection. To change these, edit the **Anomaly detection** subsection in SOUL.md — both numbers live in plain English and are honored by the workflow.
- **Control where the digest posts**: invite or remove the bot from channels in Slack — that's the only signal the agent uses. There is no channel name in the configuration.
- **Enable write actions**: write actions (void, cancel, mark paid) only fire on explicit Slack confirmation, but they require the secret to have the matching write scope. If you want them, swap the restricted key for one with `Invoices: write` (still no need for raw card data access).
