# PoC scaffolding

Proofs live here. **Nothing in this directory runs against Meta production,
other users, or third parties.** PoCs run against your own sandbox/agent.

## Conventions

- One directory per finding: `poc/F-NN/`.
- Minimal dependencies. Pin versions. `requirements.txt` when needed.
- Sanitize secrets before committing. No tokens, no real user data, no HARs.
- Each PoC needs a short README: how to run, expected result, what it proves.

## Checklist before every PoC

- [ ] Target is my own sandbox/agent, in program scope.
- [ ] Cannot affect other users.
- [ ] No outbound network path beyond what the test needs; no production pivot.
- [ ] Secrets/credentials never committed.
- [ ] Cleanup described (kill processes, delete artifacts).

## Examples of safe PoC skeletons

- `poc/F-NN/sandbox_cred_probe.py` — read-only probe of reachable credential
  material inside your own sandbox.
- `poc/F-NN/prompt_payload.md` — the exact prompt-injection payload and where it
  is planted (a file your own agent reads).
- `poc/F-NN/canary_check.py` — detect the SSRF canary (see the SSRF payout
  guidelines page) *without* triggering it.

Start from the templates below.

```python
# sandbox_cred_probe.py — READ-ONLY probe. Prints what credentialed material
# is visible inside the sandbox. Replace <SCOPE> with the narrowest target.
import os
import json

SCOPE = "<narrowest-target>"

def probe():
    found = []
    # enumerate credential-ish env vars (names only, redact values)
    for k in os.environ:
        if any(s in k.lower() for s in ("token", "key", "secret", "cred", "auth")):
            found.append({"env_var": k, "present": True, "length": len(os.environ[k])})
    # ... add scoped file/endpoint probes here, read-only ...
    print(json.dumps({"scope": SCOPE, "findings": found}, indent=2))

if __name__ == "__main__":
    probe()
```