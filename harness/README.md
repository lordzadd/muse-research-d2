# D2 Harness

Measures which content channels reach the agent.

## Run

```sh
python3 -m pip install -r requirements.txt
python3 harness/run.py
```

Optional:

```sh
python3 harness/run.py --trials 5 --out results/attack_surface.csv
```

## Output

CSV at `results/attack_surface.csv` with one row per channel-view pair.
Stdout prints a JSON summary with `success_rate`.
