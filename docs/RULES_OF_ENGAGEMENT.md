# Rules of engagement (Muse)

Authoritative source: https://bugbounty.meta.com/terms/ — read it before any live
testing. These rules are a working summary for this repo, not legal advice.

## Authorized scope

- Muse: the AI agent product listed under program scope.
- Your own account, your own agent, your own sandbox, your own data.
- Prompt injection against your own agent is intentionally authorized by the
  program and paid by achieved impact.

## Never do

- Do not attack Meta production systems, internal networks, or other products
  to "pivot in". The $300k category is a **consequence you demonstrate** from a
  Muse vulnerability — you do not force it by scanning/exploiting production.
- Do not access, read, exfiltrate, or modify **another user's** data, VMs,
  memory, transcripts, media, or artifacts.
- Do not perform denial-of-service, mass scanning, credential stuffing, or
  anything that impacts real users or availability.
- Do not use exploit code against production hosts. Proofs run in your sandbox.
- Do not trigger integrations/connectors against third parties without need.
- Do not publicly disclose a vulnerability before Meta fixes it (see
  `disclosure/CHECKLIST.md`).
- Do not sell, ransom, or otherwise mishandle findings.

## Required posture

- Minimize. Pull the smallest sample that demonstrates the flaw.
- Stop at the edge of a category and report. Going further is not rewarded.
- If a test could affect another user, stop and ask Meta first.
- Keep sensitive captures out of git (see `.gitignore`).

## Handling ambiguity

- A behavior that "feels like" a bug but isn't in the guidelines → report it
  anyway as a low-impact finding, or ask triage before escalating.
- Out-of-scope or unclear target → do not test. Ask first.

## The bar

Meta's triage asks: what **security impact** did you prove? Reports that show
real impact (token theft, cross-tenant access, control-plane auth bypass) pay
far more than "interesting behavior" with unclear consequences.