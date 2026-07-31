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


**Blockers:**
none

---

### Check-in 2 (end of week)

**PR link:** not added yet

**Branch:** `test/156-readme-scorer-fixture`

**What you built:**
I fixed a unit test whose fixture didn't match its own assertions. The test built a 51-word
sample README but asserted the scorer would report more than 100 words and a "comprehensive"
category (which needs 500+). I grew the fixture to 218 words of realistic README prose and
changed the expected category to "adequate", so the fixture and the assertions now describe
the same README. The scorer itself was behaving correctly, so I didn't change it.

**Tests added or updated:**
`tests/unit/test_readme_scorer.py` — updated `test_readme_with_all_quality_signals` only. It
covers a strong README hitting all the quality signals: word count over 100, adequate
category, installation / usage / badges / demo / tech-stack all detected, and an overall score
above 0.7 (it now scores 0.92). No new test was needed; the word-count buckets are already
covered by the three `test_word_count_category_*` tests.

**Self-review confirmation:** [ ] make check passes  [ ] make test-unit passes

I left both unchecked because neither passes on a clean checkout of this repo, before my
change. I couldn't turn them green without editing a lot of code unrelated to my issue, so
instead I checked that my change doesn't make anything worse:

- `pytest tests/unit/test_readme_scorer.py -q` — 23 passed. The file I changed is green.
- `make check` — 182 lint errors before my change and 182 after, so I added none. Two of them
  are in files I touched; I left them alone so the PR stays focused on the issue.
- `make test-unit` — 53 failures before, 52 after. The one difference is the test I fixed.
- The pre-commit `mypy` hook fails on this test file with 24 "missing type annotation" errors
  both before and after my change, so it needs `--no-verify` to commit.

**Draft PR feedback received from:** none
