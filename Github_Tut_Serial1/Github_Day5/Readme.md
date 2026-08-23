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



There are three critical rulesets and governance features that you should know how to configure:

1. Implementing CODEOWNERS

In enterprise teams, certain files (like cloud infrastructure scripts, security policies, or pipeline YAML files) should never be merged without the explicit approval from the domain lead. Github automates this using a special file called CODEOWNERS.

STEP 1: Create the .github Directory and CODEOWNERS File
Make sure you are working on your dev branch in your VS Code terminal:

git switch dev

a. Create a directory named .github at the root of your project

mkdir .github

b. Create an empty file named CODEOWNERS (no file extension) inside .github:


STEP 2: Add Ownership Rules

Open .github/CODEOWNERS in VS code and paste the following configurations:

# Catch-all: By default, assign Himanish106 to review everything in the repo
* @Himanish106

# Specifically assign Himanish106 for any shell deployment scripts
*.sh @Himanish106

# Specifically assign Himanish106 for any changes to GitHub configurations/workflows
/.github/ @Himanish106



STEP 3: Stage, Commit, and Push to devCheck your status to verify the new file is tracked:

git status

Stage the new file:git add .github/CODEOWNERS

Commit the change:git commit -m "chore: 

Add CODEOWNERS file for PR review automation"
Push to your remote dev branch:git push origin dev

Step 4: Promote to main via Pull RequestTo make these rules take effect for the default branch:

Go to your repository on GitHub.Open a Pull Request (base: main ----> compare: dev).
Title: chore: Configure CODEOWNERS governance
Click Create pull request and merge it into main.
Switch back to your terminal, checkout main, and pull down the merge:
git switch main
git pull origin main



2. Enabling Secret Scanning Push Protection:

DevOps engineers frequently handle Azure connection strings, service principal keys, and client secrets. Accidental commits of credentials can cause severe security breaches.

How to Enable It:

a. Go to your repository on GitHub ----> Settings.
b. In the left sidebar, click Code security and analysis (under Security).
c. Look for Secret scanning and click Enable.
d. Right below it, check the box to enable Push protection.

What Happens in Practice:
If you or anyone on your team attempts to run git push containing an Azure Subscription ID key, AWS secret, or GitHub PAT, Git will abort the push right in your terminal with a security block message before the commit ever reaches the remote server.



Intentionally Trigger a Blocked Push

In VS Code, make sure you are on dev:

git switch dev
Open deploy.sh and add a dummy GitHub Personal Access Token format string at the bottom:

# Test Secret
DUMMY_TOKEN="ghp_ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
Save the file, stage, and commit it:

git add deploy.sh
git commit -m "test: Add dummy API token for secret scan test"
Attempt to push:

git push origin dev




Step 3: Enable "Require Conversation Resolution"

To finish the second hands-on setting:

a) On GitHub, go to your repository Settings ----> Rules ----> Rulesets (or Branches).

b) Click on your Protect Main ruleset (or create one targeting main).

c) Under Branch rules / Pull request rules, check:Require conversation resolution before merging

Click Save changes.

If an interviewer or senior engineer asks about PR quality gates, you only need to say:

"We enforce Conversation Resolution on our protected main branch ruleset so no pull request can be merged until all review comments and questions are resolved."