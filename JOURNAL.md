## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/156

**Issue title:** README scorer test fixture is too short for its own word-count assertion

**Tier:** [Y] Tier 1  [ ] Tier 2  [ ] Tier 3

**Selection reasoning:**
I chose this Tier 1 issue because it is a focused testing problem and matches my current comfort level with the codebase. The scope is small enough for a first contribution: I can inspect one README scorer test, compare the fixture text to the expected word-count behavior, and update the test data or assertion without changing unrelated application logic.

**Checklist reasoning:**
I can explain the issue in my own words: one README scorer test expects a comprehensive README result, but the sample README is too short for that expectation. The affected code is in `tests/unit/test_readme_scorer.py`, with related word-count behavior in `agent/tools/readme_scorer.py`. Done means `pytest tests/unit/test_readme_scorer.py -q` should pass because the fixture and assertion describe the same expected behavior. This is a realistic Tier 1 issue because it is a localized test fix, should take a few focused hours, and the GitHub issue does not list blockers or unresolved dependencies.

**Problem summary:**
The README scorer test is supposed to verify that a strong README is counted as "comprehensive." Right now, the test fixture is only about 51 words, but the test expects the scorer to report more than 100 words. Because of that mismatch, the test fails even if the README scorer is behaving correctly. A successful fix would make the test fixture and assertion agree, either by lengthening the sample README enough to meet the comprehensive threshold or by correcting the expected result so the test validates the intended behavior.

**Branch name:** test/156-readme-scorer-fixture

**Setup confirmation:** [Y] App runs locally at localhost:5173

**Cohort ledger:** [Y] Issue added to cohort ledger


## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/JamesVillanueva-Dev/pathreview/commit/b93c24795741757bca0f57b2ec2581bd267d6ca9

**Reproduction summary:**
I ran `pytest tests/unit/test_readme_scorer.py -q` in my local venv. One test,
`test_readme_with_all_quality_signals`, failed with `assert 51 > 100`. The sample README in
the test is only 51 words, but the test expects more than 100 words and a "comprehensive"
category, so the assertions and the fixture don't match.

**PLAN.md link:** https://github.com/JamesVillanueva-Dev/pathreview/blob/test/156-readme-scorer-fixture/PLAN.md

**Walkthrough video (recommended):** [paste Loom link here, optional]

**Blockers or open questions:**


## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
The fix is implemented. I worked through the steps in PLAN.md: I reproduced the failure, grew
the sample README in `test_readme_with_all_quality_signals` from 51 words to 218 words, and
changed the category assertion from `"comprehensive"` to `"adequate"` since 100–499 words is
the adequate range. I kept every quality signal in the fixture (installation, usage, features,
tech stack, two badges, demo link) so the other assertions still pass. I also fixed the wrong
file reference in my Week 7 entry — the word-count logic is in `agent/tools/readme_scorer.py`,
not `ingestion/parsers/readme_parser.py`. `pytest tests/unit/test_readme_scorer.py -q` is now
23 passed.

**Next steps:**
Remaining PLAN.md steps are the verification and submission ones: re-run `make check` and
`make test-unit` to record before/after numbers so I can show my change doesn't add failures,
push the branch, and open the PR against `ascherj/pathreview` using the repo's PR template —
filling in Summary, Issue (`Closes #156`), Changes, Testing, and Notes for Reviewers, including
why I chose "adequate" over padding the fixture to 500+ words.

**Blockers:**
None blocking the fix itself. One thing I had to work around: `make check` and `make test-unit`
already fail on a clean checkout of this repo, so I can't use "everything is green" as my
signal. Instead I measured before/after to show I added nothing new (numbers in Check-in 2).

---

### Check-in 2 (end of week)

**PR link:** https://github.com/ascherj/pathreview/pull/451

**Branch:** `test/156-readme-scorer-fixture`

**What you built:**
I fixed a unit test whose fixture didn't match its own assertions. The test built a 51-word
sample README but asserted the scorer would report more than 100 words and a "comprehensive"
category (which needs 500+). I grew the fixture to 218 words of realistic README prose and
changed the expected category to "adequate", so the fixture and the assertions now describe
the same README. The scorer itself was behaving correctly, so I didn't change it.

**Tests added or updated:**
`tests/unit/test_readme_scorer.py` — updated `test_readme_with_all_quality_signals` only. It
covers a strong README hitting all the quality signals: word count over 100 (the fixture is
now 218 words), `word_count_category == "adequate"`, installation / usage / badges / demo /
tech-stack all detected as True, and an overall score above 0.7 (it now scores 0.919). The
regression it locks in is the fixture/assertion mismatch from #156: the test now proves a
218-word README with every quality marker lands in the 100–499 "adequate" band instead of
asserting "comprehensive", which needs 500+. No new test was needed — the three
`test_word_count_category_minimal` / `_adequate` / `_comprehensive` tests (lines 147–175)
already cover the bucket boundaries with their own correctly-sized READMEs, so adding a fourth
would duplicate them.

**Self-review confirmation:** [ ] make check passes  [ ] make test-unit passes

I left both unchecked because neither passes on a clean checkout of this repo, *before* my
change — checking them would be a false claim. I couldn't turn them green without editing a
lot of code unrelated to #156, so instead I measured before and after to prove my change adds
no new failures:

| Check | Before my change | After my change |
| --- | --- | --- |
| `make test-unit` | 53 failed, 375 passed | 52 failed, 376 passed |
| `make check` (ruff) | 182 errors | 182 errors |
| `make typecheck` | fails in `numpy/__init__.pyi` before reaching project files | identical |

- `pytest tests/unit/test_readme_scorer.py -q` — 23 passed. The file I changed is fully green.
- `ruff check tests/unit/test_readme_scorer.py` — "All checks passed!". None of the repo's 182
  lint errors are in the file I touched, and I added none.
- The single `make test-unit` difference is the test I fixed. The other 52 failures are
  pre-existing and in unrelated files (`test_tech_detector.py`, etc.); I left them alone so the
  PR stays scoped to the issue.
- `make typecheck` can't complete: mypy errors out in the numpy stubs with "Type statement is
  only supported in Python 3.12 and greater" before it checks any project file. Same before and
  after my change. This also makes the pre-commit `mypy` hook fail, so committing needed
  `--no-verify`.

**Draft PR feedback received from:** none

## Week 10 — Iteration & reflection

### Reviewer feedback

**Feedback received:** [ ] Yes  [N] No — still awaiting review

**Summary of feedback:**
No feedback

**How you responded:**
[What changes did you make, or what did you reply? If no feedback,
leave blank.]

---

### Reflection

**What was harder than you expected?**
Setting up docker was harder than expected. Without AI, I probably would have struggled with the setup for hours.

**What did you learn about working in a large codebase?**
Working in a large codebase is really difficult. It is like trying to help on a topic that you have no idea about. 

**How did AI tools help — and where did they fall short?**
AI tools were helpful in guiding me on the correct steps whenever I got stuck. They were also helpful in familiarizing myself with a codebase. If I needed to know where something is, AI is a great tool for that. I can not recall any times where is fell significantly short. 

**What would you do differently if you started over?**
If I could start over, I would have chosen a more difficult problem to work on. My problem was fairly easy and in hindsight I could have taken on a more difficult problem. 

**What are you most proud of from this module?**
I am most proud of finishing my first Codepath course. I definitely plan to take more in the future. 