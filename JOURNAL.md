## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/156

**Issue title:** README scorer test fixture is too short for its own word-count assertion

**Tier:** [Y] Tier 1  [ ] Tier 2  [ ] Tier 3

**Selection reasoning:**
I chose this Tier 1 issue because it is a focused testing problem and matches my current comfort level with the codebase. The scope is small enough for a first contribution: I can inspect one README scorer test, compare the fixture text to the expected word-count behavior, and update the test data or assertion without changing unrelated application logic.

**Problem summary:**
The README scorer test is supposed to verify that a strong README is counted as "comprehensive." Right now, the test fixture is only about 51 words, but the test expects the scorer to report more than 100 words. Because of that mismatch, the test fails even if the README scorer is behaving correctly. A successful fix would make the test fixture and assertion agree, either by lengthening the sample README enough to meet the comprehensive threshold or by correcting the expected result so the test validates the intended behavior.

**Branch name:** test/156-readme-scorer-fixture

**Setup confirmation:** [Y] App runs locally at localhost:5173

**Cohort ledger:** [Y] Issue added to cohort ledger
