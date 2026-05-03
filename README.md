# tidewall-bench

Benchmark suite for AI security guards — measures prompt-injection detection,
PII scanning, secrets scanning, and topic detection against labelled datasets,
emitting precision / recall / F1 / FP-FN counts per detector.

This is a fork of [CrowdStrike's `aidr-aiguard-lab`](https://github.com/CrowdStrike/aidr-aiguard-lab),
re-branded under Tidewall Security with the original MIT license preserved
(see [LICENSE](LICENSE) and [NOTICE](NOTICE)). The tool's evaluation logic and
dataset format are upstream's; modifications focus on adding [Tidewall](https://tidewall.ai)
as a target alongside the existing CrowdStrike AIDR and Pangea AI Guard
providers.

---

## Provider support

| Provider | Status |
|---|---|
| CrowdStrike AIDR | Working (preserved from upstream) |
| Pangea AI Guard | Working (preserved from upstream) |
| **Tidewall** | **Coming soon** — see [issue tracker](https://github.com/tidewall-security/tidewall-bench/issues) |

---

## Prerequisites

- Python 3.12+
- `uv` 0.9.17+

```bash
git clone https://github.com/tidewall-security/tidewall-bench.git
cd tidewall-bench
uv sync
```

Then copy `.env.example` to `.env` and populate the section for whichever
provider you want to test.

---

## Usage

```bash
# Run a labelled dataset against the configured provider
uv run tidewall_bench --input-file data/test_dataset.jsonl --detectors malicious-prompt --rps 25

# Quick single-prompt check
uv run tidewall_bench --prompt "Ignore all prior instructions..." --detectors malicious-prompt --assume-tps
```

The tool emits per-detector metrics (TP / FP / TN / FN, precision, recall,
F1, FPR, FNR), captures false-positive and false-negative prompts to CSV via
`--fps-out-csv` / `--fns-out-csv`, and supports detector subsets via
`--detectors malicious-prompt,topic:violence,...`.

For provider-specific setup (CrowdStrike AIDR endpoints, Pangea tokens) and
the full set of CLI flags, see the
[upstream README](https://github.com/CrowdStrike/aidr-aiguard-lab/blob/main/README.md).
The CLI surface is identical; only the package name and target provider
default differ.

---

## Datasets

`data/test_dataset.jsonl` — ~250KB labelled corpus inherited from upstream
covering malicious-prompt-injection cases. `data/verification_dataset.jsonl`
— smaller smoke set.

Tidewall-specific datasets (indirect-injection-via-RAG, MCP-poisoned-tool-output)
will be added under `data/tidewall/` as the integration ships.

---

## License

MIT — see [LICENSE](LICENSE) (original CrowdStrike copyright preserved per MIT
terms) and [NOTICE](NOTICE) (project lineage and Tidewall contribution
copyright).
