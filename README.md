# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `analysis`

**Definition:** A post whose main conclusion is supported by a relevant, checkable fact or statistic and reasoning that connects the evidence to the conclusion.

**Example 1:**
> Team Metrics Through 10 Games 10 games probably a big enough sample to look at some team-level performance metrics. Biggest takeaway is... yikes? I mean anyone who watches the games could have told you it was this bad, but nonetheless interesting to quantify relative to the rest of the league. The 5v5 play is as bad as it looks, in fact goaltending has been propping up the defense slightly it looks like. The Ducks' big losing streak(s) last year had far worse results than process IIRC so there was some hope it would turn around - this feels different. Teams have bad 10 game stretches of course, it's a long season, but they need to figure it out fast. Huge game in Philly tomorrow, gotta show up in that environment.

**Example 2:**
> 5 months ago our Zone Time metrics had improved from worst, to middle of the pack. This team is now top 10 in two zones and top 12 in another. 5v5 is even better. I think we all expected improvement, but I dont know if we were ready for something this drastic.

### `hot_take`

**Definition:** A user making unsupported claims about the team or the team's expected performance or a topic closely related to the team.

**Example 1:**
> What’s your biggest hot take as a Ducks fan? I’ll start: The webbed D logo is not as bad as people say: the Mighty Ducks jersey just make our current jerseys worse.

**Example 2:**
> Ducks in 6. Let’s go Anaheim!!

### `reaction`

**Definition:** A user's first impressions on recent team events, announcements, or speculation.

**Example 1:**
> Anaheim ducks HTR leak? Not Confirmed / Speculation  The Jersey itself is expected to be green, but I do like the purple.

**Example 2:**
> It’s been a hell of a run Remember we weren’t even expected to make playoffs, let alone win round 1. Disappointed in how the season ended but hey ducks always fly back north after things go south.

### The hardest boundary

**Which two labels:**
analysis and reaction
**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
If a post uses a specific, checkable statistic to support its claim, then label it analysis, even if it contains an emotional reaction. Only use the Reaction label when the post primarily communicates feelings about a recent event without evidence-based reasoning.

**Borderline Post**
> “Spreadsheet Hockey — I’m coping so incredibly hard right now, but over the last 10 and 20 games the Ducks are first in Expected Goals at five-on-five.”
---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
r/anaheimducks
**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I labelled the first 20 posts cold, using only my written label definitions. The remaining 180 posts were pre-labelled with an automated, taxonomy-based heuristic. I then read every pre-labelled post, corrected inaccurate labels, and applied the analysis-versus-reaction decision rule consistently across the full dataset.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `analysis` | 54 | 27% |
| `hot_take` | 50 | 25% |
| `reaction` | 96 | 48% |
| **Total** | **200** | **100%** |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* “Our first three opponents were so brutal. To go 2-1 is pretty good with that schedule. 2023 Stanley Cup champ, 2026 Cup Finalist; 2024 and 2025 Cup Champs; 2024 and 2025 Cup Finalist with the best player in hockey. If anyone else had a tougher first three games, I’d love to hear it. Our young guns should be a playoff squad again. Probably won’t win the division but no reason we shouldn’t do well.”
>
> *Could have been:* `analysis` or `hot_take`
>
> *I chose, because:* I chose `hot_take`. The post gives checkable facts about the opponents, but those facts do not establish its main prediction that the Ducks should make the playoffs again.

**2.**
> *The post:* “What is a successful end of regular season stat line for a $18 million 1st line center? What would the floor be for a successful first season under this contract? I think 40 goals/60 assists would destroy any narrative that he failed to meet the contract expectations. My actual prediction is 25 goals/50 assists. What do you think?”
>
> *Could have been:* `analysis` or `hot_take`
>
> *I chose, because:* I chose `hot_take`. The post includes numbers, but they are proposed standards and predictions rather than evidence used to support a conclusion.

**3.**
> *The post:* “How we feeling? Preseason Game 1 and dominated over the Sharks with a 6-2 victory with goals from LaCombe, Carlsson, and Lineau AND a hat trick from the Bucket himself. Our PP looks pretty good so far as well in my opinion. Thoughts and feelings?”
>
> *Could have been:* `reaction` or `hot_take`
>
> *I chose, because:* I chose `hot_take`. Although it responds to a recent game, its central claim evaluates the power play from a single preseason result, so I treated it as an unsupported performance claim rather than only an immediate emotional response.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, a learning rate of `2e-5`, a batch size of 16, seed 42, and a maximum input length of 128 tokens. The run used the Apple Silicon GPU through MPS.

**Anything I changed from the defaults, and why:**
I kept all of the default training hyperparameters. I only set the label list to match the three labels in my dataset: `analysis`, `hot_take`, and `reaction`. Keeping the defaults gave me a clear initial result that I can compare with later runs without introducing several changes at once.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->
The stratified split produced 139 training posts, 31 validation posts, and 30 test posts. The test set contained 8 `analysis`, 8 `hot_take`, and 14 `reaction` posts. The two smaller labels had only 8 test examples each, so their F1 scores may change substantially across seeds.


---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Pre-labeling selected subreddit posts
- *What came back:* Expected labels following the definitions in this readme
- *What I changed:* I changed some of the labels to be reactions instead of analysis because specific numbers weren't really mentioned in some posts that were marked as analysis, and instead just eyeball tested from fans who don't know the game well. 

**Moment 2**

- *What I asked for:* Help deciding difficult boundary posts
- *What came back:* Reasonings for whether some of the hard label boundary posts were hot takes or reactions
- *What I changed:* The pre-labeled labels to hot_takes if I agreed with the reasoning for that

**Pre-labelling disclosure:** I labelled the first 20 posts independently without AI assistance. The remaining 180 posts were pre-labelled using an automated taxonomy-based heuristic based on my written definitions. I then personally read every pre-labelled post and corrected labels that did not match my definitions. The final labels reflect my decisions, not the automated suggestions.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.200 | 0.500 | +0.300 |
| Macro F1 | 0.200 | 0.289 | +0.089 |
| F1 — `analysis` | 0.222 | 0.200 | -0.022 |
| F1 — `hot_take` | 0.267 | 0.000 | -0.267 |
| F1 — `reaction` | 0.111 | 0.667 | +0.556 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
     I expect that the trained model will beat the baseline in the accuracy parameter.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
The fine-tuning didn't add to the hot_take or analysis accuracy, in fact it worsened the accuracy of the analysis label. The reaction label did see significant improvement, however.


---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.500 | 0.467 | 0.633 | 0.167 |
| Macro F1 | 0.289 | 0.212 | 0.480 | 0.268 |
| F1 — `analysis` | 0.200 | 0.000 | 0.667 | 0.667 |
| F1 — `hot_take` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `reaction` | 0.667 | 0.636 | 0.774 | 0.138 |

Accuracy moved 0.167 across seeds — more than 10 points. Your dataset is too small or too lopsided for a stable measure. That IS a diagnosis. Write it down.

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
