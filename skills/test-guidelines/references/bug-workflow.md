# Reviewing a suspected bug

Read this when investigating a report that something might be broken.

1. Trace the actual code path and check that the reported behavior is possible.
2. When practical, reproduce it by hand or with the smallest test that shows it.
3. Classify the finding: a reproduced bug, a plausible but unverified risk, or a design improvement.
4. Add a regression test when the production code can actually produce the defect and the test reproduces it for the right reason.

# Fixing a bug

Read this when fixing a confirmed bug.

1. Find or write a focused test that reproduces the bug and fails for the right reason.
2. Confirm the failure shows the actual defect.
3. Fix the implementation.
4. Confirm the test now passes.
5. Confirm the test would have caught the original defect.
6. Run the surrounding suite and make sure unrelated behavior is unchanged.
