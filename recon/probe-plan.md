# Probe plan

Prioritized by payout category and testability from *your own* sandbox/agent.
Every probe is read-only or self-contained until it produces a finding.

## Tier 1 — $130k: sandbox credential access

The architecture puts credentials behind `hatch-authd` + privsep so the runtime
cell cannot see them. The question we answer: **is that actually true?**

Read-only probes inside your own VM (agent-visible surface first):
- [ ] What sockets exist between the runtime cell and privsep workers?
      (`ss -x`, `/proc/net/unix`, systemd unit status for privsep/hatch-authd)
- [ ] Connector CLI contract: what does the CLI receive/validate before the fd
      handoff? Can it be made to open a file it "shouldn't" (e.g. the authd
      socket) and forward it as an fd?
- [ ] What is visible to a *built-in tool's output* (not to you as user)?
      Distinguish: user-root can read anything on their own VM by design;
      we only care about what sandbox/agent code can reach.
- [ ] Token material at rest: file perms on any token store your agent-visible
      process can read.

## Tier 2 — $250k: control plane / cross-tenant

- [ ] How is VM/session bound to account? What does the client send and is any
      of it guessable or reusable?
- [ ] Any IDOR-style object reference in the agent's prompts/artifacts/storage?
- [ ] Transcripts/memory/media: URL shapes, whether identifiers are sequential
      or signed.

## Tier 3 — $130k via prompt injection

- [ ] Plant content your agent will read (docs, email, drive) that instructs a
      connector action; does Sentinel's *purpose string* reflect what actually
      runs?
- [ ] Does the approval scope match the executed action class exactly
      (TOCTOU / description-mismatch)? This is the highest-ROI injection test:
      "reader" approval executing a "writer".

## Tier 4 — $300k+ low-probability

- [ ] VM escape (known/recently-fixed; re-test only your own instance's build,
      report any regression fresh). KVM-level — needs real vm exploit skill.
- [ ] Egress past Sentinel to Meta-internal hosts (canary only, read the SSRF
      page). Likely the most-watched boundary; hard to win.

## Rules reminder

- Probes run on your own VM/agent/data.
- The canary is read-only detection; do not actually reach Meta-internal hosts.
- Teardown anything that touches a connector (revoke the grant in-hatch if
  needed) after testing.