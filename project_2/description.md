# ADA CS-401 — Milestone P2: Project proposal & initial analyses

**Official page:** [epfl-ada.github.io — Projects](https://epfl-ada.github.io/teaching/fall2026/cs401/projects/)

Working copy of the staff requirements for P2. Use this as a checklist while you build the repo deliverable.

---

## 1. What P2 is for

In **Milestone P2**, your team agrees on and **refines** the project proposal. You must **choose one project direction** and show that it is **feasible** with your dataset (and any extra data you bring in).

Goals for this milestone:

- Get **intimately acquainted** with the data.
- **Preprocess** it and complete **descriptive statistics**.
- Have a **documented pipeline** in a notebook.
- Show **clear project goals** and a credible plan for P3.

Even though course staff provide datasets, **feasibility is your responsibility** at this stage.

---

## 2. What to demonstrate in the notebook

When you describe the data (and any additional datasets), show at least the following (non-exhaustive):

- [ ] You can **handle the data at its full size** (memory, runtime, storage).
- [ ] You **understand what is in the data**: formats, distributions, missing values, correlations, etc.
- [ ] You considered ways to **enrich, filter, and transform** the data for your questions.
- [ ] You have a **reasonable plan for methods**, with **essential mathematical details** in the notebook.
- [ ] Your **analysis and communication plan** is sound; mention **alternatives you considered and dropped**.

Use P2 to **check with staff** that idea and feasibility are on track **before** you invest heavily in P3. Project **office hours** are there to help.

---

## 3. Grading criteria

Staff evaluate:

| Area | What they look for |
|------|---------------------|
| **Process** | Steps above done and **documented** |
| **Code** | Quality and **documentation** |
| **Project** | Feasibility and **critical awareness** |
| **Idea** | Clarity, reasonableness, depth of thought |

---

## 4. Where you work

- One **team GitHub repository** under the **[epfl-ada](https://github.com/epfl-ada)** organization.
- By the **P2 deadline**, that repo must contain the proposal and initial analysis code.

---

## 5. Deliverables (team)

### 5.1 `README.md` — detailed proposal (≤ 1000 words)

Include these sections:

1. **Title**
2. **Abstract** (~150 words) — project idea and goals; motivation; what story you want to tell and why.
3. **Research questions** — list of questions you plan to address.
4. **Proposed additional datasets** (if any) — how you will obtain, manage, process, and enrich them; show you read docs/examples; note size and format; confirm feasibility.
5. **Methods**
6. **Proposed timeline**
7. **Organization within the team** — internal milestones up to **P3**
8. **Questions for TAs** (optional)

### 5.2 Repository structure & code

- Repo **well structured**; all code for **initial analyses** and **data-handling pipelines**.
- Follow the **course project template** repository for layout (see official project page / staff instructions).

### 5.3 Jupyter notebook — initial results

- **One main notebook** with the primary results (this is what we grade for narrative + outputs).
- **Core logic** in **external scripts/modules** imported/called from the notebook.

Graded on:

- Correctness
- Quality of code
- Quality of textual descriptions

---

## 6. Quick checklist before deadline

- [ ] `README.md` complete (all sections, ≤ 1000 words)
- [ ] Repo under `epfl-ada`, sensible folder layout
- [ ] Pipeline notebook + external modules
- [ ] Data scale, EDA, preprocessing, and method sketch documented
- [ ] Team milestones through P3 listed in README
