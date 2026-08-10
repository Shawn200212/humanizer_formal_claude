# Humanizer — holdout-human-en.md

**PASS** — median window and above-0.5 share both sit inside the human held-out band.

| | this draft | held-out human prose |
|---|---|---|
| median window p(machine) | 0.000 | 0.019 (p90 0.467) |
| windows above 0.5 | 0.0% | 9.8% |
| windows scored | 17 | 480 |

Model: en, 163 boosting rounds kept of 900 trained, held-out test AUC 0.994.

A single high window proves nothing — see the false-positive rate above. Read the passage.

## Passages to rework

### 1. p(machine) = 0.003

> A critical part of the IT security in an organization such as Siemens is the secure configuration of all used software . Here, we need to know which configuration settings (from here on settings) of a software are security-relevant (sr) or not security-relevant (nsr) Permission to make digital or hard copies of all or part of this work for personal or classr…

- **rare_token_share** = 0.152 (human p10–p90: 0.04–0.126, median 0.079). Heavy rare-term load. Check the terms are defined on first use.
- **ttr** = 0.619 (human p10–p90: 0.486–0.629, median 0.565). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **sent_len_cv** = 0.917 (human p10–p90: 0.306–0.714, median 0.463). Sentence lengths swing more than published prose. Usually a run-on next to a fragment; even out the extremes.

### 2. p(machine) = 0.003

> One might now ask why we still missed 23 sr settings and could not meet our goal of ≈ 100% recall. When we investigated the 23 false negatives, we could see that the LDA-based model could not take the context of a word into account and lacked the semantical understanding 4Code: kaggle/tumin4/transformer-based-machine-learning necessary to classify the settin…

- **rare_token_share** = 0.161 (human p10–p90: 0.04–0.126, median 0.079). Heavy rare-term load. Check the terms are defined on first use.
- **hapax_share** = 0.779 (human p10–p90: 0.617–0.766, median 0.701). Very high share of once-only words — synonym churn. Academic prose is repetitive on purpose: a defined term should keep its name.
- **ttr** = 0.646 (human p10–p90: 0.486–0.629, median 0.565). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **parallel_start_rate** = 0.286 (human p10–p90: 0.0–0.167, median 0.0). Consecutive sentences opening identically. Vary what occupies the subject position.

### 3. p(machine) = 0.002

> Nevertheless, these words also occur frequently in the nsr descriptions, and we constructed based on tf-idf a counterpart set of words that mark nsr settings, e.g., “color", but not enough to prevent a high number of false positives. The same problem occurred when we used n-grams or named entity recognition: The entity represents a particular case referring …

- **rare_token_share** = 0.12 (human p10–p90: 0.04–0.126, median 0.079). Heavy rare-term load. Check the terms are defined on first use.
- **clause_per_sent** = 1.857 (human p10–p90: 0.733–2.5, median 1.333). Long comma-chained sentences. Split at the clause boundary.
- **clause_cv** = 0.73 (human p10–p90: 0.622–1.406, median 0.958). Every sentence has the same number of clauses. Vary the syntactic shape, not just the length.
- **connective_per_1k** = 4.505 (human p10–p90: 0.0–13.575, median 4.348). Discourse markers on nearly every sentence.
