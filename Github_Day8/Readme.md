Today we will cover 2 essential Devops tasks:

i) git cherry-pick: Extracting a specific hotfix commit from one branch and applying it to another without merging entire branches.

ii) Git tagging (git tag): Creating immutable semantic version milestones (eg. v1.0.0) for production releases and pipeline triggers

Part 1: Cherry Picking (git cherry-pick)

Imagine a bug is fixed on experimental dev branch, but dev contains other unapproved features. You only want that single bug fix commit in main.

dev:   A ─── B (Hotfix: fix-auth) ─── C (Unfinished Feature)
              │
              └─── [git cherry-pick <hash-of-B>] ───> main: A ─── B'

