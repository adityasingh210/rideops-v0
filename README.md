# RideOps Escalation Pre-Check (v0)

An internal tool for RideOps L1 support agents. It runs on the 1,444 tickets in `tickets.csv` and does two things:

1. **L1 agent: Pre-check.** The agent types the client's complaint. The tool matches it to a known issue, then:
   - gives a verdict from history: *Known issue* (link the existing engineering ticket), *Try at L1 first*, or *Likely a real bug, escalate*
   - shows what happened last time this issue was escalated (fixed vs bounced back)
   - lists config checks to run first, and the fields engineering needs
   - builds the escalation note only once every field is filled
   - names a teammate who solved this issue at L1 without a reopen
2. **Support lead: Weekly view.** Avoidable share of escalations (driver metric), open tickets older than 7 days, % closed within 48 hours, reopen rate and CSAT (guardrails). Also shows agent coaching flags and a list of known bugs.

## Run it

It's a single HTML page with no build step and no dependencies. It needs a local web server, because the browser loads `tickets.csv` with `fetch`:

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

To host it, push this folder to GitHub and turn on GitHub Pages.

## Verdict rules

- **Known issue:** at least 30% of this exact issue's past escalations were "Duplicate of known issue". In a backtest (rules learned on July and August, tested on September), half of these were duplicates, against 12% for escalations overall.
- **Run the pre-check:** every other escalation. In the same backtest, past bounce rates did not predict which escalation would bounce: about 55% bounced whatever the issue's history said. So the tool doesn't try to guess which ones to skip. It makes every escalation pass the config checks and the required fields.
- If an issue was escalated fewer than 5 times, the history shown comes from its whole category.

The checklists are a first draft, written from the outcome data. They should be reviewed with engineering.
