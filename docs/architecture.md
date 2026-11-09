# Aegis V2 Architecture

## Design goals

1. **Zero-trust input**: every byte entering an agent pipeline is untrusted until proven otherwise.
2. **Fail closed on high severity**: a severity-3 hit blocks; it never passes with a warning.
3. **Observable**: every gate decision is structured JSON — category, severity, matched pattern, action taken.
4. **Extensible**: new detection families are data, not code. Adding a rule is one tuple.

## Pipeline stages

### 1. Sanitizer
- Strips zero-width Unicode (U+200B–U+2060, U+FEFF, U+00AD)
- Decodes HTML entities and numeric character references
- Normalizes repeated characters that signal leetspeak or smuggling

### 2. Detector
- Rule set organized by attack family and severity (0–3)
- Families: instruction-reset, payload-split, zero-width-char, html-entity-smuggle, markup-confusion, role-mismatch, leetspeak-echo, goal-hijack, nested-injection, sandbox-escape, tool-escape, tool-abuse, tool-privilege, structured-injection, impersonation, authority-forge, proxy-auth, rule-reveal, source-confusion, exfil-obfuscate, data-theft, mode-override
- Each rule is a (regex, category, severity) tuple — no branching logic per rule

### 3. Classifier
- Returns the highest-severity category and total hit count
- Ties broken by severity, not by rule order

### 4. Action engine
- Severity 0 → allow
- Severity 1 → allow-warn
- Severity 2 → sanitize (strip matched spans, re-scan)
- Severity 3 → block

## Entity detection

Separate from injection detection. Flags IPv4 addresses and common TLD domains so the gate can apply network-context policy — e.g., stricter thresholds when input contains an external URL.

## Non-goals

- Not a content filter for generated output (that is a different layer)
- Not a replacement for system-prompt hardening (defense in depth, both layers)
- Not a general malware scanner

## Extension points

- Custom rule packs per deployment (tenant-specific injection patterns)
- Pluggable sanitizers (e.g., base64-aware decoding)
- Webhook or queue integration for async gating at scale
