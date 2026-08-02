Day 3 Overview: Connecting Local Git to GitHub

Up until today, everything has been saved locally on your laptop inside .git. Today, we establish the bridge between your machine and GitHub (the cloud host) using industry-standard security.

Part 1: Create the Remote Repository on GitHub
1) Log into your account at GitHub.

2) In the top-right corner, click the + icon and select New repository.

3) Fill in the repository details:

   a) Repository name: azure-devops-lab

   b) Description: Azure Infrastructure and DevOps Sandbox

   c) Public / Private: Choose Private (recommended for infrastructure code) or Public.

   d) IMPORTANT: Leave "Add a README file", "Add .gitignore", and "Choose a license" UNCHECKED.

Why keep them unchecked? You already have a local repository with a history, .gitignore, and files. If you initialize GitHub with a README, the remote and local histories will diverge, causing a non-fast-forward merge conflict on your first push.

 4) Click Create repository.



 Part 2: Set Up Authentication (Personal Access Token)

 GitHub disabled raw password authentication for Git terminal operations. You must use either a Personal Access Token (PAT) or SSH Keys. We will use a PAT (HTTPS) as standard for pipeline service connections and initial setup.

 Generating your PAT:

 1. On GitHub, click your Profile Picture (top right) ----> Settings.
 2. Scroll to the bottom of the left sidebar and click Developer settings.
 3. Select Personal access tokens ----> Tokens (classic).
 4. Click Generate new token ----> Generate new token (classic).
 5. Set:
    a) Note: VS Code Local - Laptop
    b) Expiration: 30 days (or custom)
    c) Scopes (Permissions): Check the box for repo (Full control of private repositories).
 6. Generate Token
 7. CRITICAL: Copy the generated token immediately and save it in a secure password manager. You will not see it again.




 Part 3: Linking Local to Remote (git remote)

 Now, open your VS Code terminal inside your project directory (azure-devops-lab/Github_Day1_Day2 or root directory).


 Step 1: Add the remote URL:Run the following command, replacing <YOUR-USERNAME> with your actual GitHub username:

 git remote add origin https://github.com/<YOUR-USERNAME>/azure-devops-lab.git 

 Breakdown of this command:
 a) git remote add: Tells Git to store a pointer to a remote server.

 b) origin: The conventional alias name given to your primary remote server.

 c) https://...: The exact URL pointing to your cloud repository.

 Step 2: Verify the remote link

 To confirm that the git stored the URL correctly run:

 git remote -v

 Expected Output:

 origin  https://github.com/<YOUR-USERNAME>/azure-devops-lab.git (fetch)
 origin  https://github.com/<YOUR-USERNAME>/azure-devops-lab.git (push)

 Pro-Tip / Inspection Command:
 If you ever make a typo in the URL, fix it using:
 git remote set-url origin <CORRECT-URL>
 If you want to view detailed configuration for the remote, run:
 git remote show origin





 Part 4: Renaming Default Branch & First Push (git push)

 Step 1: Ensure Your Primary Branch is Named main
Check your current branch name:
git branch
If it says * master, rename it to industry-standard main:
git branch -M main

Step 2: Push your Local commits to Github

git push -u origin main

What happens next:
1) Terminal/OS will prompt for credentials:

  a) Username: Enter your GitHub username.

  b) Password: Paste your Personal Access Token (PAT) (NOT your account password).

2) Git uploads all 3 commits, tags, and object files to GitHub.

Breakdown of git push -u origin main:
 a) push: Uploads local commits to the remote.

 b) -u (or --set-upstream): Links local branch main directly to remote branch main on origin. This is crucial. After running this once, you can simply type git push or git pull in the future without specifying arguments.

 c) origin: Target remote destination.

 d) main: The branch being pushed.

 Part 5: Verification and Day 3 inspection toolkit

 Once the push finishes, execute these commands to inspect the updated remote relationship:

1. Verify Local vs Remote Branches
git branch -a
Shows local branches AND remote tracking branches (remotes/origin/main).

If your output is:

* main
  remotes/origin/main

Line 1: main is your local branch.
The * means you are currently on this branch (your current working branch).
Think of it as:

"I'm currently working on my local main branch."


Line 2: remotes/origin/main:

This is not another branch you work on directly.

It is Git's local record of the main branch on the remote repository.

Breaking it down:

origin = the remote repository (usually GitHub)
main = the branch on that remote

So it means: "The remote repository (origin) has a branch called main."

2. Verify Push Status
git status
Expected output: Your branch is up to date with 'origin/main'. Nothing to commit, working tree clean.

3. Check GitHub Web UI
Go to [https://github.com/](https://github.com/)<YOUR-USERNAME>/azure-devops-lab. Refresh the page.

You should see your files (deploy.sh, .gitignore, Readme.md), your commit messages, and total commit count!