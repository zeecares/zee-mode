# calibrate trigger evals

A skill only helps if it loads when it should and stays out of the way when
it should not. These prompts test the `description` line in `SKILL.md`, not
the loop itself.

How to run: give an agent the descriptions of every skill in this repo (not
their bodies), send each prompt in a fresh session, and record whether it
picks `calibrate`. Every should-trigger prompt picks it; no should-not-trigger
prompt does. When you change the description, rerun both lists. When a real
request misfires, add it here.

## Should trigger

1. We use an LLM to grade support replies for tone. How do we know its
   grades match what our reviewers would say?
2. I rewrote the rubric for our faithfulness judge. What do I need to check
   before it goes back into CI?
3. Our judge passes 97% of outputs and nobody trusts it. Where do we start?
4. Set up a labelled dev set and a held-out test set for our summarization
   judge.
5. The model provider shipped a new version. Does our judge need to be
   rechecked?
6. Our scoring API only returns a probability per label, no reasoning. How
   should we set thresholds and handle low-confidence cases?
7. Compute kappa, TPR, and TNR between my labels and the judge's verdicts.
8. We want to tune the judge prompt automatically with an optimizer. What
   data is it allowed to see?

## Should not trigger

1. Write unit tests for the date parser. (deterministic check, no judge)
2. Review this diff and find what could break. (interrogate)
3. Prove the signup flow works end to end in the real app.
   (create-verification-skill)
4. Which eval platform should we buy to store traces and scores?
   (tooling choice, not judge alignment)
5. Our model's accuracy on the benchmark dropped from 81% to 74%. Why?
   (model regression, no judge involved)
6. Label these 200 support tickets by topic. (labelling work, no judge being
   measured)
7. Pick the better of these two product names. (a one-off opinion)
8. Simplify this retry wrapper before we add a second backend.
   (a principle, not a skill)
