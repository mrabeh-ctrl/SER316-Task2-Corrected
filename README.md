# Number Guessing Game

## Project Description
A Java number guessing game. The player tries to guess a randomly generated number.

## Branch Structure at the Start of the Project
- **main** - stable production branch (initial number guessing game).
- **dev** - integration branch for features (adds an encouraging message for players).
- **feature1** - 4 commits: quit with a negative number, play-again loop, improved guess feedback, version comment.
- **feature2** - branched from dev: adds a maximum number of attempts and a game-over condition.
- **feature3** - 4 messy commits: adds a hint system that shows proximity after several attempts.
- **hotfix** - 1 commit: fixes `Utils.randomInt` so the maximum value is included in the range.

## Git Learning Summary

### Differences between merge, rebase, squash, and cherry-pick
- **Merge** joins two branches and records it with a merge commit that has two parents. History is preserved exactly as it happened, but it becomes non-linear.
- **Rebase** replays a branch's commits on top of another branch, creating new commits with new hashes. History stays linear, but the information about where the work branched off is lost, and it rewrites history, so it is only safe on branches nobody else uses.
- **Squash** (`rebase -i` with `squash`) combines several commits into one, with a single clean message. It removes noisy intermediate commits but also the step-by-step development detail.
- **Cherry-pick** copies one specific commit onto the current branch as a new commit (new hash). It does not bring along anything else from the source branch.

### What I observed in the history
- **feature1 (merge):** I merged `dev` into `feature1` and resolved one conflict in `Main.java` (the "Good luck!" line vs. the version comment). The result is a merge commit with two parents, and the 4 original commits stayed visible. `dev` then fast-forwarded to it.
- **feature2 (rebase):** Rebasing onto `dev` stopped twice, once per commit, with conflicts in `GameEngine.java`, `GameUI.java`, and the tests. My commits got new hashes and sit in a straight line on top of `dev`, with no merge commit. I later merged `dev` back into it (after `feature3` landed), which fast-forwarded into `dev`.
- **feature3 (squash + rebase):** The 4 messy commits ("started hint", "got it done", "had to fix", "done") became one commit, "Add hint system to show proximity after 3 attempts". Because I squashed first, I only resolved the conflict once when rebasing onto `dev`. It then merged into `dev` as a fast-forward.
- **hotfix (cherry-pick):** The hotfix commit was copied onto `main` as a new commit with a different hash. Merging `main` into `dev` then brought it into the development line without conflicts.

### When I would use each strategy in real projects
- **Merge:** when bringing `dev` into a shared feature branch, or integrating finished work where the history of how it came together matters.
- **Rebase:** to update my own private feature branch with the latest `dev` before opening a PR. Never on shared branches like `dev` or `main`.
- **Squash:** before merging a feature with many "wip" or "fix typo" commits, so `dev` gets one meaningful commit per feature.
- **Cherry-pick:** for urgent fixes that must go to `main` (production) right now, without pulling in unfinished work from `dev`. The fix should then be merged back into `dev`.

### Lessons learned
Run the tests after every conflict resolution, before continuing a merge or rebase. When both sides add features, the correct resolution usually keeps both (for example, the quit option, max attempts, and hints all live together in `GameEngine`).
