Lets imagine a scenario:

You are an IT professional working on a feature or configuration.You are in the middle of writing complex Azure infrastructure code. Suddenly, there is a production outage that requires a quick fix on the main branch.

You cannot git switch main because your uncommitted changes in deploy.sh would be overwritten or cause conflicts. You also cannot git commit because your code is broken and unfinished.

git stash solves this by taking your uncommitted changes (both staged and unstaged) and moving them into a safe local storage area leaving your directory clean so you can switch branches.


Let's simulate this scenario.

Step 1: Create an "Unfinished" Feature
Ensure you are on dev:

git switch dev
Open deploy.sh and add a line of "work in progress" code:

echo "Adding incomplete firewall configuration..." >> deploy.sh
Check the status:

git status
You see deploy.sh is modified, but not committed.

Step 2: The Emergency
You need to switch to main to fix a typo, but Git blocks you. Try it:

git switch main
Git will likely refuse to switch because you have uncommitted changes.

Step 3: Stash Your Work
"Park" your work in the stash:

git stash
Your directory is now clean!

git status
Now, switch to main safely:

git switch main
Step 4: Fix the "Production Bug"
Perform your emergency fix on main:

echo "Fixing critical production typo" >> deploy.sh
git add deploy.sh
git commit -m "fix: Hotfix production configuration typo"
git push origin main
Step 5: Restore Your Work
Switch back to dev:

git switch dev
Retrieve your unfinished work:

git stash pop


In real-world DevOps, you might have multiple stashed items.

git stash: Saves current changes and resets the directory.

git stash list: Shows all your saved stashes.

git stash pop: Restores the most recent stash and removes it from the list.

git stash apply: Restores the stash but keeps it in the list (useful if you need to apply the same change to multiple branches).

git stash drop: Deletes the most recent stash without applying it.