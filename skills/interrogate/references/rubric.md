# Review Rubric

Score each dimension 1-5. 1 is broken, 5 is something you'd point at in a promo packet. 3 is fine — ship it, file the rest as follow-ups.

The number is a communication tool, not a grade. The comments are what matter. If you catch yourself writing "3" for everything, you're not looking.

## Dimensions

### Correctness — does it do what it claims?

- **1:** Doesn't work. Wrong output, crashes, fails the author's own tests. Or the tests pass and the feature is still wrong because the tests test the mocks.
- **2:** Works for the happy path the author tried. Breaks on empty input, missing fields, the other user's data, or the thing the ticket mentioned in paragraph 3.
- **3:** Does what the ticket says. Edge cases exist but they're documented or tracked. You'd ship this.
- **4:** Handles the cases a careful engineer would think of. Errors are specific. Invalid states are unrepresentable. The tests would catch a regression in the thing this PR is about.
- **5:** Correct in ways that required understanding the problem, not just the ticket. The kind of code where the bug report comes in six months and you look at it and go "no, that's handled."

### Design — will this still make sense in six months?

- **1:** Wrong abstraction. The new code fights the existing architecture, or invents a parallel one. You'll rewrite this. Not "might rewrite" — will.
- **2:** Works but the shape is off. Responsibilities are confused (this function does I/O and formatting and validation), or the new type duplicates one that exists, or the API will need breaking changes the moment a second consumer appears.
- **3:** Fits. A new teammate could find the right file and make a change without first attending a meeting about "how we do things." The module has a reason to exist.
- **4:** Improves the design of the surrounding code. Deletes more than it adds, or makes the next feature obvious. The kind of change that makes the file *after* the PR better than the file *before*, not just different.
- **5:** Changes how you'll approach the next three problems. You'd send this PR to someone and say "this is how we do X now."

### Scope — is this the right amount of change?

- **1:** Can't tell what this PR is about. Mixes a feature, a refactor, a dependency bump, and a drive-by rename. Or it's 2,000 lines that should have been four PRs. Review is theater because nobody can actually hold this in their head.
- **2:** There's a real change in here but it's padded. Unrelated files, leftover debug, a refactor that isn't needed for the thing being shipped. The description says "also cleaned up a few things."
- **3:** One thing. The diff is the size it needs to be. You could revert this PR and the rest of the codebase wouldn't notice, which is the point of a PR.
- **4:** Tight. Every file is there because it has to be. The author clearly asked "do I need this" and the answer was no more than once. Commit history is coherent enough that `git bisect` would land on a useful commit.
- **5:** Surgical. You look at the diff and understand the whole change in one sitting. Nothing you'd cut. Nothing you'd add. The PR description is shorter than the diff and that's correct.

### Tests — will we know if this breaks?

- **1:** No tests, or tests that assert mocks were called, or tests that pass when the feature is broken. Coverage theater. The test file exists so the CI check is green.
- **2:** Tests exist for the happy path. They'd catch a rename. They wouldn't catch a logic error, an off-by-one, or the bug this PR is supposedly fixing. Fixtures are so specific that any code change breaks the test without the behavior changing.
- **3:** Tests cover the behavior this PR introduces, including the failure case. A future change that broke this feature would fail a test with a name you'd understand. That's the bar.
- **4:** Tests are documentation. You could delete the implementation comments and the tests would tell you what this module does and what it refuses to do. Edge cases have names like `rejects expired tokens` not `test7`.
- **5:** The tests made you understand the feature better than the PR description did. They encode the invariants. They're the reason you're confident shipping this on a Friday.

### Operations — can we run this, debug it, and survive it breaking?

- **1:** No thought given. Unbounded query, no timeout, no error handling, secrets in the diff, a migration that locks the table. This will page someone and they won't know why.
- **2:** It'll run. It'll also be mysterious when it doesn't. No logs at the decision points, no metric for the thing this feature exists to do, errors swallowed or rethrown as `Internal Server Error`. The dashboard won't show this.
- **3:** Runnable. Errors surface. There's a log or a metric at the boundary (request came in, job finished, user-visible failure). You could debug a production incident involving this code without first adding instrumentation.
- **4:** Observable by design. Feature-flagged if it's risky. Rollback is `revert` or flip a flag, not "deploy the previous SHA and hope the migration is backwards-compatible." Alerts would fire on the failure mode that actually matters, not on CPU.
- **5:** You'd let this run unattended. The failure modes were named in the PR. There's a runbook, or the code is so straightforward you wouldn't need one. Capacity, backpressure, and "what if the dependency is down" were answered before you asked.

## How to use this

You are not averaging these into a score. You're using them to force a conversation about the dimension that's actually the problem.

Most PRs that feel "off" are a 2 in one dimension and a 3-4 in the others. Name the 2. That's the review. "Looks good but the tests wouldn't catch the bug this is fixing" is a better review than five paragraphs of nits about naming.

A 1 in any dimension is a request for changes, not a suggestion. A 2 is "I would like this fixed, here's why, I'll defer to you if you disagree with a reason." A 3+ ships.

Don't score a dimension you didn't look at. "Tests: 3" on a PR you didn't run the tests for is a lie. Write "not evaluated" instead.

## Output

After walking the dimensions (and after the other reviewers have gone — you are last):

```
## Verdict: [SHIP / FIX THEN SHIP / REWORK]

### Scores
- Correctness: N — [one sentence]
- Design: N — [one sentence]
- Scope: N — [one sentence]
- Tests: N — [one sentence]
- Operations: N — [one sentence]

### What would change my mind
[The one thing that, if addressed, would move a FIX to a SHIP, or a REWORK to a FIX. If it's SHIP, skip this.]

### Notes
[Anything that didn't fit a dimension. Praise belongs here too.]
```

SHIP: merge it. FIX THEN SHIP: specific, bounded changes; don't start over. REWORK: the approach is wrong; talking about line comments is a waste of everyone's time until the shape changes.

If you write REWORK, you owe an alternative. "This isn't it" without "here's the shape that would be" is just a veto.
