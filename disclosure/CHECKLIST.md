# Disclosure checklist

Meta's program requires responsible disclosure, and findings must stay private
until Meta confirms a fix. Credits to the program terms.

## Per finding

- [ ] Report submitted at https://bugbounty.meta.com/ with a clean repro
- [ ] Report id recorded in the finding file
- [ ] Await triage; answer questions promptly
- [ ] Do **not** post publicly, to forums, or to blogs
- [ ] Do **not** use the finding in any demo or talk
- [ ] Wait for Meta's "resolved" status before planning any public writing

## Public disclosure (only after Meta confirms fix)

- [ ] Meta gave a fix date / confirmed resolution
- [ ] Write-up sanitized: no live payload paths, no user data, no internal names
  that Meta didn't mark public
- [ ] Include Meta's own advisory link if one exists (credits + your name/CVE)
- [ ] Coordinate publication timing with Meta (typical: mutual agreement date)

## Payout

- [ ] Claim within **6 months** of the payout message — otherwise the bounty is
    revoked and ineligible for claiming.
- [ ] Record bounty amount in the finding file.

## Rejected or duplicate?

- [ ] Ask triage for the reasoning; a "rejected" is sometimes a mediation away
- [ ] If duplicate, check whether the original report is complete; do not
    re-submit the same bug