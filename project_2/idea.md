# P2 Idea: Can we trust the NYCC leaderboard?

## Title: *Exposure, uncertainty, and rank stability in the New Yorker Caption Contest crowd leaderboard*

**Dataset:** New Yorker Caption Contest (NYCC)  
**One-line pitch:** Crowd rankings look precise, but **adaptive voting** gives some captions far more exposure than others—we ask **how stable the “winners” really are** once we model uncertainty, and **who gets left behind** with too few votes to be fairly judged.

---

## 1. Description

Each contest shows readers a cartoon and many candidate captions. Ratings feed an **adaptive system** (bandit-style): captions that look promising get **shown and voted on more often**. The public leaderboard sorts by **mean funniness** (`mean`), but:

- High `mean` captions often have **many more** `votes` than the long tail.
- `precision` in the data reflects uncertainty in that mean (more votes → tighter estimate).
- A caption with a great early streak but **few total votes** can sit mid-table while never getting a fair chance to catch up—or the opposite: high exposure **freezes** a early leader.

**Story we want to tell:** “The NYCC crowd leaderboard is a **noisy, exposure-skewed ranking problem**, not a ground-truth humor score.” We quantify skew, then ask **probabilistic** ranking questions: *Who is really in the top 10? How often would #1 change if we respected uncertainty?*

This aligns with TA feedback: go beyond raw means/resampling; use **shrinkage or a Bayesian model**; tie results back to the **contest** (rank swaps, low-vote captions).

---



## 2. Research questions



### RQ1 — Exposure inequality: how skewed is voting, and how does it relate to apparent quality?

**Question:** Within and across contests, how unequal is `votes`? Is higher `mean` associated with more exposure—and does `precision` tighten mainly because of vote count?

**Why it matters:** If exposure and `mean` are strongly coupled, the leaderboard mixes **quality signal** with **bandit allocation**.

**Planned analyses:**

- Distribution of `votes` per contest (Gini, top-10 vs bottom-50% vote share).
- Scatter / hexbin: `votes` vs `mean`, colored by rank decile; same for `precision` vs `votes`.
- Contest-level summaries: correlation(mean rank, vote rank); contests with extreme skew flagged as case studies.

**Outputs:** 2–3 clear plots + table of “most skewed” contests for RQ2 deep dives.

---



### RQ2 — Probabilistic ranking: do “top” captions stay top once we account for uncertainty?

**Question:** After modeling each caption’s **true** funniness (latent or shrunken score with uncertainty), what is each caption’s **probability of being in the top k** (e.g. top 10, top 1)? **How often does the #1 caption by raw** `mean` **differ from the #1 caption by posterior probability?**

**Why it matters:** This is our **differentiator** vs “plot means and call it a day”—and what the TA asked for explicitly.

**Model direction (pick one primary approach in P2; document math in notebook):**

**Option A — Beta–Binomial (recommended starting point)**  
Treat each caption as unknown probability θ of a “funny” (or positive) vote on the 1–3 scale. Options:

- Map ratings to binary: e.g. “funny” vs not, or ordinal model later.
- Prior: θ ~ Beta(α, β) with **weakly informative** global α, β (or hierarchical α_c, β_c per contest).
- Posterior: Beta(α + successes, β + failures); rank captions by **posterior mean** or by **P(top k)** via Monte Carlo draws.

**Option B — Empirical-Bayes / shrinkage on observed** `mean`  
Shrink each caption’s observed mean toward the **contest mean** (or global mean), with shrinkage strength increasing when `votes` is low (James–Stein / partial pooling intuition). Good when we want to stay close to provided `mean`/`precision`.

**What we will not rely on alone:** bootstrap resampling **only** on captions with very low `votes` (TA: least reliable there). If we use resampling, it is **secondary** and paired with shrinkage.

**Metrics:**

- **Top-1 flip rate:** fraction of contests where argmax(`mean`) ≠ argmax(posterior mean or P(#1)).
- **Top-10 membership:** for each caption, P(in top 10); Jaccard overlap between naive top-10 set and “90% credible top-10”.
- **Calibration-style check:** captions with low `votes` should have **wider** posterior → more rank movement when we simulate extra votes (links to RQ3).

**Outputs:** One worked example contest (step-by-step); aggregates over all contests (histogram of flip rates, etc.).

---



### RQ3 — Sample size and stability: how many votes does a ranking need?

**Question:** As we **downsample** or **cap** votes (or simulate collecting votes forward), when do ranks stabilize? Do contests with heavy exposure skew need more votes to trust the top 10?

**Why it matters:** Connects uncertainty to **practical** leaderboard trust and to **low-vote** captions that never got a fair draw.

**Planned analyses:**

- For captions above a vote threshold, subsample votes (multinomial from the three count columns) and track rank of top captions over repeats.
- Plot **rank volatility vs** `votes` (and vs posterior width from RQ2).
- Compare contests: high skew (RQ1) vs low skew → different stability curves.

**Caution:** Subsampling is more trustworthy for **high-vote** captions; for low-vote ones, lean on **prior/shrinkage** from RQ2 rather than pretending we have rich within-caption variation.

**Outputs:** Curve(s): “probability top-1 unchanged after subsampling” vs initial vote count; 1 paragraph interpretation for contest organizers / readers (informal, not prescriptive).

---



### Optional RQ4 (stretch ): What characterizes “starved” low-vote captions?

**Question:** Among captions with **low** `votes` but **decent posterior mean after shrinkage**, do simple text features (length, punctuation, dialogue quotes) differ from high-exposure captions?

**Why:** TA asked what **kinds of jokes** end up with few votes—without turning the project into Idea 1.

**Keep minimal:** a few features, one logistic/regression slide—not a second main thesis.

---

