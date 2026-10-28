# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall Accuracy

<!-- Your criterion. It must name a number. -->
The model should achieve a minimum of 0.65 accuracy on the test set across all three seeds.


**Why this target:**
This is because if the model was to just just randomly pick from one of the 3 labels for each data point, it should get around a 0.33 accuracy, so having a goal of 0.65 accuracy allows us to gauge that the model performs substantially better (2x) than just picking randomly.


---

## 2. Minimum Label Counts

<!-- Your criterion. -->
Each label must represent 25% of the total dataset, meaning they should each have at least 50 datapoints. 


**Why this target:**
I did this because we need to create a large enough dataset such that a single incorrect prediction won't completely throw off the model's performance. By getting at least 50 points for each label, we allow the model to have enough samples to have an established "profile" of each label


---

## 3. Maximum Label Counts

<!-- Your criterion. -->
No single label should represent more than 50% of the total dataset, meaning there should be no label with more than 100 samples


**Why this target:**
We do this because we don't want the model to have too many posts within its training data of a single label such that it overfits to that singular label instead of learning the patterns of other labels. 


---

## 4. Confident Predictions should be accurate

<!-- Your criterion. -->
The top third most confident predictions must have an accuracy rate of 0.7 across all three seeds


**Why this target:**
The model's confidence is only worthwhile if that confidence also correlates with the actual outcome of those predictions. High confidence with low accuracy means nothing but poor performance


---

## 5. Confidence must be a reliable metric

<!-- Your criterion. -->
The accuracy of the top third most confident predictions must be at least 15% more accurate than that of the least confident third of predictions across all three seeds


**Why this target:**
If the lower confidence predictions don't have a significantly drop off in accuracy, then there is no reason for confidence to be tracked at all, as it would not be a reliable source of new information about the predictions.


---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
