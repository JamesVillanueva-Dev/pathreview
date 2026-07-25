## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/156

**Issue title:** README scorer test fixture is too short for its own word-count assertion

**Tier:** [Y] Tier 1  [ ] Tier 2  [ ] Tier 3

**Selection reasoning:**
I chose this Tier 1 issue because it is a focused testing problem and matches my current comfort level with the codebase. The scope is small enough for a first contribution: I can inspect one README scorer test, compare the fixture text to the expected word-count behavior, and update the test data or assertion without changing unrelated application logic.

**Checklist reasoning:**
I can explain the issue in my own words: one README scorer test expects a comprehensive README result, but the sample README is too short for that expectation. The affected code is in `tests/unit/test_readme_scorer.py`, with related word-count behavior in `ingestion/parsers/readme_parser.py`. Done means `pytest tests/unit/test_readme_scorer.py -q` should pass because the fixture and assertion describe the same expected behavior. This is a realistic Tier 1 issue because it is a localized test fix, should take a few focused hours, and the GitHub issue does not list blockers or unresolved dependencies.

**Problem summary:**
The README scorer test is supposed to verify that a strong README is counted as "comprehensive." Right now, the test fixture is only about 51 words, but the test expects the scorer to report more than 100 words. Because of that mismatch, the test fails even if the README scorer is behaving correctly. A successful fix would make the test fixture and assertion agree, either by lengthening the sample README enough to meet the comprehensive threshold or by correcting the expected result so the test validates the intended behavior.

**Branch name:** test/156-readme-scorer-fixture

**Setup confirmation:** [Y] App runs locally at localhost:5173

**Cohort ledger:** [Y] Issue added to cohort ledger


## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [paste commit URL here after pushing]

**Reproduction summary:**
I ran `pytest tests/unit/test_readme_scorer.py -q` in my local venv. One test,
`test_readme_with_all_quality_signals`, failed with `assert 51 > 100`. The sample README in
the test is only 51 words, but the test expects more than 100 words and a "comprehensive"
category, so the assertions and the fixture don't match.

**PLAN.md link:** https://github.com/JamesVillanueva-Dev/pathreview/blob/test/156-readme-scorer-fixture/PLAN.md

**Walkthrough video (recommended):** [paste Loom link here, optional]

**Blockers or open questions:**
