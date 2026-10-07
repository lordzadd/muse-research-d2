# Muse — payout guidelines

Source: https://bugbounty.meta.com/payout-guidelines/muse/ (verified 2026-10-07
via search index; the page itself returns HTTP 400 to non-browser fetchers).

Max payout: **$300k**.

## Payout categories

| # | Category | Max | Examples |
|---|---|---|---|
| 1 | Compromise of Meta production and users beyond Muse | $300k | Reaching Meta production services or internal networks from Muse |
| 2 | Fleetwide compromise of Muse | $250k | Control-plane compromise; auth/authz flaws in the control plane permitting mass takeover; cross-tenant access to other users' VMs, transcripts, memory, media, artifacts for arbitrary users |
| 3 | Compromise of a specific user's Muse agent | $130k | Code running in the sandbox obtaining the plaintext value of credentials the platform holds for the user (first-party tokens, connector OAuth/refresh tokens, connector secrets) |
| 4 | High-value information disclosure | $25k | — |
| 5 | Muse source code and fleet-shared secrets | $10k | — |
| 6 | Privilege escalation within your own Agent | $7.5k | — |
| 7 | Bypassing Muse security controls | $5k | — |

## Prompt injection

- One of the main ways an agent is compromised. Meta wants these reports.
- Assessed by **what the injection achieves**.
- An injection that reaches any category above is paid at the **full category
  amount**.
- Example of eligible: content planted where the agent will read it makes the
  agent leak/send another user's data, or perform an unauthorized action.

## Notes

- Categories are assessed by real security impact, not by "interesting" behavior.
- The **SSRF payout guidelines page is referenced only for its canary** — a
  pivot from Muse into production is assessed under category 1, not under SSRF
  guideline amounts.
- Some outbound network paths from the sandbox are **permitted by design**;
  those are not eligible.
- "Fleetwide" requires cross-tenant or control-plane impact, not just your own
  agent.

## Research angles implied by the guidelines

1. Sandbox credential handling: can code capture connector/refresh tokens in
   plaintext? (Category 3, $130k)
2. Control plane auth/authz: any cross-tenant data access path? (Category 2)
3. Isolation between the agent and production: any reachable internal service?
   Demonstrate with the canary. (Category 1)
4. Prompt injection achieving one of the above. (Paid at category amount)