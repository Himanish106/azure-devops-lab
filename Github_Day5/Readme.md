Part 1: What is a Pull Request?

In an enterprise environment, developers never push directly to main. Direct pushes bypasses tests and code audits, increasing the risk of breaking production infrastructure. So the work flow is like this:

a) Make changes on your development branch (dev).
b) Push dev to Github
c) Open a Pull Request (PR) asking permission to merge dev into main
d) Peer engineers review the difference, comment on lines of code, and approve it.
e) Automated CI/CD pipelines runs the test.
f) The peer is merged directly on Github.

Local Machine:    [dev] ──(git push)──> GitHub: [origin/dev]
                                                    │
                                          [Open Pull Request]
                                                    │
                                            [Peer Code Review]
                                                    │
                                            [Merge into main]
                                                    │
Local Machine:    [main] <──(git pull)── GitHub: [origin/main]


Part 2: Hands-on: Create and Merge a Pull Request

Step 1: Make a New Update on devSwitch to your dev branch: git switch dev

Open deploy.sh in VS Code and add a line for Azure Subnet configuration:

echo "Deploying Azure Infrastructure..."
echo "Creating Resource Group: rg-production-eastus"
echo "Creating Virtual Network: vnet-DEV-TESTING-eastus"
echo "Creating Subnet: snet-app-eastus"

Save the file (Ctrl + S), stage, commit, and push to GitHub:

git add deploy.sh
git commit -m "feat: Add subnet creation to deploy script"
git push origin dev

Step 2: Open the Pull Request on GitHub

Open your browser and go to your repository on GitHub.You will see a yellow banner:
dev had recent pushes... Compare & pull request. Click that button.
(If you don't see the banner, go to the Pull requests tab ----> click New pull request).

Set the branch targets:

base: main (the destination where code will go)
compare: dev (the source containing your new changes)
Title: feat: Add subnet deployment configuration
Description: Adds application subnet definition to deploy.sh for Azure infrastructure rollout.

Click Create pull request.

Step 3: Inspect Diff and Merge the PRIn the PR view, click on the Files changed tab at the top.

Notice how GitHub highlights the new green line (+ echo "Creating Subnet: snet-app-eastus").

Go back to the Conversation tab.Click the green button Merge pull request ----> Confirm merge.Now main on GitHub has your new subnet code, but your local laptop's main branch does not have it yet.



Difference between git fetch and git pull:

The core difference is that git fetch only downloads data from the remote repository without changing your local files, whereas git pull downloads the data and immediately merges it into your current working branch. Essentially, git pull is a shortcut command that performs a git fetch followed by a git merge.


![alt text](image.png)


Method 1: Safe Inspection with git fetch
git fetch downloads all changes, branches, and commits from GitHub to your laptop without modifying any of your local files.

git switch main
Run git fetch:

git fetch origin
Inspect how many commits origin/main is ahead of your local main:


git status

Output:

On branch main
Your branch is behind 'origin/main' by 2 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)
To preview the exact lines GitHub has that your local machine doesn't:

git diff main origin/main
Method 2: Applying Changes with git pull
Once you know the incoming changes are safe, update your local branch:

git pull origin main
Expected Output:

Updating ...
Fast-forward
 deploy.sh | 1 +
 1 file changed, 1 insertion(+)
Check deploy.sh in VS Code—it now contains the snet-app-eastus line!