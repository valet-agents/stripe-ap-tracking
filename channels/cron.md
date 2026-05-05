# Daily AP Digest

The cron channel fires once on its schedule. There is no payload
to parse — your job is to run the AP workflow and post the result
to Slack.

## Steps

1. Follow the **Daily AP Workflow** in SOUL.md (Phases 1–3): pull
   past-due invoices, yesterday's new invoices, and the trailing
   90-day baseline for anomaly detection. Compose the digest with
   the Past due / New invoices yesterday / Anomalies sections.
2. Resolve target channels per the SOUL **Where to post** section:
   list every channel the bot is a member of and post once to each.
   If the bot is in zero channels, DM the workspace install user
   instead with the digest and a one-line invite hint.
3. Post exactly once per resolved destination. Do not retry on
   failure — log the error in your session and continue with the
   remaining destinations. The next cron fire is the recovery.
4. Do not send any follow-ups, reactions, or thread replies after
   the initial post. Your turn ends after the posts complete.

## Skip conditions

Skip posting (and stop silently) if any of these are true:

- All three sections (past-due, new yesterday, anomalies) are
  empty. Nothing to say — stay quiet rather than spam an empty
  digest. (Optional: if you'd rather post a one-liner heartbeat,
  the SOUL allows `No AP activity. Nothing past due, no new
  invoices, no anomalies.` — default is silent.)
- Stripe is not configured, returns an auth error, or the
  `STRIPE_SECRET_KEY` slot is missing. In that case, DM the
  workspace install user with a one-line hint: *"I can't reach
  Stripe — add or fix `STRIPE_SECRET_KEY` in the agent settings."*
  Do not post anything in channels.
