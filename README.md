# portfolio-audit-log

A monthly fingerprint log for published investment reports — one SHA-256 hash per report, appended in git history so a published number cannot be quietly revised.

**Brand of the underlying report:** Chris的AI投研手记 · 投资证据链

## What this repo is

Every month a report is generated, the file is hashed and the digest is appended to `audit_log.json`. Because this file lives in git, the append-only property is enforced by the history rather than by a promise: revising an entry rewrites a commit, and the previous state stays visible in the log.

```
entry = { date, report, sha256, note }
```

## What it proves — and what it does not

| Proves | Does not prove |
|---|---|
| The report file at `date` had exactly this digest | That the report's *method* is correct |
| No entry was silently edited after publication | That the numbers inside are free of bugs |
| A reader can recompute the digest and compare | Anything about a third party's data |

A hash is a statement about **identity**, not about **quality**. It answers "is this the same file that was published in August?" and nothing more. The correctness of the analysis is a separate question, handled by the audit practice in [`backtest-lint`](https://github.com/chris-dad/backtest-lint) and documented in its [case studies](https://github.com/chris-dad/backtest-lint/blob/main/CASE-STUDIES.md).

## How to verify an entry

```bash
# 1. take any entry from audit_log.json
grep -A2 '"date": "2026-08-18"' audit_log.json

# 2. recompute the digest of the report file you were given
shasum -a 256 portfolio_report.html

# 3. the two digests must match character for character
```

If the file you hold produces a different digest, you are not holding the file that was fingerprinted — regardless of what its filename says.

## Current state

| Field | Value |
|---|---|
| Entries | 1 (as of 2026-11-22) |
| First entry | 2026-08-18 — `portfolio_report.html`, initial entry after the investment-scope correction |
| Cadence | Monthly, appended after the five publication gates pass |

One entry is a starting point, not a track record. The reason this repository exists in public while nearly empty is deliberate: it is cheaper to publish the mechanism before there is anything impressive in it, and a hash chain that starts small is still a hash chain.

## License

MIT
