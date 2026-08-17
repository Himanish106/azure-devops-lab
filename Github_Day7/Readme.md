Git Rebasing & Squash Workflows

In a collaborative enterprise environment, the git log should tell a clean story. If your repository shows commits like:

"fixed typo"

"oops, forgot file"

"testing again"

"fixed typo for real this time"

...it makes it impossible for other engineers (or your future self) to understand the intent of the code changes. Today, we learn how to rewrite history to keep things professional.

The Roadmap
Interactive Rebasing (git rebase -i): The DevOps tool for "polishing" history.

Squashing Commits: Combining many messy work-in-progress commits into one meaningful "feature" commit.

When to Rebase vs. Merge: A critical distinction for professional Git hygiene.

Part 1: The Concept (Squashing)
Imagine you are working on a feature and you make 5 small, messy commits. Before you merge that PR into main, you should squash those 5 commits into 1 single, clean commit titled feat: Add Azure Application Gateway.



Hands-on — Interactive Rebase
Let's simulate a messy history and clean it up.

Step 1: Create a "Messy" History
Make sure you are on dev:

git switch dev
Create 3 small, messy commits:

echo "File 1" > file1.txt
git add .
git commit -m "work in progress"

echo "File 2" > file2.txt
git add .
git commit -m "fixing stuff"

echo "File 3" > file3.txt
git add .
git commit -m "last fix"
View your messy log:

git log --oneline -n 3
Step 2: Squash them with Interactive Rebase
We want to combine these 3 commits into 1. We look back 3 commits from HEAD:

Run the interactive rebase:

git rebase -i HEAD~3
Your terminal will open an editor (usually vim or nano) with a list of commits. It looks like this:

pick a1b2c3d work in progress
pick e4f5g6h fixing stuff
pick i7j8k9l last fix
Change pick to squash (or just s) for the commits you want to merge into the one above it:

pick a1b2c3d work in progress
squash e4f5g6h fixing stuff
squash i7j8k9l last fix
Save and exit the editor (Esc -> :wq in vim, or Ctrl+X, Y, Enter in nano).

A second editor window will open asking you to clean up the commit message. Delete all lines except for your new, professional title:

feat: Add infrastructure configuration files
Save and exit.

Step 3: Verify
Run your log again:

git log --oneline
You will now see only one commit where there used to be three!



Now one question why we squash the last 2 and not the first one:

pick a1b2c3d work in progress
squash e4f5g6h fixing stuff
squash i7j8k9l last fix

This is because:

In Git's rebase logic, the squash command means:

"Take this commit and combine/melt it into the commit directly above it."

If you mark the very first line as squash, Git will throw an error because there is no commit listed above it in that rebase window to melt into.

How the Hierarchy Works
Line 1 (pick a1b2c3d): Acts as the anchor / base commit. This is the foundation that survives.

Line 2 (squash e4f5g6h): Melts into Line 1 (a1b2c3d).

Line 3 (squash i7j8k9l): Melts into the combination of Lines 1 & 2.

Plaintext
[pick a1b2c3d]   <── Foundation (survives)
      ▲
      ├── [squash e4f5g6h] (melts up into a1b2c3d)
      ▲
      └── [squash i7j8k9l] (melts up into a1b2c3d)
At the end of the process, Git combines all the code changes from all three commits into one single commit and lets you write a single, clean commit message to replace all three.