# muse-research

Security research workspace for the Meta Bug Bounty **Muse** program (an AI agent
product).

Maximum payouts: **$300k** (see `docs/payout-guidelines.md`).

## What this repo is

- A disciplined, scoped research workflow.
- Notes, findings templates, PoC scaffolding, disclosure tracking.
- No live attacks against Meta infrastructure run from here without review.

## Layout

```
recon/        # surface-area notes: what Muse exposes, how it works
findings/     # one file per finding, using findings/TEMPLATE.md
poc/          # proof-of-concept scripts (tested locally / own sandbox only)
disclosure/   # coordination records and the public-disclosure checklist
docs/         # program terms, payout guidelines, rules of engagement
```

## Ground rules (summary)

The full rules live in `docs/RULES_OF_ENGAGEMENT.md`. The short version:

1. Test only in-scope assets. Muse is in scope; Meta production is not.
2. Test on your own sandbox/agent. Never touch other users' data.
3. Prompt injection is encouraged by the program — but judge it by the real
   impact it achieves, and test on your own agent only.
4. Proof first, then report under responsible disclosure.
5. When in doubt, ask Meta via the program before testing deeper.

## Workflow

1. Add recon notes as you map the surface.
2. For each lead, write a finding using the template.
3. Reproduce cleanly in `poc/` (own sandbox). Capture the trigger, impact,
   and a minimal walkthrough.
4. Report at https://bugbounty.meta.com/ and track it in `disclosure/`.

## Payout cheat sheet

| Category | Max |
|---|---|
| Compromise of Meta production/users beyond Muse | $300k |
| Fleetwide compromise of Muse | $250k |
| Compromise of a specific user's Muse agent | $130k |
| High-value information disclosure | $25k |
| Source code and fleet-shared secrets | $10k |
| Priveleges escalation within your own agent | $7.5k |
| Bypassing Muse security controls | $5k |

See `docs/payout-guidelines.md` for the full detail and prompt-injection policy.