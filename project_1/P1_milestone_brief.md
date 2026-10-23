# ADA CS-401 Fall 2026 — Milestone P1 Brief

**Team dataset:** New Yorker Caption Contest (NYCC)  
**P1 due:** Friday 2 October 2026, 23:59 CET  
**Weight:** 10% of the project (project is 30% of the course)  
**Submit:** individually, Google Form, **≤ 500 words**  
**Official page:** [epfl-ada.github.io — Projects](https://epfl-ada.github.io/teaching/fall2026/cs401/projects/)

This note is a working document (longer than 500 words). The Form submission must be a **compressed outline** of three ideas, not this whole file.

---

## 1. Goal of P1

P1 is **not** the final project and **does not grade code**. The staff want you to:

1. Agree as a **team** on one official dataset (done: NYCC).
2. Read the paper(s) in the dataset sheet **Column G** (at least enough to know what the data is and how it was collected).
3. **Play with a sample** of the data (summary stats, a few contests). No full pipeline required.
4. Propose **exactly three** bold, creative project ideas that *could* be done with this dataset (plus extra data if needed). Ideas may be hard or later dropped.
5. For **each** idea, state clearly:
  - **(1) Title / overall goal**
  - **(2) High-level research questions**
  - **(3) High-level solution steps** per question (no precise algorithm write-up)

**Grading:** creativity and clarity only.

---

## 3. Introduction to the selected dataset

### 3.1 The real-world contest

Every week *The New Yorker* publishes a **cartoon without a caption**. Readers submit one-line jokes. Editors filter spam/offense, then a **crowdsourced rating site** collects votes. Editors use those votes (and their taste) to pick the official top three.

Voting scale (typical in this dump):

- `1` = not funny  
- `2` = somewhat funny  
- `3` = funny

A **bandit / UCB-style** algorithm shows “promising” captions more often, so funny lines get **many more votes** than mediocre ones. Average scores and sample sizes are **not** a simple random sample of readers.

Contest began in 2005 (~900+ contests by the paper’s writing). Large-scale public ratings exist for roughly **contests 530–895** (on the order of **365** cartoons).

### 3.2 What the research dump contains

The paper *Humor in AI* (Zhang et al., 2024, [arXiv:2406.10522](https://arxiv.org/abs/2406.10522)) packages:

- **~365** cartoons + contests  
- **~2.2 million** captions  
- **~250 million+** human ratings

Local layout (your `data/newyorker_caption_contest/`):

- `data/{contest_id}.csv` — one file per week  
- `images/{contest_id}.jpg` — the cartoon

Typical CSV columns:


| Column                                   | Meaning                           |
| ---------------------------------------- | --------------------------------- |
| `caption`                                | submitted one-liner               |
| `mean`                                   | average rating (about 1–3)        |
| `precision`                              | uncertainty of that mean          |
| `votes`                                  | how many times it was shown/rated |
| `not_funny` / `somewhat_funny` / `funny` | vote counts                       |


Files are usually **sorted by** `mean` **descending**. The paper’s *Humor in AI* benchmark (GPT-4 vs humans, RLHF/DPO) is **optional inspiration**, not a required method for ADA.

### 3.3 What this dataset is “for” (ADA view)

Each row is: **this drawing + this sentence + how funny the crowd said it was**.  
You can study humor as **text**, as **preference / ranking**, as **time / culture** (week by week), and (harder) as **vision + language**.

**Caveats to mention in any serious proposal:**

- Bandit bias: high `mean` captions are oversampled.  
- Editor ≠ crowd: official winners may differ from the rating leaderboard.  
- Humor is subjective and culturally dated.  
- Images exist; P1 does not require a vision model.

---

## 4. Submission

Idea 1 — Anatomy of a Punchline: What Makes the Crowd Laugh?

Goal: Identify linguistic features associated with highly rated captions while separating general writing patterns from cartoon-specific effects.

RQ1: Are caption length, dialogue, sentiment, wordplay, named entities, or a late “punch word” associated with better within-contest ranks?
Solution: Combine the contest files, extract interpretable text features, and compare their distributions across top-, middle-, and bottom-ranked captions. Use votes and precision only to represent rating reliability.

RQ2: Are these relationships stable across contests, or does each cartoon reward a different style?
Solution: Estimate feature relationships separately across contests or cartoon themes, then compare their directions and strengths.

RQ3: Can text features distinguish top-ten captions from median captions without seeing the cartoon?
Solution: Train a simple classifier with contest-aware data splits and inspect its errors to determine what text alone cannot explain.

Idea 2 — Can We Trust the Leaderboard? Exposure Bias and Ranking Stability

Goal: Investigate whether captions can be ranked reliably when an adaptive bandit gives promising captions many more votes than others.

RQ1: How unequal is voting exposure, and how is it related to mean rating and precision?
Solution: Verify vote totals, reconstruct mean ratings from the three rating categories, and compare votes, means, and precision within and across contests.

RQ2: Do apparent top captions remain top after accounting for uncertainty?
Solution: Estimate rating uncertainty using resampling or a probability model, then calculate each caption’s probability of belonging to the top ten.

RQ3: How many votes are needed for a caption’s estimated quality to become stable?
Solution: Repeatedly downsample highly rated captions and measure how often their ranks change, comparing stable and unstable contests.

Idea 3 — Does the Joke Fit the Picture?

Goal: Test whether successful captions fit a cartoon while remaining surprising enough to reinterpret it—an “incongruity sweet spot.”

RQ1: Are top-ranked captions more semantically connected to visible objects and settings than low-ranked captions?
Solution: Select a representative cartoon sample, create validated scene descriptions manually or with a pretrained model, and compare image–caption similarity across rating percentiles.

RQ2: Is humor highest at an intermediate similarity level rather than at either extreme?
Solution: Relate similarity to within-contest rank using linear and curved relationships while accounting for caption length, rating reliability, and contest.

RQ3: Which visual themes support the greatest diversity of successful interpretations?
Solution: Group cartoons into manually checked themes and compare the rating distribution and semantic diversity of their top captions, illustrating results with selected examples.