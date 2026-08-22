Today we will cover 2 essential Devops tasks:

i) git cherry-pick: Extracting a specific hotfix commit from one branch and applying it to another without merging entire branches.

ii) Git tagging (git tag): Creating immutable semantic version milestones (eg. v1.0.0) for production releases and pipeline triggers

Part 1: Cherry Picking (git cherry-pick)

Imagine a bug is fixed on experimental dev branch, but dev contains other unapproved features. You only want that single bug fix commit in main.

dev:   A ─── B (Hotfix: fix-auth) ─── C (Unfinished Feature)
              │
              └─── [git cherry-pick <hash-of-B>] ───> main: A ─── B'


Part 1: Cherry-Picking (git cherry-pick)
Imagine a bug is fixed on the experimental dev branch, but dev contains other unapproved features. You only want that single bugfix commit in main.

dev:   A ─── B (Hotfix: fix-auth) ─── C (Unfinished Feature)
              │
              └─── [git cherry-pick <hash-of-B>] ───> main: A ─── B'
Hands-on Step:
Switch to dev and make a specific fix:

git switch dev
echo "Fixing critical auth timeout" >> deploy.sh
git add deploy.sh
git commit -m "fix: Resolve critical authentication timeout"
Copy the commit hash of this new fix:

git log --oneline -n 1
(Copy the 7-character hash, e.g., a1b2c3d).

Switch to main and apply only that commit:

git switch main
git cherry-pick <PASTE_COMMIT_HASH>
Verify that main now has the fix without merging the rest of dev:

git log --oneline -n 2


Part 2: Release version tagging (git tag)

In DevOps pipelines, deployment triggers are often linked to Git Tags following Semantic Versioning (v<Major>.<Minor>.<Patch>).

Types of Tags:
Lightweight Tag: A simple pointer to a commit.

Annotated Tag (Enterprise Standard): Stores the author, date, and a release description message.