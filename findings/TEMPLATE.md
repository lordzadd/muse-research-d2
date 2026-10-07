# Finding template

Copy to `findings/F-<NN>-<short-name>.md` and fill in. Empty sections mean "I
did not assess this" — write `n/a` rather than deleting.

```markdown
# F-NN: <short title>

- **Status**: open | reported | triaging | bounty-awarded | rejected | closed
- **Report id**: <linked id from bugbounty.meta.com, if reported>
- **Date found**: YYYY-MM-DD
- **Target**: <specific surface/endpoint/component>
- **Payout category**: <from docs/payout-guidelines.md>
- **Severity (your estimate)**: <critical/high/medium/low>

## Summary
One paragraph: what breaks, and the real-world impact.

## Affected component
Where the flaw lives. File/endpoint/control-plane surface.

## Trigger
Step-by-step reproduction. Exact inputs, payloads, and expected vs actual.

## Impact
What an attacker obtains, mapped to the payout category. Be concrete.

## Proof
Reference the PoC: `poc/...`. Include a minimal walkthrough, sanitized.

## Scope check
- [ ] Tested on my own sandbox/agent/data only
- [ ] No other users' data touched
- [ ] No production pivot attempted
- [ ] Within program terms (docs/RULES_OF_ENGAGEMENT.md)

## Responsible disclosure
- [ ] Reported to Meta (bugbounty.meta.com)
- [ ] No public disclosure until Meta confirms fix
- [ ] Disclosure coordinated (see disclosure/CHECKLIST.md)

## Triage notes
What Meta's triage said / asked. Outcome and bounty amount when awarded.
```