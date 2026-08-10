# Humanizer — holdout-machine-en.md

**REWORK** — 57% of windows score above 0.5, against 10% for held-out human prose.

| | this draft | held-out human prose |
|---|---|---|
| median window p(machine) | 0.556 | 0.019 (p90 0.467) |
| windows above 0.5 | 57.1% | 9.8% |
| windows scored | 7 | 480 |

Model: en, 163 boosting rounds kept of 900 trained, held-out test AUC 0.994.

A single high window proves nothing — see the false-positive rate above. Read the passage.

## Passages to rework

### 1. p(machine) = 0.822

> Modern server software is configurable to a degree that few of its operators fully appreciate. A recent release of MySQL exposes over six hundred system variables, Apache httpd offers several hundred directives across its core and standard modules, and a Kubernetes cluster presents thousands of settable fields once custom resources are counted. Only a small …

- **rare_token_share** = 0.195 (human p10–p90: 0.04–0.126, median 0.079). Heavy rare-term load. Check the terms are defined on first use.
- **ttr** = 0.703 (human p10–p90: 0.486–0.629, median 0.565). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **digit_per_1k** = 0.0 (human p10–p90: 4.523–174.318, median 49.466). Very few numbers. This is the most reliable tell in the whole set and the least visible on a read-through: generated prose is fluent and specific-sounding, then you count the numbers and there are none. Go paragraph by paragraph and ask what quantity belongs in it.
- **hapax_share** = 0.837 (human p10–p90: 0.617–0.766, median 0.701). Very high share of once-only words — synonym churn. Academic prose is repetitive on purpose: a defined term should keep its name.

### 2. p(machine) = 0.746

> What makes these failures durable is that nothing in the software distinguishes them from the hundreds of benign performance and behavior knobs sitting beside them in the same configuration file. The current answer to this problem is manual curation. Organizations such as the Center for Internet Security publish hardening benchmarks that enumerate security-r…

- **rare_token_share** = 0.167 (human p10–p90: 0.04–0.126, median 0.079). Heavy rare-term load. Check the terms are defined on first use.
- **ttr** = 0.687 (human p10–p90: 0.486–0.629, median 0.565). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **digit_per_1k** = 0.0 (human p10–p90: 4.523–174.318, median 49.466). Very few numbers. This is the most reliable tell in the whole set and the least visible on a read-through: generated prose is fluent and specific-sounding, then you count the numbers and there are none. Go paragraph by paragraph and ask what quantity belongs in it.
- **sent_len_skew** = -0.347 (human p10–p90: -0.2–1.611, median 0.63). The sentence-length distribution is symmetric. Authored prose is right-skewed: mostly medium, a few long. Add one genuinely long, qualified sentence.

### 3. p(machine) = 0.571

> Automated approaches have taken two forms. Static analysis traces configuration variables to the code paths they influence and flags those reaching security-sensitive APIs, which is precise where it applies but requires source access, per-language tooling, and substantial engineering per project. Keyword matching over documentation is trivially portable but …

- **rare_token_share** = 0.154 (human p10–p90: 0.04–0.126, median 0.079). Heavy rare-term load. Check the terms are defined on first use.
- **ttr** = 0.718 (human p10–p90: 0.486–0.629, median 0.565). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **digit_per_1k** = 0.0 (human p10–p90: 4.523–174.318, median 49.466). Very few numbers. This is the most reliable tell in the whole set and the least visible on a read-through: generated prose is fluent and specific-sounding, then you count the numbers and there are none. Go paragraph by paragraph and ask what quantity belongs in it.
- **sent_len_skew** = -0.478 (human p10–p90: -0.2–1.611, median 0.63). The sentence-length distribution is symmetric. Authored prose is right-skewed: mostly medium, a few long. Add one genuinely long, qualified sentence.
