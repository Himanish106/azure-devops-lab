In this workflow, you keep main strictly for stable production-ready code and do all your daily work,testing and additions inside dev.

Step 1: Verify your starting state on main

Ensure you are starting on your main branch with a clean working directory:

git switch main
git status

Expected Output:

On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean


Step 2: Create and Switch to the dev branch: Create the dev branch off main and switch to it immediately:

git switch -c dev

What this does: Copies your entire current main state into a new branch named dev and places you on dev.

-c ---> This stands for create

Verify with git branch:

Expected Output:

* dev
  main



Step 3: Publish the dev Branch to GitHub
Now, establish upstream tracking for dev on your remote repository:

git push -u origin dev



Now if you go to github you will see two seperate branches has been created. If you switch branches from the portal you will see whatever existed in main previously has also been copied to dev. The question stands how?

When you run git switch -c dev while standing on main, Git creates dev as an exact carbon copy of main at that precise moment in time.Every single file, folder, commit history line, and code change that existed in main is immediately present in your new dev branch.

How it works behind the scenes?

How It Works Behind the Scenes
Think of a branch in Git not as a heavy folder full of copied files, but as a lightweight pointer (or label) pointing to a specific commit:

Commit 1 ──> Commit 2 ──> Commit 3
                            ▲
                       main │ dev  <-- Both point to the exact same history!
Before step 2, main points to Commit 3.

When you run git switch -c dev, Git creates a new label named dev and points it to Commit 3 as well.

Therefore, everything in main is inside dev.





Step 4: Make & Commit Changes on dev
Now that you are on dev, let's add new infrastructure code.

Open deploy.sh in VS Code.

Add a third line for Virtual Network creation:

echo "Deploying Azure Infrastructure..."
echo "Creating Resource Group: rg-production-eastus"
echo "Creating Virtual Network: vnet-production-eastus"
Save the file (Ctrl + S).

Stage and commit the change on dev:

git add deploy.sh
git commit -m "feat: Add virtual network creation script"
Push the updated dev branch to GitHub:

git push