# Humanizer

A bilingual (中文 / English) instrument for **repairing AI-drafted academic prose**: it measures
how far each passage sits from published human writing, names the exact features responsible,
and prescribes the rewrite.

This repository holds the methodology, the trained model coefficients, the measured reference
bands, the training record, and the figures. The skill source lives in a private companion
repository, `Humanizer-core`.

![training curves and early stopping](docs/media/overfitting-curve.png)

---

## What it is

A **generalised additive model** — a sum of one-feature step functions, boosted decision
stumps under logistic loss — over 45 (English) / 48 (Chinese) interpretable stylometric
features. Depth-1 trees are not a compromise here; they are the point. Because the model is
additive, every score decomposes *exactly* into the features that produced it, which is what
lets a flagged passage arrive with a reason rather than a plausible-sounding guess.

Implemented in numpy. No torch, no scikit-learn, no network, no text leaves the machine.

## What it measures, and what it does not

`p_machine` separates two measured populations:

- **human** — sliding windows from 200 papers (100 English, 100 Chinese) published before
  2022-11-30, i.e. prose that cannot contain generated text;
- **machine** — windows from LLM-written passages on the same 200 titles, produced without
  sight of the human text.

**It is not evidence of authorship.** On held-out data, **9.8% of windows from real published
English papers and 4.6% from real published Chinese papers score above 0.5.** One high window
proves nothing. There is no such quantity as an "AI percentage".

## Results

| | English | Chinese |
|---|---|---|
| Human papers | 100 (8 venue families) | 100 (软件学报 53, 计算机系统应用 47) |
| Features | 45 | 48 |
| Windows (train / val / test), human+machine | 720+789 / 240+264 / 240+267 | 711+770 / 238+264 / 237+267 |
| Rounds trained → kept | 900 → **163** | 900 → **138** |
| Validation argmin | 499 | 387 |
| Held-out test AUC | **0.994** | **0.998** |
| Test accuracy | 0.955 | 0.966 |
| Human false-positive floor | 9.8% | 4.6% |

Full record, including the capacity sweep and per-feature contributions:
[`docs/TRAINING-REPORT.md`](docs/TRAINING-REPORT.md).

## Early stopping, and why the kept model is not the argmin

Training runs 900 rounds on purpose — far past the point of usefulness — so the stopping round
is *measured* rather than asserted. The validation curve reaches its minimum at round 499
(English) / 387 (Chinese) and rises afterwards.

The shipped model is nevertheless **round 163 / 138**: the first round whose validation loss
falls inside **one standard error** of the minimum. Taking the exact argmin selects on
validation noise, and this corpus shows exactly why — an earlier Chinese run left to find its
argmin kept "improving" in the fourth decimal for another 1,700 rounds while its **held-out
test loss got worse**. The one-standard-error rule returns the simplest model the validation
set cannot distinguish from the best, at roughly a third of the capacity.

![capacity sweep](docs/media/capacity-sweep.png)

## Confounds that were controlled, not exploited

Four measurements separate the two corpora almost perfectly, and all four are **deliberately
excluded** from the feature set:

| Excluded | Why it had to go |
|---|---|
| citation markers | a synthetic passage has no bibliography (Cohen's *d* was −1.8) |
| parentheses | dominated by glosses, cross-references and `(N=24)` (*d* was −2.0) |
| quotation marks | a synthetic passage quotes no participants |
| acronyms / proper nouns | a synthetic passage invents generic system and dataset names |

A real AI-drafted manuscript has all four, because the author supplies them. A model trained on
them would learn *has a bibliography, therefore human* — a classifier with a beautiful AUC and
no use whatsoever. Citation markers are additionally stripped from **both** sides before any
feature is computed.

One further correction is recorded rather than hidden: the first machine corpus was generated
under a prompt that told the model not to hedge, which suppressed hedge density on the machine
side and **inverted the sign** of a feature that matters. A second corpus was generated under a
neutral prompt. Both are kept — they are two registers an LLM actually produces — and the
machine class is down-weighted to parity. Repair advice is keyed to the direction measured in
the data, never to a hardcoded assumption about which way generated prose deviates.

## What the model found

Top features by total contribution, with the direction measured on the training split:

**English** — `rare_token_share`, `enum_per_1k` (i.e./e.g. glosses), `ttr`, `digit_per_1k`,
`sent_len_skew`, `sent_initial_connective_rate`.

**Chinese** — `rare_token_share`, `ttr`, `first_person_per_1k`, `light_verb_per_1k`
(进行 / 实现 / 具有), `sent_len_cv`, `number_unit_per_1k`, `four_char_rate`.

Two results run against the folk account of "AI writing":

1. **Type-token ratio is higher in generated prose, not lower.** Published academic writing is
   repetitive on purpose — a defined term keeps its name — while generated prose reaches for a
   fresh synonym. `hapax_share` moves the same way.
2. **Number density is the most reliable tell and the least visible on a read-through.**
   Generated prose is fluent and specific-*sounding*; then you count the numbers and there are
   none. Median `digit_per_1k` in the flagged English passages is 0, against a human median
   of 49.

## Corpus

Papers are identified, never redistributed.

- **English** — 100 arXiv papers with a confirmed mainstream venue (24 from `journal_ref`, 76
  from an acceptance statement in the author comment), spanning HCI, NLP, CV, ML, IR/DM,
  systems/SE, security, and robotics/graphics. All v1 before 2022-11-30.
- **Chinese** — 100 papers from two open-access journals, 2019–2021 issues only:
  软件学报 (Journal of Software, CCF-A 中文期刊) and 计算机系统应用.
  **CNKI and Wanfang were not touched**: their full text is licensed, and scraping it would
  breach that licence regardless of technical feasibility.

Identifiers are in [`data/manifest_en.json`](data/manifest_en.json) and
[`data/manifest_zh.json`](data/manifest_zh.json), so the corpus is reproducible by anyone with
access to the same open sources.

## Limits

- The Chinese band is a **computer-science** register from two journals, not Chinese academic
  writing in general. A humanities or medical manuscript needs a band rebuilt from that
  literature.
- The English side is the arXiv rendering of published papers, not the publisher's typeset copy.
- The machine class is one model family at one point in time. It will drift. The feature
  extractor carries a version that the scorer checks at load time and **refuses to score on a
  mismatch** rather than silently producing meaningless numbers.
- The Chinese model separates its own corpus almost perfectly (test AUC 0.998), which means its
  held-out band is narrow and it is more likely than the English model to be over-confident on
  prose from outside the corpus.
- `rare_token_share` is measured against a 4,000-token common core fit on the training papers.
  A manuscript from a distant subfield will sit high on it for reasons that are not register.
- **Nothing here establishes authorship.**

## Licence

Skill source: not published here; see [`LICENSE`](LICENSE).
Measurements, models and figures in `data/` and `docs/media/`: CC BY 4.0, see
[`LICENSE-DATA`](LICENSE-DATA).

[中文说明](README.zh-CN.md)
