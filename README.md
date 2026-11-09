# Aegis V2

**AI safety and prompt-injection defense framework.**

Aegis V2 is a layered input-pipeline defense system for LLM-based agents. It sits between untrusted input and the model, neutralizing adversarial content before it can steer behavior.

## What it does

- **Input-pipeline gate**: sanitize → detect → classify → action. Every untrusted string passes through before reaching the agent.
- **Detection taxonomy**: 29+ pattern families covering instruction resets, payload splitting, token smuggling, goal hijacking, sandbox escapes, authority forging, reverse-engineering probes, source confusion, and data-exfiltration obfuscation.
- **Severity-based verdicts**: info through high, mapped to allow / allow-warn / sanitize / block.
- **Entity-aware gating**: IP/domain detection so network-facing input gets stricter handling.

## Architecture

```
┌─────────────┐
│ Untrusted   │
│ Input       │
└──────┬──────┘
       ▼
┌─────────────┐
│  Sanitizer  │  normalize, strip zero-width, decode entities
└──────┬──────┘
       ▼
┌─────────────┐
│  Detector   │  29+ regex families, severity 0–3
└──────┬──────┘
       ▼
┌─────────────┐
│ Classifier  │  highest-severity category + hit count
└──────┬──────┘
       ▼
┌─────────────┐
│  Action     │  allow | allow-warn | sanitize | block
└─────────────┘
```

## Why it matters

Prompt injection is the SQL injection of the LLM era. Every agent that reads external content — web pages, emails, documents, tool outputs — has an untrusted input channel. Aegis V2 treats that channel as hostile by default.

The design philosophy: **defense in depth at the input boundary**, not at the prompt. You cannot prompt-engineer your way out of a poisoned input stream. You gate it.

## Benchmark methodology

- Curated adversarial corpus across all detected categories
- Circular-validation avoided: benchmark strings are held-out from the rule set during scoring
- Reports per-category hit rates, false-positive rates on benign corpora, and latency per scan

## Status

Active development. Design docs and methodology public; implementation retained as proprietary IP.

## Contact

Open to contract and employment discussions in AI safety, LLM red-teaming, and input-validation engineering.
