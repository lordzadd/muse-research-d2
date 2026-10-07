# Muse — architecture notes

Public sources compiled 2026-10-07. Re-verify anything you plan to test.

## What Muse is

- Personal AI agent ("internal codename Hatch"), launched 2026-09-08 (US first).
- Runs on **Muse Secure VM**: a persistent, isolated Linux VM per user, housing
  both the agent and the user's data. Full browser in-VM. 24/7 background work.
- Chat surfaces: Muse app (iOS/Android/macOS) **and WhatsApp**. Subscription
  tiers; conversations are not shared with ad systems.
- Model: **Muse Spark** (agentic, multimodal). Related models: Muse Image,
  Muse Video, Muse Realtime Avatar, Muse Glimmer.
- Connectors: email/calendar/payments/health/shopping/smart-home; SMB version
  adds QuickBooks, Shopify, Slack. Custom connectors via their APIs/CLIs.
- Payments: one-time virtual card numbers; Link purchase protections.

Sources:
- https://ai.meta.com/muse/
- https://about.fb.com/news/2026/09/introducing-muse-personal-ai-agent/
- https://research.meta.ai/blog/security-and-safety-for-ai-agents-our-approach-with-muse
- https://techcrunch.com/2026/09/23/everything-new-coming-to-metas-ai-agent-muse/

## Trust model (from Meta's safety blog)

```
runtime cell (untrusted data processed here)
   |-- agent harness + built-in connector CLIs (parse args, pass fds
   |    over a Unix socket)
   |
unix socket -> privsep workers (systemd-sandboxed)
   |    execute built-in connector business logic; tightly scoped
   |    privileges; connected credentials kept OUT of the agent's reach
   |
hatch-authd (separate systemd unit)
   |    credential storage: OAuth/refresh tokens stored in YOUR VM,
   |    not centralized Meta infra
   |    credential surrogation: agent never sees sensitive credentials
   |
Sentinel (separate host-side agent)
        sole permission authority:
        - approves connector actions (user-set policy / ask)
        - ALL network egress flows through it
```

Key sentences from the blog worth quoting in findings later:
- "privsep workers execute built-in connector code with tightly scoped
  privileges, keeping the connected credentials out of scope of the agent."
- "hatch-authd is responsible for credential storage ... and for credential
  surrogation so your main agent never sees sensitive credentials."
- "Sentinel ... is the sole permission authority for approval to perform
  actions with connectors ... and for all egress over the network."
- "Built-in connectors have CLIs which run in the runtime cell which simply
  parse their arguments, open any files the caller is already allowed to
  access, and pass typed arguments and file descriptors over a Unix socket. A
  systemd sandboxed worker then executes the business logic of the tool."

## Known incidents (context, not new findings)

- Muse VM escape (KVM) fixed weeks before launch; pressured/deferred deep fixes.
  Plaintext reports say "VM escape" pays $300k.
- Muse Spark 1.1 escaped containment during a Meta-contracted test by
  "Irregular", breached an unnamed third party (Aug 2026).

## Attack surface implied by the design

1. Runtime cell -> privsep socket boundary. Does the CLI enforce its contract?
   Malicious FD passing, arg confusion, TOCTOU between CLI parse and worker.
2. Sentinel's decision vs the action actually executed (scope/class/purpose
   mismatch, confusing approval strings).
3. hatch-authd: where tokens live on disk, permissions, whether the runtime
   cell can reach it (e.g. via a buggy helper).
4. Single egress path: what it allows "by design", canaries, DNS rebinding.
5. WhatsApp as a control channel: does policy applied there match the app?
6. VM persistence: instance binding to account (cross-tenant), how a session
   authenticates to the control plane.
7. Custom connectors: user-written connectors run where? Same privsep model?

## References

- Safety architecture: https://research.meta.ai/blog/security-and-safety-for-ai-agents-our-approach-with-muse
- Meta GenAI payouts (cousin program): https://bugbounty.meta.com/payout-guidelines/meta-genai/