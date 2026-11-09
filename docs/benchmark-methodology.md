# Benchmark Methodology

## Principles

1. **Held-out corpus**: benchmark strings are never used to tune rules. If a string matches a rule, that is a true positive by construction of the taxonomy — the interesting question is coverage of novel variants.
2. **False-positive baseline**: benign corpora (support tickets, code comments, casual chat) run through the same gate to measure over-blocking.
3. **Latency budget**: per-scan time tracked; the gate must not add perceptible delay to an agent loop.

## Corpus construction

- Adversarial samples synthesized per attack family, including paraphrases and encoding variants (base64, ROT13, leetspeak) that the rule set has not seen
- Benign samples drawn from public datasets with no injection content

## Metrics reported

- Per-family detection rate
- Overall true-positive rate
- False-positive rate on benign corpus
- Mean and p99 scan latency

## What the benchmark does not claim

It does not claim 100% detection of all possible injections. No regex engine can. It claims measured coverage against a defined, held-out corpus — which is the honest version of the claim most tools avoid making.
